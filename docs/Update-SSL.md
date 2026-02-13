---
date: 2026-02-20
---
# Update SSL 
!!! info
    This is an example for server that uses the httpd service listens on port 443.

### Action to check expired certificates
```bash
site="<ip-server>" ; port="443" ;echo | openssl s_client -servername $site -connect $site:$port | openssl x509 -noout -dates
```

### SCP Cert from Local to Target
```bash
scp -r /path/to/source/ user@<server>:/path/to/target
```

### Create Backup Cert
!!! note
    In this example, the SSL httpd service configuration uses the certificate in /etc/ssl/.
```bash
cd /etc/ssl
mkdir ssl_backup
cp -pfv <current-cert> ssl_backup/
```

### Change the Old Certificate to the New One
!!! note
    The source path is taken from the previous SCP result.
```bash
sudo cp -v path/to/source /etc/ssl/
```

### Check SSL expiration in the target server 
=== "PEM"
    ```bash
    openssl x509 -noout -in cert.pem -dates
    openssl x509 -noout -in cert.pem -text
    ```
=== "JKS"
    ```bash
    (for cert java)
    keytool -list -v -keystore cert.jks | less
    ```

### Restart or Reload Service
```bash
systemctl restart httpd.service

-or-

systemctl reload httpd.service
```

