---
layout: post
title:  "使用C#验证IOS9产生的ECDSA签名"
date:   2016-04-15 08:08:08 +0800
categories: develop ios
excerpt: 本文主要介绍了在C#中使用ECDsaCng验证IOS9产生的ECDSA签名，其中也简单介绍了ASN1的一些知识。
---

### 前言

在[IOS9中使用KeychainTouchID](http://idivines.com/develop/ios/2016/04/05/IOS9-KeychainTouchID.html) 一文中简单介绍了服务器如何参与指纹认证的流程，以及使用openssl如何验证IOS9产生的ECDSA签名。本文将着重介绍在服务器中使用C#验证IOS9产生的ECDSA签名数据。

### 公钥格式说明

1. SecGenerateKeyPair 产生密钥对，其中公钥固定为65个字节，第一个字节固定为0x04,然后是64个字节的真正公钥数据。

2. C#中 ECDSA 算法有 ECDsaP256, ECDsaP384, ECDsaP521 三种，产生的公钥长度分别是 72 Bytes, 104Bytes, 140Bytes 其中最前面的8个字节是附加字节。

3. 附加加字节的前4字节表示 “ECS1”、“ECS3”、“ECS5” 三种类型，后4字节表示公钥长度。我们用到的是256位的 ECDsaP256 算法，前面8个字节是 0x45,0x43,0x53,0x31,0x20,0x00,0x00,0x00 ，前面4个字节 0x45,0x43,0x53,0x31 表示 “ECS1” ,后面4个字节 0x20,0x00,0x00,0x00 表示公钥是256位。

4. 根据上面两点我们就可以把 SecGenerateKeyPair 产生的公钥用到C#中，具体如下:

		//这个是SecGenerateKeyPair产生的公钥    
		byte[] pubKeyForIos = {
		                       0x04,0xa1,0xe1,0xd1,0xdd,0xad,0xd5,0xbb,0x05,0xc8,0xce,0x29,0x8c,0xe4,0x27,0x02,
                               0x2e,0x3a,0x8b,0x0e,0xaf,0x51,0x5c,0x5d,0x16,0x49,0xab,0x7e,0x08,0xdf,0x32,0x30,
		                       0xb8,0x57,0xdc,0x48,0x13,0x18,0x3a,0x71,0x11,0xd4,0x60,0x32,0xd1,0x78,0xa4,0x4d,
		                       0x09,0xf1,0x10,0x00,0xbd,0xae,0x02,0x9a,0xfd,0xec,0x73,0x23,0x1e,0x63,0x4c,0xc9,0xc1
		                      };


		//将第一个字节 0x04 换成 0x45,0x43,0x53,0x31,0x20,0x00,0x00,0x00    
		byte[] pubkeyForCSharp = {
								  0x45,0x43,0x53,0x31,0x20,0x00,0x00,0x00,
								  0xa1,0xe1,0xd1,0xdd,0xad,0xd5,0xbb,0x05,0xc8,0xce,0x29,0x8c,0xe4,0x27,0x02,
		                          0x2e,0x3a,0x8b,0x0e,0xaf,0x51,0x5c,0x5d,0x16,0x49,0xab,0x7e,0x08,0xdf,0x32,0x30,
		                          0xb8,0x57,0xdc,0x48,0x13,0x18,0x3a,0x71,0x11,0xd4,0x60,0x32,0xd1,0x78,0xa4,0x4d,
		                          0x09,0xf1,0x10,0x00,0xbd,0xae,0x02,0x9a,0xfd,0xec,0x73,0x23,0x1e,0x63,0x4c,0xc9,0xc1
		                         };


### 签名数据格式说明

1. 256位的ECDSA签名结果其实就是两个很大的整数r和s，由于普通的数据类型不够，所以就用数组来表示，r和s都是32个字节的长度，总共是64个字节。

2. C#中使用 ECDsaCng.SingData() 来签名数据，结果是64个字节,是原始的两个整数。

2. SecKeyRawSign 签名结果大于64个字节，是由于采用了ASN1编码，而且结果有 70bytes，71bytes，72bytes 三种情况。

4. 这里我们需要对SecKeyRawSign 签名的结果进行解码，这里需要一点ASN1编码的知识,具体如下：

        //SecKeyRawSign签名的结果
		byte[] signForIos = new byte[]{0x30,0x45,0x02,0x20,0x6C,0x80,0x22,0xE4,0xA5,0x89,0x8A,0x81,0xE9,0x82,0x0C,0x13,
		                               0xC6,0xE7,0x09,0x95,0xC9,0x1C,0x4F,0x00,0x32,0x13,0x59,0xB2,0x2D,0xD3,0xEF,0xE4,
		                               0x9D,0xDD,0x3B,0xAD,0x02,0x21,0x00,0xF0,0x84,0x9E,0x2E,0xB6,0x01,0x3B,0x2A,0x81,
		                               0xB0,0x36,0x83,0x51,0x7F,0x20,0x56,0xD7,0xFB,0xD6,0x9B,0x76,0x38,0x39,0xF8,0xD2,
		                               0x89,0xED,0x32,0x24,0x0D,0xA1,0x25};

        //第1字节：0x30表示一种通用的格式
        //第2字节：0x45表示后面跟了69个字节的数据
        //第3字节：0x02表示后面是一个整数
        //第4字节：0x20表示整数长度是32个字节
 
        //第35字节：0x02表示后面是个整数
        //第36字节：0x21表示整数长度是33个字节
        //第37字节：0x00需要忽略掉，这个是由于ASN1编码产生的，  

        byte[] integer1 = {
                           0x6C,0x80,0x22,0xE4,0xA5,0x89,0x8A,0x81,0xE9,0x82,0x0C,0x13,0xC6,0xE7,0x09,0x95,
                           0xC9,0x1C,0x4F,0x00,0x32,0x13,0x59,0xB2,0x2D,0xD3,0xEF,0xE4,0x9D,0xDD,0x3B,0xAD
                          };

        byte[] integer2 = {
					       0xF0,0x84,0x9E,0x2E,0xB6,0x01,0x3B,0x2A,0x81,0xB0,0x36,0x83,0x51,0x7F,0x20,0x56,
					       0xD7,0xFB,0xD6,0x9B,0x76,0x38,0x39,0xF8,0xD2,0x89,0xED,0x32,0x24,0x0D,0xA1,0x25
                          };

      
        //最后把integer1和integer2合起来就可以给C#使用了
        byte[] signForCSharp = {
						        0x6C,0x80,0x22,0xE4,0xA5,0x89,0x8A,0x81,0xE9,0x82,0x0C,0x13,0xC6,0xE7,0x09,0x95,
						        0xC9,0x1C,0x4F,0x00,0x32,0x13,0x59,0xB2,0x2D,0xD3,0xEF,0xE4,0x9D,0xDD,0x3B,0xAD,
						        0xF0,0x84,0x9E,0x2E,0xB6,0x01,0x3B,0x2A,0x81,0xB0,0x36,0x83,0x51,0x7F,0x20,0x56,
						        0xD7,0xFB,0xD6,0x9B,0x76,0x38,0x39,0xF8,0xD2,0x89,0xED,0x32,0x24,0x0D,0xA1,0x25
						       };

### 备注
1. IOS中计算 ECDSA-SHA256 是自己先计算 SHA256 然后把哈希后的结果传给 SecKeyRawSign 
2. C#中计算 ECDSA-SHA256 是把原始的数据传给 ECDsaCng.VerifyData()



