# Useful Snippets
Some personal notes on a few all too easily forgotten commands.

## PuTTY
Confirming server fingerprint from console prior to connecting with PuTTY:
```bash
cd /etc/ssh
sudo ssh-keygen -E -lf ssh_host_rsa_key.pub
```
^- *Note the "-lf" switch above is Lima-Foxtrot, lowercase.*
