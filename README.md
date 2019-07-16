# Useful Snippets
Some personal notes on a few all too easily forgotten commands.

## DNS
Query an MX record (example):
```bash
dig +short google.com mx

30 alt2.aspmx.l.google.com.
20 alt1.aspmx.l.google.com.
10 aspmx.l.google.com.
40 alt3.aspmx.l.google.com.
50 alt4.aspmx.l.google.com.
```

Having established a mail server name, lookup its IP address:
```bash
nslookup mail99.example.com

Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
Name:   mail99.example.com
Address: xx.xx.xx.xx
Name:   mail99.example.com
Address: xxx:xxx:xxx:xxx::xx
```

## Linux Misc.

Check details/availability of an uninstalled package:
```bash
sudo apt-get update
apt-cache showpkg <NAME OF PACKAGE>
```

## NMAP

Check if a specified port is open:
```bash
sudo nmap -sS -Pn -p 25 x.x.x.x
```
^- in this example, is port 25 listening?

## OpenSSL
Check certificate dates from a command line:
```bash
echo | openssl s_client -connect jonathancraddock.com:443 -servername jonathancraddock.com 2>/dev/null | openssl x509 -noout -dates
```

View available ciphers:
```bash
openssl ciphers -s -tls1_2
```
^- *in this example, list only the ciphers available if TLS1.2 is negotiated.*

For an A+ rating on the https://globalsign.ssllabs.com/ SSL checker, using only TLS v1.2 and with no "weak" cipher suites, I found I could enable:

* ECDHE-ECDSA-AES256-GCM-SHA384:
* ECDHE-RSA-AES256-GCM-SHA384:
* DHE-RSA-AES256-GCM-SHA384:
* ECDHE-ECDSA-CHACHA20-POLY1305:
* ECDHE-RSA-CHACHA20-POLY1305:
* DHE-RSA-CHACHA20-POLY1305:
* ECDHE-ECDSA-AES128-GCM-SHA256:

Generate Ephemeral Diffie-Hellman (DHE) parameters:
```bash
openssl dhparam -out /etc/ssl/certs/dhparam.pem 4096
```
^- *This is going to take a long time... around 5 mins on my $5 Digital Ocean droplet. Assuming you're wanting to include this in your Apache config, see syntax below.*
```bash
SSLOpenSSLConfCmd DHParameters "/etc/ssl/certs/dhparam.pem"
```

## Powershell
See when an AD user last changed their password:
```powershell
get-aduser "firstname.lastname" -properties PasswordLastSet
```

## PuTTY
Confirming a server fingerprint from the console, prior to connecting with PuTTY - for example:
```bash
cd /etc/ssh
sudo ssh-keygen -E -lf ssh_host_rsa_key.pub
```
^- *Note the "-lf" switch above is Lima-Foxtrot, lowercase.*

## SMTP / Telnet / OpenSSL
View SMTP header using Telnet:
```bash
telnet [ip/host] 25
...
quit
```

Query TLS using OpenSSL:
```bash
openssl s_client -connect mail99.example.com:25 -starttls smtp
```

## Windows 10
Diagnosing WiFi issues for a user, if you are able to remotely access a command line on their laptop:
```dos
netsh wlan show interfaces
```
^- *Quick means of confirming user's SSID, Signal and Receive/Transmit rates.*

Check WiFi password:
```dos
netsh wlan show profiles name="SSID" key=clear
```
