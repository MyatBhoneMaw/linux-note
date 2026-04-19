# APT (Advanced Package Tool) အသုံးပြုနည်း လမ်းညွှန်

Linux (Debian/Ubuntu) system များတွင် software များကို ထည့်သွင်းခြင်း၊ ဖယ်ရှားခြင်းနှင့် update ပြုလုပ်ခြင်းတို့ကို လွယ်ကူစွာ စီမံခန့်ခွဲနိုင်ရန် **APT (Advanced Package Tool)** ကို အသုံးပြုပါသည်။ ဤ document တွင် `apt` ၏ အခြေခံနှင့် အရေးကြီးသော command များကို နမူနာများနှင့်တကွ ရှင်းပြထားပါသည်။

---

## ၁။ APT ဆိုတာဘာလဲ?

APT ဆိုသည်မှာ Linux system ထဲသို့ software များ ထည့်သွင်းရာတွင် လိုအပ်သော file များကို အလိုအလျောက် ရှာဖွေ၊ ဒေါင်းလုဒ်လုပ်ပြီး install လုပ်ပေးသည့် **Package Manager (Software စီမံခန့်ခွဲသူ)** တစ်ခု ဖြစ်ပါသည်။

---

## ၂။ အခြေခံ Command များ (Basic Commands)

### (က) Software List ကို အသစ်ပြင်ဆင်ခြင်း (Updating Package List)

System ထဲမှာရှိတဲ့ software version တွေ အသစ်ထွက်မထွက် သိနိုင်ဖို့အတွက် repositories (software သိုလှောင်ရာနေရာ) ဆီက အချက်အလက်ကို အရင်ယူရပါမယ်။

```bash
sudo apt update
```

**ရှင်းလင်းချက်:** ဤ command သည် software အသစ်များကို install လုပ်ခြင်းမဟုတ်ဘဲ၊ မည်သည့် software များ update ထွက်နေသည်ကို စာရင်းစစ်ဆေးခြင်းသာ ဖြစ်ပါသည်။

**Example Output:**

```text
Hit:1 http://archive.ubuntu.com/ubuntu jammy InRelease
Get:2 http://archive.ubuntu.com/ubuntu jammy-updates InRelease [119 kB]
Reading package lists... Done
Building dependency tree... Done
```

---

### (ခ) Software များကို အဆင့်မြှင့်တင်ခြင်း (Upgrading Packages)

Update လုပ်ပြီးနောက် software များကို နောက်ဆုံးထွက် version သို့ ပြောင်းလဲလိုလျှင် အသုံးပြုပါသည်။

```bash
sudo apt upgrade
```

**ရှင်းလင်းချက်:** ဤ command သည် လက်ရှိ system ထဲရှိ software အားလုံးကို နောက်ဆုံး version (Latest Version) သို့ မြှင့်တင်ပေးပါသည်။

---

### (ဂ) Software အသစ်ထည့်သွင်းခြင်း (Installing a Package)

မိမိအသုံးပြုလိုသော software ကို install လုပ်ရန် အသုံးပြုပါသည်။

```bash
sudo apt install <software_name>
```

_ဥပမာ - `git` ကို ထည့်သွင်းလိုလျှင်:_ `sudo apt install git`

**Example Output:**

```text
Reading package lists... Done
The following NEW packages will be installed:
  git
0 upgraded, 1 newly installed, 0 to remove.
After this operation, 18.9 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
```

---

### (ဃ) Software ကို ပြန်လည်ဖယ်ရှားခြင်း (Removing a Package)

မလိုအပ်တော့သော software များကို system ထဲမှ ဖယ်ထုတ်ရန် အသုံးပြုပါသည်။

```bash
sudo apt remove <software_name>
```

**မှတ်ချက်:** `remove` သည် software ကိုသာ ဖြုတ်ပေးပြီး ၎င်း၏ configuration files (ဆက်တင်ဖိုင်များ) ကို ချန်ထားခဲ့ပါမည်။ အားလုံးကို အပြီးတိုင်ဖြုတ်လိုလျှင် `sudo apt purge <software_name>` ကို သုံးနိုင်ပါသည်။

---

### (င) Software ရှာဖွေခြင်း (Searching for a Package)

မိမိသွင်းချင်သည့် software နာမည် အတိအကျကို မသိလျှင် သို့မဟုတ် ရှိမရှိ စစ်ဆေးလိုလျှင် သုံးပါသည်။

```bash
apt search <keyword>
```

_ဥပမာ - web browser တစ်ခု ရှာလိုလျှင်:_ `apt search browser`

---

### (စ) မလိုအပ်သော File များကို ရှင်းလင်းခြင်း (Cleaning up)

Software တွေ သွင်းပြီးနောက် ကျန်ခဲ့တဲ့ မလိုအပ်တဲ့ installation files (cached files) တွေကို ဖျက်ထုတ်ပြီး storage နေရာလွတ်ရအောင် လုပ်ပေးတာ ဖြစ်ပါတယ်။

```bash
sudo apt autoremove
```

**ရှင်းလင်းချက်:** အခြား software များအတွက် မလိုအပ်တော့သော **Dependencies (ဆက်စပ်ဖိုင်များ)** ကို အလိုအလျောက် ရှာဖွေဖယ်ရှားပေးပါသည်။

---

## ၃။ အသုံးဝင်သော Command အနှစ်ချုပ် (Quick Summary)

| Command                     | အသုံးပြုပုံ                                       |
| :-------------------------- | :------------------------------------------------ |
| `sudo apt update`           | Software list (စစ်ဆေးစာရင်း) ကို update လုပ်ရန်   |
| `sudo apt upgrade`          | ရှိပြီးသား software တွေကို version မြှင့်ရန်      |
| `sudo apt install`          | Software အသစ်သွင်းရန်                             |
| `sudo apt remove`           | Software ဖြုတ်ရန်                                 |
| `sudo apt search`           | Software နာမည် ရှာဖွေရန်                          |
| `sudo apt list --installed` | System ထဲမှာ သွင်းထားသမျှ software စာရင်းကြည့်ရန် |

---

## ၄။ သတိပြုရန် အချက်များ

1.  **sudo (Superuser Do):** `apt` command အများစုသည် system ကို ပြောင်းလဲစေနိုင်သောကြောင့် အရှေ့မှ `sudo` (Admin အခွင့်အရေး) ခံပြီး ရိုက်ရပါမည်။
2.  **Internet Connection:** Software များ ဒေါင်းလုဒ်လုပ်ရန် အင်တာနက် လိုအပ်ပါသည်။
3.  **Dependencies (ဆက်စပ်ဖိုင်များ):** Linux တွင် software တစ်ခု သွင်းရန် နောက်ထပ် software လိုအပ်ချက်များ ရှိတတ်ပါသည်။ `apt` သည် ၎င်းတို့ကို အလိုအလျောက် တွက်ချက်ပြီး တစ်ခါတည်း သွင်းပေးပါသည်။

---
