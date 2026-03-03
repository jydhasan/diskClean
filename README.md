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
> **Tested on:** DISKPART version 10.0.19041.3636
