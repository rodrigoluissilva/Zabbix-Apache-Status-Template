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

- Applications 3
- Items 28
- Triggers 5
- Graphs 6
- Screens 1
