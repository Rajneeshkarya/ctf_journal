# BillingV2 CTF

## Category: Easy

## Enumeration
- **Nmap Scan:** Found open ports **22 (SSH)**, **80 (HTTP)**, **3306 (MySQL)**
- **Web Recon** Discovered **Mbilling Login page**
- **SQL Injection Attempts:**
    - Basic & advanced SQLi **failed**
    - Used `--script=vulners` in Nmap but CVEs were **not exploitable**
- **Directory Enumeration**
    - `dirsearch` on `/mbilling` revealed multiple directories but **no useful files**
- **Burpsuite Analysis**
    - Intercepted login & forgot password requests but found **no vulnerabilities**

## Initial Foothold

- Searched **MBilling vulnerabilities** and found a **Metasploit exploit**
- Successfully **gained a Meterpreter shell**
- Captured **user.txt**

## Privilege Escalation

- **SetUID check** Found **pppd** setuid but couldn't execute due to permission erros
- **Checked `sudo -l`:** Found **fail2ban-client** as `NO PASSWORD`
- **Exploiting Fail2Ban:**
    - Misused `actionban`, but realized correct syntax was `action`
    - Overwrote a rule to execute:
      ```bash
      cat /root/root.txt > /tmp/root.txt && chmod 777 /tmp/root.txt
      ````
    - Restarted Fail2Ban service - **Successfully escalated to root**
- Captured **root.txt**

## Summary
**Enumerated** services & found Mbilling
**Exploited** MBilling via Metasploit -> **Got user shell**
**Escalated Privileges** using **Fail2Ban misconfiguration** -> **Got root**
**Captured both flags**

---
**Lesssons Learned**
1. Always enumerate **sudo permissions `(sudo -l)`**
2. Understand how **fail2Ban works** to craft correct payloads
3. **Restarting a service** can sometimes execute modified rules
