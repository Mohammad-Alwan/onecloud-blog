---
date: 2026-01-19
status: new
---

# Corrupted inodes
### Issue
![fsck-root](assets/fsck-root.png)

### Troubleshoot Steps
1. Check mount status root filesystem (/)
```
findmnt /
```

2. If the root filesystem is mounted as read-write (rw), remount it as read-only (ro)
```
mount -o remount,ro /
```

3. Filesystem check and repair on root logical volume 
```
fsck -y /dev/mapper/system-rootlv
```

4.  Reboot server 
```
reboot
```

### Root Cause
!!! info
    The cause of this problem is still under discussion

