# Wazuh-manager-migration
---
date: 2021-10-25
title: "How to migrate wazuh-manager"
weight: 
type: docs
description: 
---
### **Introduction**
This SOP helps you in migrating the wazuh-manager server.

### **Prerequisite**
- VPN access
- Access to bastion server

### **Steps**

**Backup your files**<br>
To avoid losing any configuration data or agent keys, we will stop the wazuh-manager service and make a copy of the directory where it lives. But first, lets check if we have enough space to create a copy of /var/ossec:

```
sudo du -h /var/ossec | tail -n1
sudo df -h /var
```
Now we copy all files to a separated backup directory:
```
sudo service wazuh-manager stop
sudo cp -rp /var/ossec /var/ossec_backup
```
Move ossec_backup directory to the new server. We can use sftp or rsync service for this operation.


**Install wazuh-manager server**

```
apt-get update
apt-get install curl
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | gpg --no-default-keyring --keyring gnupg-ring:/usr/
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | tee -a /etc/apt/sources.list.d/wazuh.list
apt-get update
apt-get install wazuh-manager
systemctl daemon-reload 
systemctl enable wazuh-manager.service 
systemctl start wazuh-manager.service
systemctl status wazuh-manager
```
**Restore configuration**
