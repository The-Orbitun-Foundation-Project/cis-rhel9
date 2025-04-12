# 1.1.1.1 Ensure cramfs kernel module is not available (Automated)

## Audit
```bash
# Check if 'cramfs' is loaded
lsmod | grep -q cramfs

# Check if 'cramfs' is blacklisted
grep -q "blacklist cramfs" /etc/modprobe.d/*.conf

# Check if 'cramfs' is forced to /bin/false
grep -q "install cramfs /bin/false" /etc/modprobe.d/*.conf

# Check if 'cramfs' module exists in kernel
find /lib/modules/$(uname -r)/kernel/fs -name cramfs -type d 2>/dev/null
```

## Remediation
```bash
# Check if cramfs module is loaded and unload it
lsmod | grep -q cramfs && sudo modprobe -r cramfs

# Blacklist cramfs to prevent loading
echo "blacklist cramfs" | sudo tee /etc/modprobe.d/cramfs.conf

# Set cramfs to install as /bin/false (prevent loading)
echo "install cramfs /bin/false" | sudo tee -a /etc/modprobe.d/cramfs.conf

# Verify if the module exists in kernel directories
find /lib/modules/$(uname -r)/kernel/fs -name "cramfs" 2>/dev/null | while read -r path; do
    echo " - module exists in: $path"
done

echo -e "\n - remediation of kernel module: cramfs complete"
```
