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


