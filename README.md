# 🛡️ USB Pendrive Virus Removal Guide (Windows)

> VBS Trojan ভাইরাস থেকে পেনড্রাইভ এবং PC সুরক্ষিত করার সম্পূর্ণ গাইড।

---

## ⚠️ লক্ষণ

- পেনড্রাইভ লাগালে এই error দেখায়:
  ```
  Loading script "D:\sysvolume\u805003.vbs" failed
  (Operation did not complete successfully because the file
  contains a virus or potentially unwanted software.)
  ```
- Format দেওয়ার পরেও সমস্যা যায় না
- Windows Defender-এ **Trojan:PowerShell/Runner.PGRA!MTB** দেখায়

---

## 🔴 Step 1: Windows Defender দিয়ে ভাইরাস Remove করুন

1. **Start → Windows Security → Virus & threat protection** খুলুন
2. **"Start actions"** বাটনে ক্লিক করুন
3. **Remove / Quarantine** করুন
4. PC **Restart** করুন
5. Restart-এর পর আবার **Full Scan** দিন:
   - Scan options → **Full Scan** → Scan now

---

## 🔧 Step 2: CMD দিয়ে পেনড্রাইভ Force Format করুন

### CMD খুলুন (Administrator হিসেবে)

```
Win + S → "cmd" লিখুন → Right Click → Run as Administrator
```

### DiskPart চালু করুন

```bash
diskpart
```

### সব Disk দেখুন

```bash
list disk
```

> আউটপুট দেখে পেনড্রাইভের Disk Number বের করুন (সাধারণত Disk 1)

```
  Disk ###  Status         Size     Free
  --------  -------------  -------  ----
  Disk 0    Online          238 GB        ← এটা PC-র Hard Drive
  Disk 1    Online         3894 MB        ← এটা পেনড্রাইভ
```

### ⚠️ সঠিক Disk Select করুন

```bash
select disk 1
```

> **সতর্কতা:** ভুল disk number দিলে PC-র সব data মুছে যাবে!

### পেনড্রাইভ Clean করুন

```bash
clean
```

### নতুন Partition তৈরি করুন

```bash
create partition primary
```

### Format করুন

```bash
format fs=fat32 label="USB" override quick
```

### Drive Letter Assign করুন

```bash
assign
```

### DiskPart থেকে বের হন

```bash
exit
```

---

## 🚫 "Access is denied" Error হলে

ভাইরাস Active থাকলে এই error আসতে পারে।

**সমাধান — Safe Mode-এ Format করুন:**

```
Win + R → msconfig → Enter
```

1. **Boot** tab-এ যান
2. **Safe boot** চেক করুন → **OK**
3. PC Restart হবে → Safe Mode-এ উঠবে
4. Safe Mode-এ CMD (Admin) খুলুন
5. উপরের সব diskpart commands আবার দিন

Safe Mode শেষে:
```
Win + R → msconfig → Boot → Safe boot uncheck → OK → Restart
```

---

## ✅ Step 3: Malwarebytes দিয়ে Extra Scan করুন

Windows Defender-এর পাশাপাশি Malwarebytes দিয়ে scan করুন।

1. Download: https://www.malwarebytes.com (Free version)
2. Install করুন
3. **Scan** চালান
4. পাওয়া সব threat **Remove** করুন

---

## 🔐 Step 4: পাসওয়ার্ড পরিবর্তন করুন

ভাইরাস Active থাকাকালীন পাসওয়ার্ড চুরি হতে পারে। নিচেরগুলো অবশ্যই পরিবর্তন করুন:

- [ ] Email (Gmail, Yahoo, etc.)
- [ ] Facebook / Social Media
- [ ] Online Banking
- [ ] যেকোনো গুরুত্বপূর্ণ অ্যাকাউন্ট

---

## 🛡️ ভবিষ্যতে সুরক্ষিত থাকতে

| করণীয় | বর্জনীয় |
|--------|---------|
| পেনড্রাইভ লাগানোর পর Scan করুন | অপরিচিত পেনড্রাইভ সরাসরি Open করবেন না |
| Windows Defender চালু রাখুন | ডাবল ক্লিক করে পেনড্রাইভ খুলবেন না |
| Windows নিয়মিত Update করুন | Unknown `.vbs` / `.bat` ফাইল Run করবেন না |
| Full Format ব্যবহার করুন | Quick Format-এ ভাইরাস থেকে যেতে পারে |

---

## 💡 পেনড্রাইভ যদি তারপরেও ঠিক না হয়

পেনড্রাইভটি **permanently infected বা নষ্ট** হয়ে গেছে।
**নতুন পেনড্রাইভ কেনাই সবচেয়ে নিরাপদ সমাধান।**

---

## 📋 Quick Reference — সব Commands একসাথে

```bash
# CMD (Admin) তে চালান
diskpart
list disk
select disk 1        # আপনার পেনড্রাইভের নম্বর দিন
clean
create partition primary
format fs=fat32 label="USB" override quick
assign
exit
```

---

> **Made for:** Windows 10/11
> **Virus Type:** Trojan:PowerShell/Runner.PGRA!MTB | VBS Script Virus
> **Tested on:** DISKPART version 10.0.190
> 41.3636
>
```
tasklist
```
## Find Virus 
---
```
wmic process list brief
````
## Details Virus
---
## Network Connection Check
```
netstat -ano
tasklist | findstr PID_NUMBER
```
## Windows System file Intrigrity
```
sfc /scannow
```
## আমি খুঁজেছি এই ধরনের নাম:

- random.exe
- svhost.exe (fake spelling)
- winupdate.exe (fake)
- cryptominer / xmrig
- unknown high memory exe

## Pendrive letter diye scan
```
attrib -h -r -s /s /d E:\*.*
```

# 🛡️ Advanced Malware Check Script — Windows PowerShell

> **Professional Level Manual Inspection Guide**  
> Windows সিস্টেমে Hidden Infection চেক করার জন্য Advanced PowerShell Scripts

---

## ⚡ শুরু করার আগে

### Step 1 — PowerShell Administrator হিসেবে খুলুন

```
Start → Search → PowerShell → Right Click → Run as Administrator
```

> ⚠️ **সতর্কতা:** Administrator ছাড়া কিছু script কাজ নাও করতে পারে।

---

## 🧠 Script 1 — Suspicious Process Check

System32 ছাড়া অন্য কোথাও থেকে চলছে এমন process খুঁজে বের করে।

```powershell
Get-Process |
Where-Object {
    $_.Path -notmatch "Windows\\System32" -and
    $_.Path -ne $null
} |
Select Name, Path, Id |
Format-Table
```

**📌 কী দেখাবে:**
- System32 ছাড়া কোথায় `.exe` চলছে
- যেকোনো অপরিচিত path সন্দেহজনক হতে পারে

---

## 🧠 Script 2 — Startup Malware Check

System startup-এ কোন কোন program automatically চালু হচ্ছে তা দেখায়।

```powershell
Get-CimInstance Win32_StartupCommand |
Select Name, command, Location
```

**📌 কী দেখাবে:**
- Startup-এ থাকা সকল entry
- Unknown বা অপরিচিত startup entry থাকলে সন্দেহ করুন

---

## 🧠 Script 3 — Network Connection Malware Check

Established network connection এবং তাদের remote IP দেখায়।

```powershell
Get-NetTCPConnection |
Where-Object State -eq "Established" |
Select LocalAddress, RemoteAddress, OwningProcess
```

**📌 কী দেখাবে:**
- কোন process কোন IP-তে connected
- Unknown foreign IP detect করতে পারবেন
- Suspicious IP পেলে online-এ check করুন

---

## 🧠 Script 4 — Hidden Driver Malware Check 🔥

> **Very Important** — Rootkit এবং hidden driver malware ধরতে সবচেয়ে কার্যকর।

```powershell
Get-WmiObject Win32_SystemDriver |
Where-Object {$_.State -eq "Running"} |
Select DisplayName, PathName
```

**📌 কী দেখাবে:**
- সকল running system driver
- ⚠️ `System32` ছাড়া অন্য কোনো path থেকে driver চললে **বিপদ**

---

## 🧠 Script 5 — File System Root Check

System32 ফোল্ডারে নতুন বা অপরিচিত `.exe` ফাইল আছে কিনা দেখায়।

```powershell
Get-ChildItem C:\Windows\System32 |
Where-Object {$_.Extension -eq ".exe"} |
Select Name, Length, LastWriteTime
```

**📌 কী দেখাবে:**
- System32-এর সকল `.exe` ফাইল ও তাদের তারিখ
- নতুন বা অপরিচিত `.exe` থাকলে সন্দেহ করুন

---

## 🛑 সন্দেহজনক কিছু পেলে কী করবেন?

1. **Output copy করুন** — সম্পূর্ণ result কপি করুন
2. **Analyse করান** — কোনো expert বা AI-কে দেখান
3. **VirusTotal চেক করুন** — সন্দেহজনক file `virustotal.com`-এ আপলোড করুন
4. **Disconnect করুন** — মারাত্মক সন্দেহ হলে internet বন্ধ রাখুন

---

## ✅ সিস্টেম Clean হওয়ার লক্ষণ

| চেক | স্ট্যাটাস |
|-----|----------|
| সকল Process path স্বাভাবিক | ✅ |
| Startup-এ পরিচিত program | ✅ |
| Network connection পরিচিত | ✅ |
| Driver সব System32-এ | ✅ |
| System32-এ নতুন `.exe` নেই | ✅ |

---

## 💡 পরবর্তী পদক্ষেপ (Advanced)

> **Super Deep Hidden Rootkit Detector Method (Expert Level)**  
> ৯৯.৯% advanced malware ধরতে সক্ষম।

এই method-এ অন্তর্ভুক্ত:
- Memory forensics analysis
- Registry deep scan
- Boot sector inspection
- Kernel-level hook detection

---

## 📝 Notes

- এই scripts শুধুমাত্র **নিজের সিস্টেম** চেক করার জন্য
- অন্যের সিস্টেমে অনুমতি ছাড়া ব্যবহার করবেন না
- Windows Defender এবং এই scripts একসাথে ব্যবহার করা ভালো

---

*তৈরি করা হয়েছে: Zahid ভাইয়ের জন্য 🛡️*


# 🔐 Windows Rootkit & Malware Detection - Security Professional Manual Inspection Guide

## 📋 Overview
এই গাইডলাইনটি Windows সিস্টেমের জন্য **Professional Level Manual Inspection** এর একটি সম্পূর্ণ ডকুমেন্টেশন। নিচের ধাপগুলো অনুসরণ করে আপনি আপনার সিস্টেমে কোনো রুটকিট বা ম্যালওয়্যার আছে কিনা তা ম্যানুয়ালি চেক করতে পারবেন।

---

## 📁 Table of Contents
- [Prerequisites](#prerequisites)
- [Step 1: Hidden AutoRun Scan](#step-1-hidden-autorun-scan)
- [Step 2: Driver Level Rootkit Check](#step-2-driver-level-rootkit-check-)
- [Step 3: Kernel Hook Detection](#step-3-kernel-hook-detection)
- [Step 4: Network Listener Analysis](#step-4-network-listener-analysis-)
- [Step 5: Process Anomaly Detection](#step-5-process-anomaly-detection-)
- [Step 6: Boot Level Investigation](#step-6-boot-level-investigation)
- [Quick Response Protocol](#-quick-response-protocol)
- [Glossary](#glossary)

---

## ⚡ Prerequisites
- **PowerShell** (Run as Administrator)
- **Command Prompt** (Run as Administrator)
- Basic understanding of Windows processes

---

## 🔍 Step 1: Hidden AutoRun Scan

### PowerShell (Admin)
```powershell
# Check All Users Startup
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Run

# Check Current User Startup  
Get-ItemProperty HKCU:\Software\Microsoft\Windows\CurrentVersion\Run
```

### 📊 Output Analysis

**✅ Safe Output Example:**
```
SecurityHealth     : C:\Program Files\Windows Defender\MSASCuiL.exe
RtHDVCpl           : C:\Program Files\Realtek\Audio\HDA\RAVCpl64.exe -s
```

**🚨 Suspicious Indicators:**
| Location | Risk Level |
|----------|------------|
| `C:\Users\[User]\AppData\Roaming\*.exe` | ⚠️ High |
| `C:\Temp\*.exe` | ⚠️ Critical |
| `C:\ProgramData\*.exe` (unknown vendor) | ⚠️ High |
| Random name folders (e.g., `C:\sdfg43\`) | ⚠️ Critical |

---

## 🔥 Step 2: Driver Level Rootkit Check ⭐

### PowerShell (Admin)
```powershell
Get-WmiObject Win32_SystemDriver |
Where-Object {$_.PathName -notmatch "Windows\\\\System32"}
```

### 📊 Output Analysis

**✅ Clean System:** No output or very few known drivers

**🚨 Suspicious Output Example:**
```
Name           : UnknownDriver
PathName       : C:\ProgramData\Microsoft\Driver\sysdrv.sys
State          : Running
```

### ⚠️ Red Flags:
- Drivers from `C:\ProgramData`
- Hidden/system file attributes
- Drivers from temp folders
- Digital signature missing

---

## 🌐 Step 3: Kernel Hook Detection

### Command Prompt (Admin)
```cmd
netsh winhttp show proxy
```

### 📊 Output Analysis

**✅ Normal:**
```
Current WinHTTP proxy settings:
    Direct access (no proxy server).
```

**🚨 Suspicious:**
```
Current WinHTTP proxy settings:
    Proxy Server(s) :  192.168.1.105:8080
    Bypass List     :  <local>
```

### 🔴 Critical Signs:
- Unknown proxy server IP
- Port 8080/3128/4444 configured
- Proxy set without user knowledge

---

## 📡 Step 4: Network Listener Analysis ⭐⭐

### Command Prompt (Admin)
```cmd
netstat -abon
```

### Alternative (Simpler):
```cmd
netstat -ano
```

### 📊 Port Analysis Table

| Port | Service | Risk if Unknown |
|------|---------|-----------------|
| 4444 | Metasploit | 🔴 CRITICAL |
| 5555 | Android ADB/Backdoor | 🔴 CRITICAL |
| 6666 | IRC/Backdoor | 🔴 CRITICAL |
| 1337 | Various Malware | 🔴 CRITICAL |
| 8080 | HTTP Proxy | ⚠️ HIGH |
| 3389 | RDP | ⚠️ MEDIUM |

### 🚨 Suspicious Output Example:
```
Proto  Local Address     Foreign Address      State       PID
TCP    0.0.0.0:4444      0.0.0.0:0           LISTENING   4200
TCP    192.168.1.100:54321 45.155.205.xxx:8080 ESTABLISHED 6942
```

---

## 🎯 Step 5: Process Anomaly Detection ⭐⭐⭐

### PowerShell (Admin)
```powershell
Get-CimInstance Win32_Process |
Select Name, ProcessId, ExecutablePath |
Where-Object {$_.ExecutablePath -notmatch "Windows"} |
Format-Table -AutoSize
```

### 📊 Critical Locations

| Path | Risk Level | Common Malware |
|------|------------|----------------|
| `C:\Users\[User]\AppData\Roaming\` | 🔴 CRITICAL | Trojan.Downloader |
| `C:\Users\[User]\AppData\Local\Temp\` | 🔴 CRITICAL | Ransomware |
| `C:\ProgramData\` | ⚠️ HIGH | Rootkit Components |
| `C:\PerfLogs\` | 🔴 CRITICAL | Hidden Malware |
| `C:\` (root directory) | ⚠️ HIGH | Legacy Malware |

### ✅ Safe Examples:
```
chrome.exe    1234  C:\Program Files\Google\Chrome\Application\chrome.exe
discord.exe   5678  C:\Users\User\AppData\Local\Discord\app-1.0\discord.exe
```

### 🚨 Suspicious Examples:
```
svchost.exe   4321  C:\Users\User\AppData\Roaming\svchost.exe
system.exe    8765  C:\Temp\winupdate.exe
```

---

## 🖥️ Step 6: Boot Level Investigation

### Command Prompt (Admin)
```cmd
bcdedit /enum
```

### 📊 Normal Boot Configuration:
```
Windows Boot Loader
-------------------
identifier              {current}
device                  partition=C:
path                    \Windows\system32\winload.efi
```

### 🚨 Bootkit Indicators:
- Multiple `{current}` entries
- Unknown identifiers
- Strange device paths
- Linux entries without dual-boot

---

## 🚀 Quick Response Protocol

### If You Find These → **IMMEDIATE ACTION REQUIRED**

### 🔴 CRITICAL ALERT - Run These 3 Commands:

#### `rk2` - Driver Check
```powershell
Get-WmiObject Win32_SystemDriver | Where-Object {$_.PathName -notmatch "Windows\\\\System32"}
```

#### `rk4` - Network Check
```cmd
netstat -abon | findstr /i "listening established"
```

#### `rk5` - Process Check
```powershell
Get-CimInstance Win32_Process | Where-Object {$_.ExecutablePath -notmatch "Windows" -and $_.ExecutablePath -match "Temp|AppData|ProgramData"} | Format-Table Name, ProcessId, ExecutablePath -AutoSize
```

### 📞 Immediate Actions:
1. **Disconnect network** if unknown outbound connections found
2. **Capture screenshots** of suspicious findings
3. **Save command outputs** to text file:
   ```powershell
   [commands] | Out-File C:\malware_scan_$(Get-Date -Format 'yyyyMMdd_HHmmss').txt
   ```
4. **Quarantine system** if bootkit suspected

---

## 📊 Threat Level Matrix

| Level | Color | Action Required |
|-------|-------|-----------------|
| **Critical** | 🔴 | Immediate isolation |
| **High** | 🟠 | Deep scan + Analysis |
| **Medium** | 🟡 | Monitor + Update |
| **Low** | 🟢 | Normal operation |

---

## 💡 Pro Tips

### 📝 Save Results for Analysis
```powershell
$date = Get-Date -Format 'yyyyMMdd_HHmmss'
$output = "C:\Rootkit_Scan_$date.txt"

# Save all results
"=== AUTO RUN ===" | Out-File $output
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Run >> $output
Get-ItemProperty HKCU:\Software\Microsoft\Windows\CurrentVersion\Run >> $output

"=== DRIVERS ===" >> $output
Get-WmiObject Win32_SystemDriver | Where-Object {$_.PathName -notmatch "Windows\\\\System32"} >> $output

"=== PROCESSES ===" >> $output
Get-CimInstance Win32_Process | Where-Object {$_.ExecutablePath -notmatch "Windows"} >> $output

Write-Host "Scan saved to: $output" -ForegroundColor Green
```

### 🔍 Additional Checks
- Check `C:\Windows\Temp` for suspicious files
- Review Event Viewer for error patterns
- Monitor CPU usage for mining malware
- Check browser extensions

---

## 📚 Glossary

| Term | Meaning |
|------|---------|
| **Rootkit** | Hidden malware that modifies OS core |
| **Bootkit** | Malware infecting boot process |
| **HKLM** | HKEY_LOCAL_MACHINE registry |
| **HKCU** | HKEY_CURRENT_USER registry |
| **PID** | Process Identifier |
| **UEFI** | Modern BIOS replacement |

---

## 🏁 Conclusion

এই গাইডলাইনটি অনুসরণ করলে আপনি প্রফেশনাল লেভেলে আপনার Windows সিস্টেম চেক করতে পারবেন। **মনে রাখবেন:**

✅ নিয়মিত স্ক্যান করুন  
✅ আপডেট রাখুন  
✅ সন্দেহজনক কিছু পেলেই রিপোর্ট করুন  
✅ ব্যাকআপ রাখুন  

---

### 📝 Last Updated: March 2026
### 👤 Author: Security Professional
### ⚖️ License: Educational Purpose Only

---
*এই ডকুমেন্টেশন শুধুমাত্র নিরাপত্তা বিশ্লেষণের শিক্ষামূলক উদ্দেশ্যে তৈরি।*
## Rootkit Check 
```
Get-WmiObject Win32_SystemDriver | Where-Object {$_.PathName -notmatch "Windows\\System32"}

Get-CimInstance Win32_Process | Where-Object {$_.ExecutablePath -notmatch "Windows" -and $_.ExecutablePath -match "Temp|AppData|ProgramData"} | Format-Table Name, ProcessI, ExecutablePath -AutoSize

netstat -abon
```

# New
নিচে আমি **Windows-এ PowerShell ব্যবহার করে basic forensic / security check** করার কিছু ধাপ দেখাচ্ছি। এগুলো মূলত **detection / investigation** এর জন্য — ১০০% নিশ্চিতভাবে rootkit বা keylogger ধরা সবসময় সম্ভব না, কিন্তু এগুলো দিয়ে অনেক suspicious জিনিস ধরা যায়।

---

# 1️⃣ Rootkit detect (System integrity check)

Rootkit সাধারণত **system file modify** করে। তাই প্রথমে Windows system file verify করা হয়।

### Step 1: System File Checker

PowerShell (Admin) এ run করুন:

```
sfc /scannow
```

এটা Windows system file scan করবে এবং corrupted file repair করবে।

---

### Step 2: DISM health check

```
DISM /Online /Cleanup-Image /CheckHealth
```

আরো deep scan:

```
DISM /Online /Cleanup-Image /RestoreHealth
```

---

### Step 3: Hidden driver check

Rootkit অনেক সময় driver হিসেবে লুকিয়ে থাকে।

```
driverquery
```

আরো detail:

```
driverquery /v
```

Suspicious driver location খুঁজবেন:

```
C:\Temp
C:\Users
AppData
```

Normal driver সাধারণত থাকে:

```
C:\Windows\System32\drivers
```

---

# 2️⃣ Keylogger detect

Keylogger সাধারণত **startup program বা background process** হিসেবে থাকে।

### Step 1: Startup program check

```
wmic startup get caption,command
```

PowerShell version:

```
Get-CimInstance Win32_StartupCommand
```

এখানে দেখবেন unknown program আছে কিনা।

---

### Step 2: Keyboard hook program detect

Running process দেখুন:

```
Get-Process
```

Suspicious নাম খুঁজুন যেমন:

* keylogger
* monitor
* spy
* hook

---

### Step 3: Installed program check

```
Get-WmiObject -Class Win32_Product
```

---

# 3️⃣ Reverse shell detect

Reverse shell হলে attacker এর server এ connection থাকে।

### Step 1: Active network connection

```
netstat -ano
```

দেখবেন:

```
ESTABLISHED
LISTENING
```

---

### Step 2: Suspicious connection filter

```
netstat -ano | findstr ESTABLISHED
```

তারপর PID দিয়ে process বের করুন:

```
tasklist | findstr PID_NUMBER
```

---

### Step 3: PowerShell network process

```
Get-NetTCPConnection
```

---

# 4️⃣ Network sniffing detect

Packet sniffer থাকলে network adapter **promiscuous mode** এ যেতে পারে।

### Step 1: Network adapter check

```
Get-NetAdapter
```

---

### Step 2: Running capture tool check

```
tasklist
```

Sniffer software খুঁজবেন:

* Wireshark
* tcpdump
* npcap
* sniff

---

### Step 3: Listening port check

```
netstat -abno
```

যদি unknown program port open করে থাকে তাহলে suspicious।

---

# 🔎 Extra forensic command (very useful)

### Running scheduled tasks

```
schtasks /query /fo LIST /v
```

Malware অনেক সময় scheduled task ব্যবহার করে।

---

### Services check

```
Get-Service
```

Unknown service খুঁজবেন।

---

# ⚠️ Important

এই command গুলো **basic forensic analysis**।
কিন্তু advanced malware detect করতে tools লাগে যেমন:

* **Microsoft Sysinternals Autoruns**
* **Process Explorer**
* **TCPView**
* **GMER rootkit scanner**

---

✅ আপনার আগের process list দেখে **কোন suspicious sign পাইনি**।

---

👍 আপনি যেহেতু Kali Linux ও security নিয়ে কাজ করছেন, চাইলে আমি আপনাকে দেখাতে পারি:

**Windows-এ সবচেয়ে dangerous hidden malware detect করার ৩টা advanced technique**

যেগুলো অনেক ethical hackerও জানে না।

Windows-এ **advanced malware detection** করা সাধারণত forensic analysts ও incident responders করে। নিচে আমি **৩টি advanced technique** দেখাচ্ছি যেগুলো Windows security investigation-এ খুব ব্যবহার হয়। এগুলো mainly **analysis / detection** এর জন্য।

---

# 1️⃣ Autoruns দিয়ে hidden persistence detect

অনেক malware **startup, service, scheduled task, browser plugin** হিসেবে লুকিয়ে থাকে। এগুলো detect করার জন্য সবচেয়ে powerful tool হলো **Autoruns** (Microsoft Sysinternals suite)।

### Step-by-step

1️⃣ Autoruns download করুন

* [https://learn.microsoft.com/sysinternals/downloads/autoruns](https://learn.microsoft.com/sysinternals/downloads/autoruns)

2️⃣ Zip extract করে **Autoruns64.exe** run করুন (Admin)

3️⃣ Menu থেকে:

```
Options → Hide Microsoft Entries
```

এতে শুধু **third-party entries** দেখা যাবে।

4️⃣ এখন check করুন:

* Logon
* Services
* Drivers
* Scheduled Tasks
* AppInit
* Winsock Providers

⚠️ Suspicious হলে সাধারণত দেখা যায়:

```
C:\Users\...\AppData\Roaming\something.exe
C:\Temp\malware.exe
```

Normal system file থাকে:

```
C:\Windows\System32\
C:\Program Files\
```

---

# 2️⃣ Process Explorer + VirusTotal analysis

Running process analyse করার জন্য সবচেয়ে powerful tool হলো
**Process Explorer**।

### Step-by-step

1️⃣ Download:

[https://learn.microsoft.com/sysinternals/downloads/process-explorer](https://learn.microsoft.com/sysinternals/downloads/process-explorer)

2️⃣ Run **procexp64.exe**

3️⃣ Menu:

```
Options → VirusTotal → Check VirusTotal.com
```

এখন প্রতিটা process এর পাশে score দেখাবে:

```
0/75  → safe
10/75 → suspicious
40/75 → malware
```

4️⃣ Suspicious process হলে:

Right click → **Properties**

দেখবেন:

* Path
* Command line
* Network connection
* Loaded DLL

---

# 3️⃣ TCPView দিয়ে hidden network connection detect

অনেক malware **reverse shell / C2 server** এর সাথে connect করে।

এটা detect করতে use হয় **TCPView**।

### Step-by-step

1️⃣ Download:

[https://learn.microsoft.com/sysinternals/downloads/tcpview](https://learn.microsoft.com/sysinternals/downloads/tcpview)

2️⃣ Run **tcpview.exe**

এখানে real-time network connection দেখাবে:

```
Process        Local Address        Remote Address
chrome.exe     192.168.1.5          142.250.182.206
```

⚠️ Suspicious sign:

* Unknown process
* Strange foreign IP
* Random high port

Right click → **Whois**

এতে IP owner দেখা যায়।

---

# 🔎 Bonus forensic check

PowerShell দিয়ে **suspicious scheduled tasks**:

```
Get-ScheduledTask | Where-Object {$_.State -eq "Ready"}
```

Service check:

```
Get-Service | Where-Object {$_.Status -eq "Running"}
```

---

# ⚠️ Reality check

Modern malware অনেক সময় detect করা কঠিন কারণ:

* Rootkit
* Kernel driver
* Memory-only malware

এগুলো detect করতে professional tools লাগে:

* **GMER**
* **Volatility**
* **Kaspersky TDSSKiller**

---

✅ আপনার আগের process list দেখে এখন পর্যন্ত **malware sign পাইনি**।

---

যেহেতু আপনি Kali Linux ও cyber security নিয়ে কাজ করছেন, চাইলে আমি আপনাকে দেখাতে পারি:

* **Windows-এ hackerরা কীভাবে hidden backdoor install করে**
* **এবং defender কীভাবে সেটা detect করে**

এটা খুব interesting cyber security topic। 😈

ঠিক আছে! আমি আপনাকে **Autoruns দিয়ে Windows 10-এ Trojan/Crypto malware 100% clean করার সম্পূর্ণ workflow** step by step diagramসহ দেখাচ্ছি। এটা follow করলে malware detect ও remove করা অনেক সহজ হবে।

---

# **Autoruns Workflow to Clean Trojan/Crypto Malware (Windows 10)**

---

## **Step 1: System কে Safe Mode-এ চালু করা**

* Malware remove করার জন্য Safe Mode সবচেয়ে নিরাপদ।
* **Steps:**

  1. Start → Power → **Restart while holding Shift**
  2. Troubleshoot → Advanced Options → Startup Settings → Restart
  3. Safe Mode with Networking select করুন

---

## **Step 2: Autoruns খুলুন**

* Autoruns download & unzip করুন: [Autoruns Sysinternals](https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns)
* **Run as Administrator**
* সব startup entries load হতে কিছু সময় নেবে

---

## **Step 3: Suspicious entries চিহ্নিত করা**

**Autoruns Tabs focus করুন:**

| Tab             | Suspicious entry চিহ্নিত করার guideline               |
| --------------- | ----------------------------------------------------- |
| Logon           | Random names, unknown publisher, temp folder path     |
| Scheduled Tasks | Unknown tasks, recently created, no publisher         |
| Services        | Unknown services, random names                        |
| Drivers         | Unknown drivers, recently added, no digital signature |

💡 Extra: **Options → Verify Code Signatures** enable করুন।
Signed files normally safe, unsigned unknown files suspicious।

---

## **Step 4: Suspicious entry disable/remove**

* Entry select করুন
* **Right-click → Delete** (Permanent)
* **Right-click → Uncheck** (Temporary disable)

> Driver বা service remove করার আগে research করুন।

---

## **Step 5: Suspicious process kill করা (PowerShell / CMD)**

* Autoruns-এ entry identify করার পরে, যদি process running থাকে kill করতে হবে

**PowerShell:**

```powershell
# Replace evilprocess.exe with your suspicious process name
Get-Process evilprocess | Stop-Process -Force
```

**CMD:**

```cmd
tasklist | findstr evilprocess.exe
taskkill /F /IM evilprocess.exe
```

> Kill করার পরে, Autoruns থেকে entry delete না করলে reboot-এ আবার start হবে।

---

## **Step 6: Temporary files delete**

* Windows + R → `%temp%` → Delete all files
* C:\Windows\Temp\ সব temporary files clean করুন

---

## **Step 7: Post-cleanup Full Scan**

* Windows Defender / Malwarebytes দিয়ে full scan চালান
* নিশ্চিত হন সব malicious files quarantine/remove হয়েছে

---

## **Step 8: System Restore & Backup**

* Clean System Restore point তৈরি করুন
* Regular backup রাখুন

---

## **Step 9: Monitor CPU/GPU Usage**

* Task Manager → Processes → High CPU/GPU suspicious programs check করুন
* Crypto malware অনেক সময় mining করতে high GPU/CPU usage দেখায়

---

## **Flow Diagram**

```
[Boot Windows in Safe Mode]
              |
              v
        [Run Autoruns as Admin]
              |
              v
   [Identify Suspicious Entries]
              |
              v
[Disable/Delete Entries & Kill Processes]
              |
              v
   [Delete Temp Files & Browser Check]
              |
              v
   [Full Scan: Defender / Malwarebytes]
              |
              v
   [System Restore & Backup]
              |
              v
[Monitor CPU/GPU & Reboot for Check]
```

---

💡 **Tip:**
Autoruns use করার সময় **step by step disable → reboot → check** approach ভালো। একবারে সব delete করলে system crash risk থাকে।

---

এবারের রিপোর্টটা **আগেরটার মতো harmless না** ❗
👉 এখানে **real USB shortcut virus detect হয়েছে** — এটা clean করা দরকার।

---

## 🚨 Virus Details (তোমার report অনুযায়ী)

Detected files:

* `F:/sysvolume/u561052.bat`
* `F:/sysvolume/u145106.vbs`
* `F:/USB.lnk`

👉 এগুলো typical **USB Shortcut Virus pattern**

---

## 🧠 USB Shortcut Virus কী করে?

এই virus:

* তোমার file/folder **hidden করে দেয়**
* same name দিয়ে **shortcut (.lnk)** বানায়
* click করলে:

  * virus run হয়
  * original file hidden থাকে

📌 Example:

* `Photos` folder → hidden
* `Photos.lnk` → fake shortcut

---

## ⚠️ এটা dangerous কেন?

* অন্য PC-তে ছড়ায় (USB দিয়ে)
* auto run script চালায় (.bat / .vbs)
* background এ malicious command execute করে

---

## ✅ এখন কী করতে হবে (Step-by-step fix)

### 🔴 Step 1: USB disconnect করো

👉 অন্য PC-তে লাগাইও না

---

### 🔵 Step 2: CMD দিয়ে clean করো

1. **Command Prompt (Run as Admin)** খুলে নিচের command দাও:

```id="3y5a7s"
attrib -h -r -s /s /d F:\*.*
```

👉 এটা hidden file আবার visible করবে

---

### 🔵 Step 3: Virus file delete করো

Manual delete:

* `F:/sysvolume/` → পুরো folder delete করো
* `F:/USB.lnk` → delete করো

👉 যদি delete না হয়:

```id="0i1tcc"
del /f /a F:\*.lnk
```

---

### 🔵 Step 4: Original file check করো

👉 এখন তোমার আসল file/folder visible হবে
(আগে hidden ছিল)

---

### 🔵 Step 5: Full scan দাও

Use:

* **Windows Defender**
* অথবা Malwarebytes

👉 Full scan (Quick না)

---

## 🛡️ Future protection

### ✔️ USB use করার আগে:

* Shift ধরে open করো (autorun block করে)

### ✔️ Unknown PC-তে USB লাগানো avoid করো

### ✔️ Important:

👉 USB format করলে **100% clean হবে**

---

## 🎯 Final Verdict

👉 এখনকার report:

* ❗ Real virus (USB Shortcut Virus)
* ✅ কিন্তু **easily removable**
* 💥 system infect না, mainly USB drive-এ আছে

---

চাও হলে আমি তোমাকে **automatic one-click cleaner script (CMD)** বানিয়ে দিতে পারি — just run করলেই clean হয়ে যাবে 👍




