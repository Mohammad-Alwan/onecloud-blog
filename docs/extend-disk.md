---
date: 2025-12-17
---
# RHEL OS cannot boot normally (/sysroot: can't read superblock)
### **Issue**
The RHEL operating system cannot boot normally, experiencing a stuck state due to a mount error `/sysroot: can't read superblock on /dev/mapper/os-lv_root.`
=== "Error (1/3)"
    ![issue-2](assets/extend-disk2.png)

=== "Error (2/3)"
    ![extend-disk1](assets/extend-disk1.jpeg)

=== "Error (3/3)"
    ![extend-disk2](assets/issue-2.png)

### **Troubleshooting Steps**
1. Boot into rescue mode [How to boot RHEL to Rescue Mode](https://access.redhat.com/articles/3405661)
   
2. Extend LV /dev/mapper/os-lv_root menjadi 125G
   ```bash
   vgchange -ay
   lvextend -L 125G /dev/mapper/os-lv_root
   ```

3. Resize the filesystem by using the xfs_growfs command
   ```bash 
   xfs_growfs /dev/mapper/os-lv_root
   ```

### Root Cause
The system requires 80G on the root LV (`dev/mapper/os-lv_root`), but the root LV is only 71G in size.