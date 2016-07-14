---
layout: post
title:  "常用开源许可类型及应用建议"
date:   2016-07-14 08:08:08 +0800
categories: Document
excerpt: 本文主要介绍常用开源许可类型及应用建议。
---

 <body link="blue" vlink="purple">
  <table width="720" border="1" cellpadding="4" cellspacing="4" style='width:511.50pt;border-collapse:collapse;table-layout:fixed;'>
   <col width="72" style='width:54.00pt;'/>
   <col width="100" style='mso-width-source:userset;mso-width-alt:3200;'/>
   <col width="100" style='mso-width-source:userset;mso-width-alt:2816;'/>
   <col width="279" style='mso-width-source:userset;mso-width-alt:8928;'/>
   <col width="143" style='mso-width-source:userset;mso-width-alt:4576;'/>
   <tr height="18" style='height:13.50pt;'>
    <td height="18" width="72" style='height:13.50pt;width:54.00pt;font-weight:bold;' >类别</td>
    <td width="100" style='width:75.00pt;font-weight:bold;' >许可证名称</td>
    <td width="88" style='width:70.00pt;font-weight:bold;' >典型项目</td>
    <td width="279" style='width:209.25pt;font-weight:bold;' >许可证说明</td>
    <td width="143" style='width:107.25pt;font-weight:bold;' >应用建议</td>
   </tr>
   <tr height="74.67" style='height:56.00pt;mso-height-source:userset;mso-height-alt:1120;'>
    <td height="140.67" rowspan="3" style='height:105.50pt;border-right:none;border-bottom:none;' >BSD类</td>
    <td >Apache V2.0</td>
    <td >ACE Tomcat</td>
    <td >1、允许各种链接，无开源义务。<br/>2、允许修改，无开源义务。 <br/>3、软件所有人授予专利许可。</td>
    <td rowspan="3" style='border-right:none;border-bottom:none;' >推荐使用！ <font class="font0"><br/>商业友好型许可证</font></td>
   </tr>
   <tr height="18" style='height:13.50pt;'>
    <td >BSD</td>
    <td >FreeBSD</td>
    <td rowspan="2" >1、允许各种链接，无开源义务。 <br/>2、允许修改，无开源义务。 <br/>3、无专利规定。</td>
   </tr>
   <tr height="48" style='height:36.00pt;mso-height-source:userset;mso-height-alt:720;'>
    <td >MIT</td>
    <td >ASN.1</td>
   </tr>
   <tr height="36" style='height:27.00pt;mso-height-source:userset;mso-height-alt:540;'>
    <td class="xl70" height="154.67" rowspan="4" style='height:116.00pt;border-right:none;border-bottom:none;' >MPL类</td>
    <td >CPL V1.0</td>
    <td >JUnit</td>
    <td rowspan="4" >1、允许各种链接，无开源义务。 <br/>2、允许修改，但修改部分需要开源。 <br/>3、截取部分或全部开源代码与私有源代码混合将视为对原始软件的修改从而导致私有代码开源。<br/>4、软件所有人授予专利许可。</td>
    <td rowspan="4" >可以使用！<font class="font0"> <br/>但关注修改后对应的开源义务</font></td>
   </tr>
   <tr height="38.67" style='height:29.00pt;mso-height-source:userset;mso-height-alt:580;'>
    <td >EPL V1.0</td>
    <td >Eclipse</td>
   </tr>
   <tr height="36" style='height:27.00pt;mso-height-source:userset;mso-height-alt:540;'>
    <td >MPL V1.0</td>
    <td >FireFox</td>
   </tr>
   <tr height="44" style='height:33.00pt;mso-height-source:userset;mso-height-alt:660;'>
    <td >CDDL V1.0</td>
    <td >OpenSolaris</td>
   </tr>
   <tr height="304" style='height:228.00pt;mso-height-source:userset;mso-height-alt:4560;'>
    <td height="458.67" rowspan="2" style='height:344.00pt;border-right:none;border-bottom:none;' >GPL类</td>
    <td >LGPL V2</td>
    <td >Jboss <br/>OpenOffice <br/>UcLibc <br/>SDL</td>
    <td >1、允许各种链接，动态链接无开源义务，静态链接需要开放与之链接私有软件的.o文件和makefile. <br/>2、允许修改再链接到私有软件，但是修改增加的功能实现不能依赖私有软件的数据或功能。 <br/>3、允许不受限制的使用头文件中的numerical parameters（数值参数）, data structure layouts（数据结构布局） and accessors（存取）, or small macros（小宏）, inline functions（内联参数） and templates (ten or fewer lines in length)（十行以内的模板） <br/>4、仅原则性声明专利应免费许可，无详细规定</td>
    <td >慎重使用！<font class="font0"> <br/>只允许动态链接方式使用</font></td>
   </tr>
   <tr height="154.67" style='height:116.00pt;mso-height-source:userset;mso-height-alt:2320;'>
    <td >GPL V2</td>
    <td >Linux</td>
    <td >1、允许各种链接，但被链接的整个产品需要开源。 <br/>2、允许修改，但被修改部分及整个产品均需要开源。 <br/>3、通过pipes, sockets和命令行参数与GPL软件进行通信，不会导致私有软件被传染。 4、仅原则性声明专利应免费许可，无详细规定。</td>
    <td >不建议使用！<font class="font0"> <br/>由于可能导致产品整体负有开源义务，不建议使用</font></td>
   </tr>
  </table>
