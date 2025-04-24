# Wazuh Syslog Configuration to Ingest Cisco ASA Gateway Logs  
This project documents the configuration process for ingesting Cisco ASA firewall logs into a Wazuh server using syslog and rsyslog on a RHEL-based system.

## 🎯 Objective  
To enable log collection from a Cisco ASA gateway by configuring Wazuh to ingest syslog data, setting up appropriate log files, and forwarding logs using `rsyslog`.

---

## 🔍 What is Syslog and Why Are We Doing This?
**Syslog** is a standard protocol used by systems and network devices to send log or event messages to a centralized log server.

In this project, we configure **syslog ingestion** on a Wazuh server to collect logs from a **Cisco ASA firewall**. This is essential for:

- **Centralizing logs** from your network gateway  
- **Monitoring security events** such as denies, blocks, and threats  
- **Supporting incident response** with detailed traffic records  
- **Visualizing alerts and trends** in the Wazuh dashboard  

By forwarding Cisco ASA logs into Wazuh, we’re enhancing perimeter visibility, supporting real-time threat detection, and contributing to a stronger Security Operations Center (SOC) workflow.

---

## 📚 Skills Learned  
- Syslog ingestion for firewall devices  
- Configuration of `rsyslog` on RHEL-based systems  
- Wazuh manager and agent log collection setup  
- Permission and ownership handling for log files  
- Troubleshooting log flow from Cisco ASA to SIEM

---

## 🛠️ Tools Used  
<div>
  <a href="https://documentation.wazuh.com/current/quickstart.html" target="_blank"><img src="https://img.shields.io/badge/-Wazuh-0078D4?&style=for-the-badge&logo=Wazuh&logoColor=white" />
  <a href="https://developers.redhat.com/products/rhel/download" target="_blank"><img src="https://img.shields.io/badge/-RHEL-EE0000?&style=for-the-badge&logo=Red-Hat&logoColor=white" />
  <a href="" target="_blank"><img src="https://img.shields.io/badge/-rsyslog-333333?&style=for-the-badge&logo=Linux&logoColor=white" />
  <a href="" target="_blank"><img src="https://img.shields.io/badge/-Cisco_ASA-1BA0D7?&style=for-the-badge&logo=Cisco&logoColor=white" />
</div>

---

## 📝 Deployment Steps

### 1. Modify the `ossec.conf` file in Wazuh Manager  
Navigate to the config:
☰ → Server Management → Settings → Edit Configuration
Or run this on the Wazuh Box:
```bash
sudo nano /var/ossec/etc/ossec.conf
```
Add the following block above the <policy_monitoring> section:
```bash
<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/siteasa.log</location>
</localfile>
```

### 2. Create the ASA log file
In the Wazuh Box CLI run these commands:
```bash
sudo touch /var/log/siteasa.log
sudo chown root:root /var/log/siteasa.log
sudo chmod 0600 /var/log/siteasa.log
```

### 3. Configure rsyslog to accept ASA syslogs
Edit the rsyslog configuration:
```bash
sudo nano /etc/rsyslog.conf
```
Uncomment the following two lines:
```bash
module(load="imudp")  # needs to be done just once
input(type="imudp" port="514")
```
Then add the following under Global Directives:
```bash
# Storing Messages from a Remote System into a specific File
if $fromhost-ip startswith '<SITE-ASA-IP>' then /var/log/siteasa.log
& ~
```
Save and close the file.

### 4. Restart Services
```bash
sudo systemctl restart rsyslog
sudo systemctl restart wazuh-manager
```

### 5. Verify Log Ingestion
Check if logs are populating:
```bash
cd /var/log
ll
```
You should see the siteasa.log file increasing in size as ASA logs are received.

---

### Notes
Make sure the ASA firewall is configured to send syslogs to the Wazuh server's IP on UDP port 514.
Ensure the correct IP (<SITE-ASA-IP>) is used in the rsyslog configuration.

---

### 👨‍💻 Author
Mario Tagaras | Florida State University Alum













