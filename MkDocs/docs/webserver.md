# Webserver
## Apache
Anzahl der aktuell genutzten Worker pro Domain anzeigen

```bash
apachectl fullstatus | grep http | awk -F " " '{print $14}' | sort -n | uniq -c |sort -napachectl fullstatus | grep http | awk -F " " '{print $14}' | sort -n | uniq -c |sort -n
```

MaxWorkers Fehler auslesen

```bash
grep MaxRequestWorkers /usr/local/apache/logs/error_log | grep -v 'not at'grep MaxRequestWorkers /usr/local/apache/logs/error_log | grep -v 'not at'
```

Aktive Apache-Verbindungen anzeigen

```bash
netstat -an | egrep ':80|:443' | egrep '^tcp' | grep -v LISTEN | awk '{print $5}' | egrep '([0-9]{1,3}\\\\.){3}[0-9]{1,3}' | sed 's/^\\\\(.*:\\\\)\\\\?\\\\(\\\\([0-9]\\\\{1,3\\\\}\\\\.\\\\)\\\\{3\\\\}[0-9]\\\\{1,3\\\\}\\\\).*$/\\\\2/' | sort | uniq -c | sort -nr | sed 's/::ffff://' | head -20netstat -an | egrep ':80|:443' | egrep '^tcp' | grep -v LISTEN | awk '{print $5}' | egrep '([0-9]{1,3}\\\\.){3}[0-9]{1,3}' | sed 's/^\\\\(.*:\\\\)\\\\?\\\\(\\\\([0-9]\\\\{1,3\\\\}\\\\.\\\\)\\\\{3\\\\}[0-9]\\\\{1,3\\\\}\\\\).*$/\\\\2/' | sort | uniq -c | sort -nr | sed 's/::ffff://' | head -20
```