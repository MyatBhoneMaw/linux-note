# Linux File Systems နှင့် Link များအကြောင်း လေ့လာခြင်း (Accessing Linux File Systems)

Linux system တစ်ခုကို ကျွမ်းကျင်စွာ ကိုင်တွယ်နိုင်ဖို့အတွက် File System တွေအကြောင်းနဲ့ File/Directory link ချိတ်ဆက်ပုံတွေကို နားလည်ထားဖို့ လိုအပ်ပါတယ်

---

## ၁။ Linux Links (File ချိတ်ဆက်မှုများ)

Linux မှာ link ချိတ်တယ်ဆိုတာ file တစ်ခုကို နေရာအမျိုးမျိုးကနေ လှမ်းကြည့်လို့ရအောင် လုပ်တာပါ။

### Soft Link (Symbolic Link / Symlink)

Soft link ဆိုတာ Windows က Shortcut တွေနဲ့ တူပါတယ်။ သူက မူရင်း (Original) file ရဲ့ **လမ်းကြောင်း (Path)** ကိုပဲ ညွှန်ပြနေတာပါ။

- **အလုပ်လုပ်ပုံ:** မူရင်း file ကို ဖျက်လိုက်ရင် soft link က အလုပ်မလုပ်တော့ပါဘူး (Broken link ဖြစ်သွားမယ်)။
- **ထူးခြားချက်:** File ရော Directory ပါ link ချိတ်လို့ရပါတယ်။ Disk partition မတူတာတွေကိုလည်း ချိတ်လို့ရပါတယ်။

### Hard Link

Hard link ဆိုတာကတော့ file ရဲ့ data တွေ သိမ်းထားတဲ့ **Inode** (Data block link) ကို တိုက်ရိုက် ချိတ်တာပါ။

- **အလုပ်လုပ်ပုံ:** မူရင်း file ကို ဖျက်လိုက်ရင်တောင် hard link ရှိနေသရွေ့ data တွေ ရှိနေဦးမှာပါ။ File နှစ်ခုလုံးက data တစ်ခုတည်းကိုပဲ ပိုင်ဆိုင်ထားသလို ဖြစ်နေမှာပါ။
- **ထူးခြားချက်:** File တွေပဲ ချိတ်လို့ရပါတယ်။ Directory တွေကို hard link ချိတ်လို့ မရပါဘူး (System ရဲ့ structure ပျက်မှာစိုးလို့ပါ)။ Partition မတူရင်လည်း ချိတ်လို့မရပါ။

---

## ၂။ Link ချိတ်နည်း Command များ

### File တစ်ခုကို Soft Link ချိတ်နည်း

`ln -s [Original_File] [Link_Name]`

```bash
$ touch original.txt
$ ln -s original.txt softlink.txt
$ ls -l
lrwxrwxrwx 1 user user 12 Apr 20 2026 softlink.txt -> original.txt
```

_ရှင်းလင်းချက်:_ `original.txt` ထဲမှာ စာရေးရင် `softlink.txt` ထဲမှာလည်း ပေါ်နေပါမယ်။

### File တစ်ခုကို Hard Link ချိတ်နည်း

`ln [Original_File] [Link_Name]`

```bash
$ ln original.txt hardlink.txt
$ ls -i
12345 original.txt
12345 hardlink.txt
```

_ရှင်းလင်းချက်:_ `-i` နဲ့ကြည့်ရင် Inode နံပါတ် (12345) တူနေတာကို တွေ့ရပါမယ်။ ဒါဟာ data တစ်ခုတည်းကိုပဲ နာမည်နှစ်ခုပေးထားတာပါ။

### Directory တစ်ခုကို Soft Link ချိတ်နည်း

```bash
$ ln -s /var/log/ my_logs
```

_ရှင်းလင်းချက်:_ `my_logs` ထဲကိုဝင်ရင် `/var/log/` ထဲက data တွေကို မြင်ရပါမယ်။

---

## ၃။ Linux File Systems များ

- **ext2 (Second Extended File System):** ရှေးအကျဆုံး Linux file system ပါ။ Journaling function မပါတဲ့အတွက် မီးပျက်သွားရင် data ပျက်စီးဖို့ အခွင့်အလမ်းများပါတယ်။
- **ext3 (Third Extended File System):** ext2 ကို Journaling ထပ်ပေါင်းထည့်ထားတာပါ။ မီးပျက်ရင် data တွေကို ပြန်ဆယ်ဖို့ ပိုလွယ်လာပါတယ်။
- **ext4 (Fourth Extended File System):** လက်ရှိ Linux အများစုမှာ standard သုံးနေတဲ့ file system ပါ။ File size အကြီးကြီးတွေကို support လုပ်ပြီး performance ပိုကောင်းပါတယ်။
- **xfs:** High-performance file system တစ်ခုဖြစ်ပြီး server ကြီးတွေနဲ့ storage များတဲ့နေရာတွေမှာ သုံးပါတယ်။ RHEL (Red Hat) မှာ default သုံးပါတယ်။
- **Btrfs (Better FS):** Snapshot ရိုက်တာတွေ၊ storage pooling တွေပါတဲ့ ခေတ်မီ file system တစ်ခုပါ။

### NVMe ဆိုတာဘာလဲ?

**NVMe (Non-Volatile Memory Express)** ဆိုတာ SSD တွေအတွက် အထူးထုတ်လုပ်ထားတဲ့ Communication Protocol (ဆက်သွယ်ရေးစနစ်) တစ်ခုပါ။ SATA SSD တွေထက် အဆပေါင်းများစွာ ပိုမြန်ပါတယ်။

---

## ၄။ Monitoring & Management Commands

### `lsblk` (List Block Devices)

Disk storage တွေနဲ့ Partition တွေကို ဇယားနဲ့ ပြပေးပါတယ်။

```bash
$ lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0   20G  0 disk
└─sda1   8:1    0   20G  0 part /
```

### `ls -l /dev/sda`

SDA ဆိုတဲ့ physical disk file ရဲ့ အချက်အလက်ကို ကြည့်တာပါ။

```bash
$ ls -l /dev/sda
brw-rw---- 1 root disk 8, 0 Apr 20 2026 /dev/sda
```

### `ls -l /dev/mapper`

LVM (Logical Volume Manager) သုံးထားရင် virtual disk တွေကို ဒီလမ်းကြောင်းမှာ ပြပေးပါတယ်။

### `df -h` (Disk Free - Human Readable)

Partition တစ်ခုချင်းစီမှာ နေရာဘယ်လောက်လွတ်လဲဆိုတာ GB, MB နဲ့ ပြပေးပါတယ်။

```bash
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        20G   10G  8.5G  55% /
```

### `blkid` (Block ID)

Partition တွေရဲ့ **UUID** (Unique ID) နဲ့ File system အမျိုးအစားကို ပြပေးပါတယ်။

```bash
$ blkid
/dev/sda1: UUID="a1b2-c3d4" TYPE="ext4"
```

### Format ရိုက်ခြင်း (Creating File System)

`mkfs` ဆိုတာ Make File System ကို ပြောတာပါ။ Terminal မှာ `mkfs` လို့ရိုက်ပြီး `Tab` နှစ်ချက်နှိပ်ရင် ရနိုင်တဲ့ file system တွေကို ပြပါလိမ့်မယ်။

- **နည်းလမ်း ၁:** `mkfs -t ext4 /dev/sdb1`
- **နည်းလမ်း ၂:** `mkfs.ext4 /dev/sdb1` (ပိုသုံးကြတယ်)

---

## ၅။ Mounting (Disk ကို ချိတ်ဆက်အသုံးပြုခြင်း)

Disk တစ်ခုကို format ရိုက်ပြီးရင် folder တစ်ခုနဲ့ ချိတ်မှ သုံးလို့ရပါတယ်။ အဲဒါကို **Mount** လုပ်တယ်လို့ ခေါ်ပါတယ်။

### Mount လုပ်နည်း

```bash
$ sudo mount /dev/sdb1 /mnt/mydata
```

### UUID နဲ့ Mount လုပ်နည်း (ပိုစိတ်ချရတယ်)

Disk နာမည်တွေ (sda, sdb) က ပြောင်းလဲနိုင်လို့ UUID နဲ့ ချိတ်တာ အကောင်းဆုံးပါ။

```bash
$ sudo mount UUID="a1b2-c3d4" /mnt/mydata
```

### Unmount လုပ်နည်း

```bash
$ sudo umount /mnt/mydata
```

---

## ၆။ File ရှာဖွေခြင်း (Locate vs Find)

### Locate (Ubuntu မှာ plocate ကို အသုံးများလာပါတယ်)

`locate` က database ထဲမှာ ရှာတာဖြစ်လို့ အလွန်မြန်ပါတယ်။ ဒါပေမဲ့ file အသစ်ဆိုရင် database ကို update လုပ်ပေးရပါတယ်။

- **Update database:** `sudo updatedb`
- **ရှာနည်း:** `locate filename.txt`
- **Option:** `locate -i filename` (အကြီးအသေး မခွဲဘဲရှာတာ)

### Find Command

`find` ကတော့ real-time (တကယ်ရှိတဲ့နေရာမှာ) လိုက်ရှာတာမို့ ပိုသေချာပေမဲ့ နည်းနည်းကြာပါတယ်။

- **အမည်ဖြင့်ရှာရန်:** `find /home -name "myfile.txt"`
- **Size ဖြင့်ရှာရန်:** `find / -size +100M` (100MB ထက်ကြီးတာ ရှာတာ)
- **Type ဖြင့်ရှာရန်:** `find . -type d` (Directory တွေကိုပဲ ရှာတာ)

---

> **မှတ်ချက်:** အထက်ပါ command များကို အသုံးပြုသည့်အခါ `sudo` (Admin permission) လိုအပ်နိုင်ပါသည်။
