# H2 - Logs

## Read and summarize

### Pyramid of Pain

### Diamond Model

## Apache log. Install Apache, visit the website and analyze logs from the visit.

I installed apache from the official package repo

	sudo apt-get install apache2

After I headed to localhost with firefox

To view the logs i used 

	cat /var/log/apache2/..

I encountered an error that I didn't have permissions for the the logs, I added my own user to the www-data group

	sudo usermod -a -G www-data emppu

After which I logged out and logged back in so the added group would be in effect. It still didn't work so I checked the permissions on the apache2 logs folder 

![h2/lsla.png](h2/lsla.png)

It seems that the logs aren't controlled by the www-data group but instead 'adm' group so I added myself to the 'adm' group, and relogged again for the changes to be in effect.

	sudo usermod -a -G adm emppu

After making these changes I could finally view the logs(without using Sudo or running as Root.)

	cat /var/log/apache2/access.log

![h2/apachelogs.png](h2/apachelogs.png)

From the access logs you can view the IP 127.0.0.1, time and date 06/Apr/2025:11:42:21 +0300, request type in this case /GET and which protocol HTTP, the response status 200=OK, 3380 is the response size so the size of the data that gets transmitted to the user without the response headers(Apache HTTP Server Version 2.4. Log Files). Then at the end of the row you can view the User-Agent which tells us which type of browser, OS and which version of the browser.

Another command I typically use to view logs is:

	tail -f /var/log/apache2/(log file)

It shows couple of the last rows and with the parameter -f it will follow the logs as they get updated.

![h2/tail.GIF](h2/tail.GIF)

## Nmapped. Scan ports from the web-server.

I installed Nmap from the package repo

	sudo apt-get install nmap

I disconnected my computer from the internet and scanned localhost

	nmap localhost

![h2/nmaplocalhost.png](h2/nmaplocalhost.png)

I can see that I have SSH, HTTP - Apache and IPP services running, IPP is related to the printing services.  

## Scripts. Which scripts were enabled then using -A parameter with Nmap?

-A parameter enables OS and version detection, script scanning and traceroute(Nmap documentation. Chapter 15. Nmap Reference Guide).

![h2/nmapa.png](h2/nmapa.png)

## Search the Apache logs for Nmap

	cat /var/log/apache2/access.log

![h2/nmaplogs.png](h2/nmaplogs.png)

## Capture the Nmap port scan with Wireshark

## Capture the traffic via Ngrep

## Change the User-Agent for Nmap

### How did the logs change after changing the User-Agent

### Change the Nmap to be even more stealthy

### Change some other Nmap script to be stealthy

 
## Sources:


Karvinen, T. 2025. Network Attacks and Reconnaissance. Available at: [https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/)

Bianco. 2013. The Pyramid of Pain. Available at: [https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html](https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html) 

Caltagirone et. al. 2020. Diamond Model. Available at: [https://www.threatintel.academy/wp-content/uploads/2020/07/diamond-model.pdf](https://www.threatintel.academy/wp-content/uploads/2020/07/diamond-model.pdf)

Apache HTTP Server Version 2.4. Log Files Documentation. Available at: [https://httpd.apache.org/docs/current/logs.html](https://httpd.apache.org/docs/current/logs.html)

Nmap documentation. Chapter 15. Nmap Reference Guide. Available at: [https://nmap.org/book/man.html](https://nmap.org/book/man.html)

Nmap documentation. Port Scanning Techniques. Available at: [https://nmap.org/book/man-port-scanning-techniques.html](https://nmap.org/book/man-port-scanning-techniques.html)
