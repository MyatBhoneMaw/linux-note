# 🐧 Linux File System Permissions Guide (Myanmar)

Linux system တစ်ခုမှာ ဖိုင်တွေနဲ့ ဖိုဒါ (Directory) တွေကို ဘယ်သူက ဘာလုပ်ပိုင်ခွင့်ရှိလဲဆိုတာ သတ်မှတ်ပေးတာကို **Permissions** လို့ ခေါ်ပါတယ်။ ဒီ Guide မှာ အခြေခံကနေ အဆင့်မြင့် Special Permissions အထိ အသေးစိတ် ရှင်းပြပေးသွားပါမယ်။

---

## ၁။ အခြေခံ Permission (၃) မျိုး (Basic Permissions)

Linux မှာ ခွင့်ပြုချက် (Access) ကို အဓိက ၃ မျိုး ခွဲခြားထားပါတယ်။

| Permission  | သင်္ကေတ | တန်ဖိုး (Digit) | ဖိုင်အတွက် အဓိပ္ပာယ်                          | Directory အတွက် အဓိပ္ပာယ်                                     |
| :---------- | :-----: | :-------------: | :-------------------------------------------- | :------------------------------------------------------------ |
| **Read**    |   `r`   |      **4**      | ဖိုင်ထဲက အချက်အလက်ကို ဖတ်လို့ရခြင်း           | Directory ထဲက ဖိုင်စာရင်းကို `ls` နဲ့ ကြည့်လို့ရခြင်း         |
| **Write**   |   `w`   |      **2**      | ဖိုင်ကို ပြင်လို့ရခြင်း၊ ဖျက်လို့ရခြင်း       | Directory ထဲမှာ ဖိုင်အသစ်ဆောက်ခြင်း၊ ဖျက်ခြင်း လုပ်လို့ရခြင်း |
| **Execute** |   `x`   |      **1**      | Script သို့မဟုတ် Program အဖြစ် run လို့ရခြင်း | Directory ထဲကို `cd` နဲ့ ဝင်လို့ရခြင်း (မဖြစ်မနေ ပေးသင့်သည်)  |

---

## ၂။ ဘယ်သူ့ကို Permission ပေးမှာလဲ? (Who?)

Permission သတ်မှတ်တဲ့နေရာမှာ အုပ်စု ၃ စု ခွဲခြားထားပါတယ်။

- **u (User/Owner):** ဖိုင်ကို ပိုင်ဆိုင်တဲ့သူ။
- **g (Group):** ဖိုင်ကို ပိုင်ဆိုင်တဲ့ အဖွဲ့အစည်း။
- **o (Others):** Owner လည်းမဟုတ်၊ Group ထဲမှာလည်းမပါတဲ့ တခြား user များ။
- **a (All):** အပေါ်က သုံးခုလုံးကို ဆိုလိုတာပါ။

---

## ၃။ `chmod` Command အသုံးပြုပုံ (Changing Permissions)

Command တည်ဆောက်ပုံမှာ `<ဘယ်သူ့ကိုလဲ> <ဘာလုပ်မှာလဲ> <ဘယ် Permission လဲ>` ဆိုပြီး သွားပါတယ်။

- `+` : Permission အသစ်ထည့်မယ်။
- `-` : ရှိပြီးသား Permission ကို ပြန်ဖြုတ်မယ်။
- `=` : ရှိသမျှကို ဖျက်ပြီး အသစ်နဲ့ အစားထိုး (Overwrite) မယ်။

### Symbolic Mode (အက္ခရာဖြင့် သုံးခြင်း)

```bash
# owner user ကို execute လုပ်ခွင့်ပေးမယ်
chmod u+x filename

# owner ကို read/write ပေးပြီး execute ပြန်ဖြုတ်မယ်
chmod u+rw,u-x filename

# အားလုံး (all) ကို read/write ပေးမယ်
chmod a+rw filename

# sudo သုံးပြီး -x ပေးရင် default အနေနဲ့ all (a) လို့ သတ်မှတ်ပါတယ်
sudo chmod -x testFileTwo
```

### Numeric Mode (နံပါတ်ဖြင့် သုံးခြင်း)

နံပါတ်တွေပေါင်းပြီး တစ်ခါတည်း ပေးတာက ပိုမြန်ပါတယ်။

- `7` = 4(r) + 2(w) + 1(x) -> Full access
- `6` = 4(r) + 2(w) -> Read & Write
- `5` = 4(r) + 1(x) -> Read & Execute
- `4` = Read only

**ဥပမာ -** `chmod 755 testFile`
(Owner=7, Group=5, Others=5)

---

## ၄။ ပိုင်ရှင်ပြောင်းခြင်း (Ownership) - `chown` & `chgrp`

- **Group ပြောင်းရန်:** `sudo chgrp [groupName] [fileName]`
- **Owner နှင့် Group တစ်ခါတည်းပြောင်းရန်:** `sudo chown [owner]:[group] [fileName]`

**အတိုကောက်ရေးနည်းများ:**

```bash
# Owner မပြောင်းဘဲ Group ပဲ ပြောင်းချင်ရင် (: ရဲ့ နောက်မှာ group နာမည်ရေး)
sudo chown :adminteam myFolder

# Owner ရော Group ပါ တစ်ဦးတည်းဖြစ်အောင် ပြောင်းချင်ရင် (: ကို နာမည်နောက်မှာကပ်ရေး)
sudo chown root: myFolder
```

---

## ၅။ Directory Permissions နှင့် Recursive `-R`

Directory တွေကို permission ပေးတဲ့အခါ `X` (အကြီး) ကို သုံးလေ့ရှိပါတယ်။

- `x` (အသေး): ဖိုင်ရော folder ရောကို execute ပေးတာ။
- `X` (အကြီး): Directory တွေကိုပဲ execute ပေးပြီး၊ သာမန်ဖိုင်တွေကိုတော့ မပေးဘဲ ချန်ထားခဲ့တာ (ပိုလုံခြုံပါတယ်)။

**Recursive Option:**
Main directory အောက်မှာရှိတဲ့ sub-folders နဲ့ files တွေအကုန်လုံးကို တစ်ခါတည်း permission ပြောင်းချင်ရင် `-R` ကို သုံးပါတယ်။

```bash
sudo chmod -R 755 MyDirectory/
```

---

## ၆။ `ls -l` Output ကို ဖတ်နည်း (Understanding Output)

ဥပမာ output တစ်ခုကို ကြည့်ရအောင် -
`drwxrwxr-x 2 bue adminteam 4096 Apr 13 02:42 TestDirectory`

| အစိတ်အပိုင်း           | အဓိပ္ပာယ်                                                           |
| :--------------------- | :------------------------------------------------------------------ |
| **d**                  | Directory အမျိုးအစား (ဖိုင်ဆိုရင် `-` လို့ ပြပါမယ်)                 |
| **rwx** (ပထမ ၃ လုံး)   | **User (Owner)** ရဲ့ permission (Read, Write, Execute ရတယ်)         |
| **rwx** (ဒုတိယ ၃ လုံး) | **Group** ရဲ့ permission (Read, Write, Execute ရတယ်)                |
| **r-x** (တတိယ ၃ လုံး)  | **Others** ရဲ့ permission (ဖတ်လို့ရတယ်၊ ဝင်လို့ရတယ်၊ ပြင်လို့မရဘူး) |
| **2**                  | Hard links အရေအတွက်                                                 |
| **bue**                | Owner User နာမည်                                                    |
| **adminteam**          | Owner Group နာမည်                                                   |
| **4096**               | File size (Bytes)                                                   |
| **Apr 13...**          | နောက်ဆုံး ပြင်ဆင်ခဲ့သည့် အချိန်                                     |
| **TestDirectory**      | ဖိုင် သို့မဟုတ် Folder နာမည်                                        |

---

## ၇။ Special Permissions (SSETUID, SETGID, Sticky Bit)

အထူးခွင့်ပြုချက်တွေက ပိုမိုရှုပ်ထွေးတဲ့ လုံခြုံရေးအတွက် သုံးပါတယ်။

### ၁. SETUID (Set User ID) - `s=4`

ဖိုင်တစ်ခုကို run တဲ့အခါ အဲဒီဖိုင်ရဲ့ owner ရဲ့ အခွင့်အရေးနဲ့ run စေတာပါ။ (ဥပမာ: `passwd` command)

- ပေးနည်း: `chmod u+s file` သို့မဟုတ် `chmod 4755 file`
- ပြသပုံ: `rws` (x ရှိရင် s အသေး၊ x မရှိရင် S အကြီး)

### ၂. SETGID (Set Group ID) - `s=2`

**Directory** တွေမှာ အသုံးများပါတယ်။ ဒီ permission ပေးထားတဲ့ folder ထဲမှာ ဘယ်သူပဲဖိုင်ဆောက်ဆောက်၊ အဲဒီဖိုင်ရဲ့ Group က folder ရဲ့ Group အတိုင်း အလိုအလျောက် ဖြစ်သွားတာပါ။ Teamwork လုပ်တဲ့အခါ အသုံးဝင်ပါတယ်။

- ပေးနည်း: `chmod g+s directory`

### ၃. Sticky Bit - `t=1`

ဒါက Directory တွေအတွက်ပါ။ ပေးထားရင် အဲဒီ folder ထဲက ဖိုင်တွေကို owner ကလွဲရင် ကျန်တဲ့သူတွေ (Write permission ရှိရင်တောင်) ဖျက်လို့မရအောင် ကာကွယ်ပေးတာပါ။ (ဥပမာ: `/tmp` folder)

- ပေးနည်း: `chmod o+t directory`

**ခွဲခြားချက်:**

- **SETGID:** Shared Group အတွက် (ဖိုင်အသစ်တွေ Group အတူတူဖြစ်အောင်)။
- **Sticky Bit:** သူများဖိုင် လိုက်မဖျက်နိုင်အောင် (လုံခြုံရေးအတွက်)။

---

## ၈။ umask (Default Permissions)

ဖိုင်အသစ် ဒါမှမဟုတ် Folder အသစ်ဆောက်ရင် အလိုအလျောက်ပါလာမယ့် Permission ကို `umask` က ထိန်းချုပ်ပါတယ်။

- Linux ရဲ့ Maximum Permission: File (`666`), Directory (`777`)
- **တွက်ချက်ပုံ:** `Max Permission - umask = Final Permission`

**ဥပမာ:**
umask က `002` ဆိုရင်:

- File: `666 - 002 = 664` (Owner/Group က ပြင်လို့ရ၊ Others က ဖတ်ရုံပဲ)
- Directory: `777 - 002 = 775`

### Secure ဖြစ်အောင် ဘယ်လိုထားမလဲ?

- **Root User:** များသောအားဖြင့် `022` ထားပါတယ်။ (Others တွေက ပြင်လို့မရအောင်)
- **Normal User:** `002` သို့မဟုတ် ပိုလုံခြုံချင်ရင် `022` သို့မဟုတ် `077` (မိမိကလွဲရင် ဘယ်သူမှကြည့်လို့မရအောင်) ထားသင့်ပါတယ်။

```bash
# umask ပြောင်းရန်
umask 027
```

> **မှတ်ချက်:** `umask` တန်ဖိုး များလေလေ၊ permission တွေ ပိုပိတ်လေလေ (ပိုလုံခြုံလေလေ) ဖြစ်ပါတယ်။

---
