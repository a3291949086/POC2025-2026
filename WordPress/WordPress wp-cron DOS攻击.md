## WordPress wp-cron DOS攻击

攻击者可以通过频繁访问 _wp-cron.php_，导致服务器资源被大量占用，从而引发拒绝服务（DoS）攻击。

## FOFA
```
body="wp-cron.php"
```

## POC 
```
直接访问wp-cron.php，如果成功访问则可能存在该漏洞
```


## Nuclei
```
id: wordpress-user-enumeration

info:
  name: WordPress REST API User Enumeration
  author: beichu
  severity: medium
  description: |
    Detects WordPress user enumeration via REST API
  tags: wordpress,wp,info-leak,user-enum

requests:
  - method: GET
    path:
      - "{{BaseURL}}/wp-cron.php"

    matchers-condition: and
    matchers:
      - type: status
        status:
          - 200
```