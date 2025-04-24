# Wazuh Syslog Configuration to Ingest Cisco ASA Gateway Logs  
This project documents the configuration process for ingesting Cisco ASA firewall logs into a Wazuh server using syslog and rsyslog on a RHEL-based system.

## 🎯 Objective  
To enable log collection from a Cisco ASA gateway by configuring Wazuh to ingest syslog data, setting up appropriate log files, and forwarding logs using `rsyslog`.

---

## 🔍 What is Wazuh?  
Wazuh is a free, open-source security monitoring platform used for threat detection, integrity monitoring, incident response, and compliance. It collects and analyzes security data from endpoints, networks, and applications.

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
  <img src="https://img.shields.io/badge/-Wazuh_Agent-000000?&style=for-the-badge&logo=Wazuh&logoColor=white" />
  <img src="https://img.shields.io/badge/-RHEL-EE0000?&style=for-the-badge&logo=Red-Hat&logoColor=white" />
  <img src="https://img.shields.io/badge/-rsyslog-333333?&style=for-the-badge&logo=Linux&logoColor=white" />
  <img src="https://img.shields.io/badge/-Cisco_ASA-1BA0D7?&style=for-the-badge&logo=Cisco&logoColor=white" />
</div>

---

## 📝 Deployment Steps

### 1. Modify the `ossec.conf` file in Wazuh Manager  
Navigate to the config:
```bash
☰ → Server Management → Settings → Edit Configuration
Add the following block above the <policy_monitoring> section:
