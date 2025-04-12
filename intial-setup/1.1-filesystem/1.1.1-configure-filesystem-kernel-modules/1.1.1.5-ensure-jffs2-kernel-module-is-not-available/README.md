# 1.1.1.5 Ensure jffs2 kernel module is not available (Automated)

## Audit
```bash
mod_name="jffs2"

# Check if module is loaded
lsmod | grep "$mod_name"

# Check if module is blacklisted
grep -q "blacklist $mod_name" /etc/modprobe.d/*.conf

# Check if module is set to /bin/false
grep -q "install $mod_name /bin/false" /etc/modprobe.d/*.conf

# Check if module exists in kernel
find /lib/modules/$(uname -r)/kernel/fs -name "$mod_name" -type d 2>/dev/null
```

## Remediation
```bash
mod_name="jffs2"

# Check if module is loaded and unload it
lsmod | grep -q "$mod_name" && sudo modprobe -r $mod_name

# Blacklist module to prevent loading
echo "blacklist $mod_name" | sudo tee /etc/modprobe.d/$mod_name.conf

# Set module to install as /bin/false (prevent loading)
echo "install $mod_name /bin/false" | sudo tee -a /etc/modprobe.d/$mod_name.conf

# Verify if the module exists in kernel directories
find /lib/modules/$(uname -r)/kernel/fs -name "$mod_name" 2>/dev/null 
```
