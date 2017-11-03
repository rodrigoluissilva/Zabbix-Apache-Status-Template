# **Zabbix-Apache-Status-Template**

>Template to Show Apache Statistics Using mod_status

Enable the Status module

    a2enmod status

Add this config to your Apache settings

    <IfModule mod_status.c>
    
      <Location /server-status>
        SetHandler server-status
        Require local
        Require ip 127.0.0.1 ::1 10.10.10.10
      </Location>
    
      ExtendedStatus On
    
    </IfModule>
    
Put zbxApacheStatusCheck script on the webserver, edit it to change the defaults (at least zabbixserver needs to be changed).
Schedule it using cron, because is uses zabbix-sender.

In case of problems, try running ./zbxApacheStatusCheck -d or ./zbxApacheStatusCheck --help

- Applications 3
- Items 28
- Triggers 5
- Graphs 6
- Screens 1
