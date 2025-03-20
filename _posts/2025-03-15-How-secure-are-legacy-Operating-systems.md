---
title: Server 2016 vs Newer OS Versions – Security and Modernization
description: >-
  A deep dive into the security risks of using Windows Server 2016 and how modern server versions like Server 2022 and 2025 offer better protection against advanced threats.
author: Tristan
date: 2025-03-15 08:00:00 +0100
categories: [Cybersecurity, Server Security]
tags: [Windows Server, Security, Modernization, Upgrade]
pin: false
---

# Server 2016 vs Newer OS Versions

## What is the Difference?

What is the difference between a legacy server operating system like Server 2016, which is still in LTCB (extended support), and modern variants like Server 2022 or 2025? This is a technical question that is often difficult to explain to C-Level management. If an application only works on Server 2016, what are the essential security risks that are no longer (sufficiently) mitigated against modern threats?

## Secured-Core Server

Today, sensitive information (such as credentials, tokens, etc.) can be stored in hardware, whereas on Server 2016, this was often done in software. This means that if a machine is hacked, it is almost impossible to retrieve credentials using hacking tools to move laterally within the network, thereby limiting the impact. A TPM 2.0 chip is required for this, which can be provided both in an IaaS platform like Azure and on-premises in Hyper-V to the guest OS.

Modern cybersecurity architectures utilize **Virtualized-Based Security (VBS)** and **Hypervisor-Protected Code Integrity (HVCI)**, which are active during the boot process. The system kernel is thus better protected against attacks. In combination with **Secure Boot**, this helps prevent untrusted code or drivers from starting up during boot.

## Example: Kernel Attack by the Lazarus Group

The infamous **Lazarus Group** has developed a malware variant called **CookiePlus**. This collects data about the server, including the computer name, process ID, and file paths, which can be used in the reconnaissance phase of an attack. Notably, there are no known CVEs (vulnerabilities) that can directly stop this attack.

Organizations that believe **Threat and Vulnerability Management (TVM)** is sufficient by simply patching known CVEs are mistaken. Legacy operating systems remain vulnerable even without specific CVEs. Upgrading to a modern server version offers better protection against kernel attacks and zero-days, for which patches may not yet be available.

**Windows Defender Exploit Guard** provides protection by allowing only trusted software to run, preventing malware like CookiePlus from executing and reducing the risk of a successful attack.

## Security Features Within the OS

Modern server systems include various additional security features that help continuously protect servers.

### Credential Guard

**Credential Guard** ensures that login credentials (such as passwords and SHA hashes) can no longer be extracted from a system’s memory via a memory dump. This prevents the use of hacking tools like **Mimikatz**.

### Attack Surface Reduction (ASR)

The following **ASR rules** are not supported on Server 2016, posing risks unless additional mitigations are implemented. A crucial mitigation is restricting internet access to only trusted URLs, such as Microsoft Defender updates.

- **Block JavaScript or VBScript from launching downloaded executable content**
- **Block Office communication applications from creating child processes**
- **Block Win32 API calls from Office macros**

> **Note:** On Server 2012 R2 and Server 2016, the modern unified agent must be installed before ASR works. Additionally, the Defender Cloud block level must be set to **High** or higher.

### Network Protection

**Network Protection** prevents servers from connecting to malicious domains, even if a user or process attempts to access a harmful URL. This feature is not available by default on Server 2016.

### Controlled Folder Access

Protects critical system files and directories from ransomware attacks by allowing only trusted applications to modify them.

### Exploit Protection

**Exploit Protection** replaces **EMET (Enhanced Mitigation Experience Toolkit)**, which was available in Server 2016. This helps protect against vulnerabilities that are not yet publicly disclosed.

### Windows Defender Application Control (WDAC)

**Code Integrity (CI)** was available in Server 2016, but in newer versions, management has been simplified thanks to a **default policy**. This allows all Microsoft-built-in applications (such as SQL Server) to be included by default without manual configuration.

## Servers in Intune (Endpoint Manager)

Server 2016 can only be managed via **Microsoft Defender for Endpoint (MDE)** for security settings. However, Server 2022 and 2025 offer full **Intune support**, making security management and compliance significantly easier.

## Version Limitations

Many security features list **Server 1803** as a supported version, but this refers to a **bare-metal version** of Server 2016. There is no standard Azure image available for this version, meaning most implementations use **Server 2016 build 1607**.

## Conclusion

Continuing to use **Server 2016** in a production environment poses **significant security risks**, especially given the rise of **zero-day exploits** and **advanced threats** such as those from the Lazarus Group. Newer versions like **Server 2022** and **2025** offer:

- Improved **kernel protection**
- Advanced **ASR rules**
- Better **exploit mitigation**
- Deeper integrations with **Defender** and **Intune**

Upgrading is not just a matter of **feature enhancements** but a **fundamental step** in implementing a modern security model.

## References

- [ASR Configuration Management: Microsoft Docs](https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/attack-surface-reduction-rules?view=o365-worldwide)

