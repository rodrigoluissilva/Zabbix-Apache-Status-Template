# **Zabbix-Apache-Status-Template**

>Template to Show Apache Statistics Using mod_status

- Applications 3
- Items 28
- Triggers 5
- Graphs 6 (screenshots bellow)
- Screens 1

**Instructions based on Ubuntu/Debian**

Enable the Status module

`a2enmod status` 

To allow the Zabbix server to get the data, change the file `/etc/apache2/mods-available/status.conf`
And add the line `Require ip <zabbix.server.ip.address>`

```
<IfModule mod_status.c>

  <Location /server-status>
    SetHandler server-status
    Require local
    Require ip 127.0.0.1 ::1 10.10.10.10
  </Location>

  ExtendedStatus On

</IfModule>
```

Restart the Apache Web Server or reload the configuration

`apache2ctl graceful`

Copy the script `zbxApacheStatusCheck` to your Zabbix server or some other machine and change the permission to enable execution `chmod +x zbxApacheStatusCheck`

You have to set a cron job to periodically collect statistics and send it to your Zabbix server

```
# it will run every minute

# The parameter --zabbixsource define the name of the host on your Zabbix
* * * * * /etc/zabbix/scripts/zbxApacheStatusCheck --host=10.20.1.231 --zabbixsource="My Web Server 1" --zabbixserver=10.20.1.250

# Password with special character has to be escaped 
# Password = %P@$$0rd?
* * * * * * /etc/zabbix/scripts/zbxApacheStatusCheck --host=10.20.1.232 --zabbixsource="My Web Server 2" --zabbixserver=10.20.1.250 --user=test123 --passwd='%P@\$\$0rd?'

```

In case of problems, try running `./zbxApacheStatusCheck -d` or `./zbxApacheStatusCheck --help`

Options available as parameters

```
Options:
  --version             show version and exit

  -h, --help            show this help page and exit

  -l URL, --url=URL     Define a custom URL to use
                        When you use URL, the options
                        --host, --port, --proto, --user and --passwd are not used
                        [--url=http://localhost/my-server-status?auto]
                        [default: None]

  -o HOST, --host=HOST  Apache host to collect info.
                        [default: localhost]

  -p PORT, --port=PORT  Apache port to collect info
                        [default: 80]

  -r PROTO, --proto=PROTO
                        Protocol to use. [http or https]
                        [default: http]

  --TIMEOUT=seconds     Connection timeout
                        [default: 30]

  -z ZABBIXSERVER, --zabbixserver=ZABBIXSERVER
                        Hostname or IP address of Zabbix server
                        [default: localhost]

  -u USER, --user=USER  HTTP authentication user
                        [default: None]

  -a PASSWD, --passwd=PASSWD
                        HTTP authentication password
                        [default: None]

  -s SENDERLOC, --sender=SENDERLOC
                        Path of zabbix_sender binary
                        [default: /usr/bin/zabbix_sender]

  -q ZABBIXPORT, --zabbixport=ZABBIXPORT
                        Specify port number of Zabbix server
                        trapper running on the server
                        [default: 10051]

  -c ZABBIXSOURCE, --zabbixsource=ZABBIXSOURCE
                        Specify host name the item belongs to
                        (as registered in Zabbix frontend)
                        [default: localhost]

  -k CONFIGFILE, --config=CONFIGFILE
                        Specify the file with the list of statistics to collect
                        [default: run ./zbx --dumpconfig]

  --dumpconfig          Show the atual config

  -d, --debug           Run using Debug mode

  -t, --test            Do not send data, simply test, use with --debug
```


## Screenshots


Zabbix Apache Workers Overview Graph
![Zabbix Apache Workers Overview Graph](https://image.prntscr.com/image/DPdJMyXGQHaF_9AM7v7dWw.png)

Zabbix Apache Workers Busy Graph
![Zabbix Apache Workers Busy Graph](https://image.prntscr.com/image/uIvOkTVeSWukjilAuRIN1Q.png)

Zabbix Apache Requests Graph
![Zabbix Apache Requests Graph](https://image.prntscr.com/image/9qFhS-CWQqeNuFFAjGvvmQ.png)

Zabbix Apache CPU Utilization Graph
![Zabbix Apache CPU Utilization Graph](https://image.prntscr.com/image/EJADiiaKR5WG-EP8dDtHjA.png)

Zabbix Apache CPU Load Graph
![Zabbix Apache CPU Load Graph](https://image.prntscr.com/image/WLE-k4gqTAaHoLAHKER4Rg.png)

Zabbix Apache Access Graph
![Zabbix Apache Access Graph](https://image.prntscr.com/image/VqEG71F_TemH8HutlYxhVQ.png)
