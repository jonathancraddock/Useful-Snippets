# Useful Snippets
Some personal notes, to cut down on time spent searching for this stuff! ;-)

## Bash Prompt
The default Bash user prompt on most of my VMs seems set to display the entire path to your working directory. A few layers deep and you're at the end of the line before you start typing. I prefer colours on, and display only the top-level of the folder.  
`sudo nano ~/.bashrc`

```bash
# uncomment for a colored prompt, if the terminal has the capability; turned
# off by default to not distract the user: the focus in a terminal window
# should be on the output of commands, not on the prompt
force_color_prompt=yes


if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\W\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\W\$ '
fi
```
^- un-comment the `force_color_prompt` setting, and towards the end of each `PS1` line there is a lower-case "w" (full path) which I've changed to an upper-case "W" (only the current directory).

## Browsers
Force "reader" mode in Firefox for site with malformed tags, and where the icon doesn't automatically appear. (Often sidesteps simple javascript blocking of adblock, etc.)
```
about:reader?url=
```
^- *paste full url at the end*

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

## Excel

Date to day of week:  
`=text(CELL,"dddd")`  
^- *where CELL is a date value.*

## Linux Misc.

Check details/availability of an uninstalled package:
```bash
sudo apt-get update
apt-cache showpkg <NAME OF PACKAGE>
```

Check folder sizes from current location:
```bash
du -chd 1 | sort -h
```
^- *size of each folder (only 1 level deep) sorted by size, plus a final total.*

## NMAP

Check if a specified port is open:
```bash
sudo nmap -sS -Pn -p 25 x.x.x.x
```
^- *in this example, is port 25 listening?*

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
openssl s_client -connect mail99.example.com:25 -name yourfqdn.com -starttls smtp
```
^- note, the SMTP `EHLO` will default to "mail.example.com" unless you add a `-name` switch. I discovered this after spending some time attempting to diagnose blacklisting issues... and I don't think this is highlighted in many online examples. If you add a `-debug` switch you can observe this behaviour in the hex dump.  
https://www.openssl.org/docs/manmaster/man1/s_client.html

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

## Xen Server
SSH to Xen server and make an ISO available in a new storage repository:
```bash
xe sr-create name-label="MyISORepository" type=iso device-config:location=/var/opt/iso/ device-config:legacy_mode=true content-type=iso
```
^- *having ssh'd to the server, downloaded an ISO (wget, etc, to for example, /var/opt/iso/), the command above will make it available as a new storage repository.*
