---
date: '2026-05-04T22:35:01+07:00'
---
!!! note
    In this case, the home directory of the user to be created and used for SFTP will have `root` as its owner and group.

### Create home directory
```bash
mkdir /home/directory
```

### Create User
```bash
sudo useradd -m -d /home/directory -s /bin/bash -c "COMMENT" <user>
```

### Create Key (in this case with PEM format, RSA type and specific number bits)
```bash
sudo ssh-keygen -m PEM -t rsa -b 4096 -C "COMMENT"
```

### Create ssh directory
```bash
sudo mkdir /home/directory/.ssh/ 
sudo chown <user>:<user> /home/directory/.ssh/ 
sudo chmod 700 /home/directory/.ssh/  
```

### Put the public key into the ~/.ssh/authorized_keys file.
```bash
sudo echo "<public key>" > /home/directory/.ssh/authorized_keys
sudo chmod 600 /home/directory/.ssh/authorized_keys
sudo chown <user>:<suser> /home/directory/.ssh/authorized_keys
```

### Add config fot SFTP in sshd config
```vim
## Example
        Match User <user>
        ForceCommand internal-sftp
        PasswordAuthentication yes
        ChrootDirectory /home/directory
        PermitTunnel no
        AllowAgentForwarding no
        AllowTcpForwarding no
        X11Forwarding no
        PubkeyAuthentication yes
```

### Restart sshd service
```
systemctl restart sshd.service
```

### Test sftp 
```
sftp -P <port> -i ./<private.key> <user>@<IP>
```