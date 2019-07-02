# Useful Snippets
Some personal notes on a few all too easily forgotten commands.

## DNS
Query an MX record:
```bash
dig google.com MX
dig +short google.com MX
```

## OpenSSL
Check certificate dates from a command line:
```bash
echo | openssl s_client -connect jonathancraddock.com:443 -servername jonathancraddock.com 2>/dev/null | openssl x509 -noout -dates
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

## SMTP
View SMTP header:
```bash
telnet [ip/host] 25
...
quit
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
