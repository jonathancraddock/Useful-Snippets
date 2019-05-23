# Useful Snippets
Some personal notes on a few all too easily forgotten commands.

## OpenSSL
Check certificate dates from a command line:
```bash
echo | openssl s_client -connect jonathancraddock.com:443 -servername jonathancraddock.com 2>/dev/null | openssl x509 -noout -dates
```

## PuTTY
Confirming a server fingerprint from the console, prior to connecting with PuTTY for example:
```bash
cd /etc/ssh
sudo ssh-keygen -E -lf ssh_host_rsa_key.pub
```
^- *Note the "-lf" switch above is Lima-Foxtrot, lowercase.*
