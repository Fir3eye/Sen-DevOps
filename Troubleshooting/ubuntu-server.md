# Ubuntu Server Troubleshooting

This guide provides step-by-step commands to diagnose and resolve common issues on an Ubuntu server.

## Notes
- Always ensure you have backups of important data before making major changes.
- If a specific service is causing issues, check its documentation or seek help from community forums.
---

## 1. Check Server Health
### a. System Resource Usage
```
# CPU and memory usage
top

# Disk usage
df -h

# Free memory
free -h
```
### b. Identify High Resource Consumers
```
# Top 10 memory-hogging processes
ps aux --sort=-%mem | head -n 10

# Top 10 CPU-hogging processes
ps aux --sort=-%cpu | head -n 10
```
## 2. Check Services
```
a. List All Running and Failed Services
sudo systemctl list-units --type=service --state=failed

b. Restart Any Failing Service
sudo systemctl restart <service-name>

c. Check Service Logs
sudo journalctl -u <service-name> --since "1 hour ago"
```
## 3. Check Logs for Errors
```
a. System Logs
sudo journalctl -xe

b. Kernel Logs
dmesg | tail -n 20

c. Application-Specific Logs
# Example for Apache
sudo tail -n 50 /var/log/apache2/error.log

# Example for Nginx
sudo tail -n 50 /var/log/nginx/error.log
```
## 4. Check Networking
```
a. Verify Network Configuration
ip a

b. Test Internet Connectivity
ping -c 4 8.8.8.8  # Test basic connectivity
ping -c 4 google.com  # Test DNS resolution

c. Restart Network Services
sudo systemctl restart networking
sudo systemctl restart NetworkManager
```
## 5. Check Disk Health
```
a. List Mounted Filesystems
lsblk

b. Check Disk Health and SMART Status
sudo smartctl -a /dev/sda  # Replace /dev/sda with your disk name

c. Identify Disk Errors
dmesg | grep -i error
```
## 6. Check Memory Issues
```
a. Look for Out-of-Memory (OOM) Errors
dmesg | grep -i oom

b. Clear Buffers/Cache (if necessary)
sudo sync; echo 3 > /proc/sys/vm/drop_caches
```
## 7. Check Boot Issues (if system is unresponsive)
```
a. Check GRUB Configuration
cat /boot/grub/grub.cfg

b. View Boot Logs
sudo journalctl -b
```
## 8. Monitor Processes
```
a. Monitor Real-Time System Behavior
htop  # Install with 'sudo apt install htop' if not available
```
## 9. Update and Patch System
```
a. Update Package Lists
sudo apt update

b. Upgrade All Packages
sudo apt upgrade -y

c. Remove Unused Packages
sudo apt autoremove -y
```
## 10. Reboot as a Last Resort
- If you've applied fixes and the system still doesn’t respond correctly:
```
sudo reboot
```
