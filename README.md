# **Penetration Testing Report (Metasploitable 3)** 

## 🎓 **Digital Egypt Pioneers Initiative (DEPI)**  
- **👨‍🏫 Instructor:** Ahmed Attia‏
- **🏢 Company:** Global Knowledge
- **🆔 Group Code:** `ONL4_ISS3_S1`  

---

## 👥 **Prepared By:**  
- 💼 Islam El-Sayed Ahmed  
- 💼 Abdelrahman Mohammed Ahmed
- 💼 Abdelrahman Tarek Sayed 
- 💼 Omar Salah Kamel Elmasry  
- 💼 Ahmed Elsayed Rozan  
- 💼 Mohammed Alsaeid Mohammed  

---

## 📅 **Project Details:**  
- **📋 Title:** Penetration Testing Report - Metasploitable 3  
- **🔑 Target IP:** `192.168.172.131`  
- **🖥️ OS:** Ubuntu 14.04 LTS (Linux kernel 3.x - 4.x)  
- **📍 System Type:** Host  

---

## 📜 **Table of Contents:**
1. [✨ Executive Summary](#-executive-summary)  
2. [🛡️ scope](#-scope)
3. [📚 Methodology](#-methodology)  
4. [⚠️ Summary of Findings](#-summary-of-findings)  
5. [🐞 Detailed Exploitation](#-detailed-exploitation)  
6. [🔧 Tools Used](#-tools-used)  
7. [📌 Conclusion & Recommendations](#-conclusion--recommendations)

---

## ✨ **Executive Summary:**  
A comprehensive vulnerability assessment and penetration test were conducted on **Metasploitable 3** (`192.168.172.131`) to simulate a real-world targeted cyber-attack. The assessment successfully identified and exploited multiple critical and high-risk vulnerabilities, demonstrating that a remote attacker can fully compromise the system.

**Key Achievements:**
- Full system compromise via multiple vectors (root access achieved)
- Exploitation of outdated services with known vulnerabilities
- Successful privilege escalation and remote code execution (RCE)
- Data leakage and unauthorized access to sensitive services

The target was found to be **highly exposed** with weak configurations, default credentials, and unpatched services.

---

## 🛡️ **Scope**  
- **🎯 Target:** Metasploitable 3.0  
- **🔗 IP Address:** `192.168.172.131`  
- **📍 Services Tested:** All exposed ports and services (FTP, SSH, HTTP, Samba, MySQL, etc.)  

---

## 📚 **Methodology**  
1. **🔍 Footprinting & Scanning** — Nmap, service enumeration  
2. **⚡ Vulnerability Assessment** — Nessus, Nmap NSE, Metasploit auxiliary modules  
3. **🚀 Exploitation** — Metasploit Framework, Hydra, manual techniques  
4. **📑 Post-Exploitation** — Privilege escalation, data exfiltration, LinPEAS  
5. **📋 Reporting** — Detailed findings with impact and mitigation  

**Tools & Frameworks:** Kali Linux, Metasploit Framework, Nmap, Hydra, ffuf, LinPEAS, etc.

---

## ⚠️ **Summary of Findings**  

| No. | Vulnerability                          | Risk     | Status      |
|-----|----------------------------------------|----------|-------------|
| 1   | FTP (ProFTPD 1.3.5) - mod_copy RCE    | Critical | ✅ Exploited |
| 2   | SSH (Weak/Default Credentials)         | High     | ✅ Exploited |
| 3   | HTTP (Drupal RCE + Web Vulns)          | High     | ✅ Exploited |
| 4   | Samba (Weak Creds + Writable Share)    | High     | ✅ Exploited |
| 5   | CUPS (Potential XSS)                   | Medium   | ⚠️ Attempted |
| 6   | MySQL (Weak Credentials)               | High     | ✅ Exploited |
| 7   | Ruby on Rails (ActionPack RCE)         | High     | ✅ Exploited |
| 8   | UnrealIRCd Backdoor                    | High     | ✅ Exploited |
| 9   | Apache Continuum (Cmd Exec)            | High     | ✅ Exploited |

**Total Critical/High Risk Issues:** 8+  
**Root Access Achieved:** Yes (Multiple Vectors)

---

## 🐞 **Detailed Exploitation**

### 1. **FTP (ProFTPD 1.3.5) - Critical**
- **Exploit:** `exploit/unix/ftp/proftpd_modcopy_exec`
- **Result:** Meterpreter session obtained → Full system users enumeration
- **Impact:** Remote Code Execution → Complete server compromise
- **Mitigation:** Update ProFTPD, disable mod_copy, use SFTP

### 2. **SSH (Weak Credentials) - High**
- **Credentials:** `vagrant:vagrant`
- **Technique:** Hydra brute-force + custom wordlist
- **Result:** Root access via `sudo su`
- **Mitigation:** Strong passwords, key-based auth, Fail2Ban

### 3. **HTTP / Drupal (RCE) - High**
- **Exploit:** `exploit/unix/webapp/drupal_coder_exec`
- **Additional:** phpMyAdmin access, Reflected XSS in chat app
- **Result:** Shell as `www-data`
- **Mitigation:** Update Drupal, disable directory listing, secure DB creds

### 4. **Samba (445/tcp) - High**
- **Credentials:** `chewbacca:rwaaaaawr5`
- **Technique:** Upload PHP reverse shell via writable share
- **Result:** Reverse shell as `www-data`
- **Mitigation:** Disable guest access, strong creds, restrict shares

### 5. **Ruby on Rails (Port 3500) - High**
- **Exploit:** `exploit/multi/http/rails_actionpack_inline_exec`
- **Result:** Shell as `chewbacca`
- **Mitigation:** Never run in development mode in production

### 6. **UnrealIRCd Backdoor (6697) - High**
- **Exploit:** Built-in backdoor in version 3.2.8.1
- **Result:** Shell as `boba_fett`
- **Mitigation:** Remove/replace the backdoored version immediately

### 7. **Apache Continuum (8080) - High**
- **Exploit:** `exploit/linux/http/apache_continuum_cmd_exec`
- **Result:** Meterpreter session as **root**
- **Mitigation:** Remove or update CI/CD tools

*(Additional services like MySQL and CUPS were also assessed)*

---

## 🔧 **Tools Used**  
- **Scanning:** Nmap, Nmap NSE  
- **Exploitation:** Metasploit Framework, Hydra, ffuf  
- **Post-Exploitation:** LinPEAS, Meterpreter, smbclient  
- **Others:** CeWL, msfvenom, Browser Dev Tools  

---

## 📌 **Conclusion & Recommendations**  
Metasploitable 3 is **extremely vulnerable** by design and should **never** be exposed to any untrusted network. The assessment proved that a motivated attacker can achieve full system compromise within minutes using publicly available tools and known exploits.

### Immediate Recommendations:
- **✅** Update or remove all outdated/vulnerable services
- **✅** Replace all default/weak credentials
- **✅** Implement strict firewall rules (allow only necessary ports from trusted IPs)
- **✅** Disable unnecessary services (e.g., development tools, backdoored packages)
- **✅** Follow the least privilege principle
- **✅** Regular patching and security audits
- **✅** Enable proper logging and monitoring
