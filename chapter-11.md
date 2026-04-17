# 🐧 Linux Networking အခြေခံနှင့် အသုံးချနည်းများ (Guide for Beginners)

Linux စနစ်တစ်ခုတွင် Network ချိတ်ဆက်မှုများကို စီမံခန့်ခွဲပုံ၊ အသုံးများသော Command များ နှင့် ၎င်းတို့၏ အလုပ်လုပ်ပုံများကို ဤနေရာတွင် စုစည်းဖော်ပြပေးထားပါသည်။

## 📍 အခြေခံသိမှတ်ဖွယ်ရာများ (Basic Concepts)

Linux တွင် Network ချိတ်ဆက်မှုကို Kernel (ကားနယ်လ် - OS ၏ အဓိက အစိတ်အပိုင်း) က ထိန်းချုပ်သည်။

1.  **Network Card Interface Name:** Network Card အမည်ကို Kernel က အလိုအလျောက် သတ်မှတ်ပေးသည်။ (ဥပမာ - `eth0`, `enp0s3`, `wlan0`)
2.  **Network Connection Name:** ၎င်း Interface ကို အသုံးပြုမည့် ချိတ်ဆက်မှုအမည် (Connection Name) ကိုမူ မိမိစိတ်ကြိုက် သတ်မှတ်ပေးနိုင်ပါသည်။ (ဥပမာ - "Home-WiFi", "Office-Ethernet")

### Network ကို ထိန်းချုပ်ရန် နည်းလမ်း (၃) မျိုး

- **nm-connection-editor (nmgui):** Graphical User Interface (ဂရပ်ဖစ်မျက်နှာပြင်) ဖြင့် Mouse သုံး၍ Setting ချခြင်း။
- **nmcli (Network Manager Command Line Interface):** Terminal (တမ်မီနယ်လ်) မှ Command ရိုက်၍ Network ကို အပြည့်အဝ ထိန်းချုပ်ခြင်း။ (Server များတွင် အသုံးအများဆုံး)
- **nmtui (Network Manager Text User Interface):** Terminal ထဲတွင် Keyboard (Arrow keys) များ သုံး၍ အမြင်ရလွယ်ကူသော Box လေးများဖြင့် Setting ချခြင်း။

---

## 🛠 လိုအပ်သော Tool များ ထည့်သွင်းခြင်း

Linux version အသစ်အချို့တွင် networking command အဟောင်းများ ပါမလာတတ်ပါ။ ထို့ကြောင့် အောက်ပါ command ဖြင့် အရင် install လုပ်ရန် လိုအပ်ပါသည်။

```bash
sudo apt update
sudo apt install net-tools
```

---

## 📟 Network Commands နှင့် အသေးစိတ်ရှင်းလင်းချက်များ

### ၁။ ifconfig (Interface Configuration)

Network Interface များ၏ အခြေအနေ (IP address, MAC address, Data အတက်အကျ) ကို ကြည့်ရန် သုံးသည်။

- **Output ရှင်းလင်းချက်:** `inet` သည် သင်၏ IPv4 address ဖြစ်ပြီး၊ `ether` သည် သင်၏ Hardware (MAC) address ဖြစ်သည်။ `RX/TX` သည် data လက်ခံရရှိမှုနှင့် ပေးပို့မှုကို ပြသည်။

### ၂။ ip -s link show

Network Interface (ကတ်များ) ၏ အခြေအနေနှင့်တကွ Statistics (စာရင်းဇယား) များကို အသေးစိတ်ပြသည်။ Packet ဘယ်နှစ်ခု ပို့လိုက်ပြီလဲ၊ error ဘယ်နှစ်ခု ရှိလဲဆိုတာကိုပါ ပြပေးသည်။

### ၃။ route -n

Routing Table (လမ်းကြောင်းပြဇယား) ကို ကြည့်ရန် သုံးသည်။ `-n` သည် အမည်များအစား ကိန်းဂဏန်း (Numerical) ဖြင့် မြန်မြန်ဆန်ဆန် ပြခိုင်းခြင်းဖြစ်သည်။

- **Gateway:** အင်တာနက်ထွက်ပေါက် (Router) ၏ IP address ကို ပြသည်။

### ၄။ ip route

`route -n` နှင့် ဆင်တူသော်လည်း ပို၍ ခေတ်မီသော command ဖြစ်သည်။ Default gateway ဘယ်က သွားနေသလဲဆိုတာကို တစ်ကြောင်းတည်းဖြင့် ရှင်းရှင်းလင်းလင်း ပြပေးသည်။

### ၅။ ping 8.8.8.8

Network ချိတ်ဆက်မှု ရှိမရှိ စစ်ဆေးခြင်း ဖြစ်သည်။ `8.8.8.8` (Google DNS) သို့ data packet လေးများ ပို့ကြည့်ပြီး ပြန်လာမလာ စစ်ဆေးသည်။ IP သုံး၍ စစ်ခြင်းဖြစ်သဖြင့် အင်တာနက်လိုင်း တကယ်တက်မတက် သိနိုင်သည်။

### ၆။ ping [www.google.com](https://www.google.com)

အထက်ပါအတိုင်းပင် ဖြစ်သော်လည်း Domain Name (အမည်) သုံး၍ စစ်ခြင်းဖြစ်သည်။ အကယ်၍ IP နှင့် ping ရပြီး ဤ command နှင့် ping မရပါက DNS (အမည်ကို IP ပြောင်းပေးသည့်စနစ်) အလုပ်မလုပ်ခြင်း ဖြစ်သည်။

### ၇။ cat /etc/hosts

မိမိစက်ထဲတွင် IP အမည်များကို ကိုယ်တိုင် သတ်မှတ်ထားသော ဖိုင်ကို ဖတ်ခြင်းဖြစ်သည်။ (ဥပမာ - `127.0.0.1 localhost`)

### ၈။ netstat (Network Statistics)

Network connections တွေအားလုံး (ဘယ်စက်နဲ့ ချိတ်နေလဲ၊ ဘယ် port တွေ ဖွင့်ထားလဲ) ကို ပြပေးသည်။

### ၉။ netstat -nutl

ဖွင့်ထားသော Port (Listening Ports) များကို သေချာစစ်ဆေးလိုလျှင် သုံးသည်။

- `-n`: IP address များကို နံပါတ်အတိုင်းပြရန်။
- `-u`: UDP protocol ကိုပြရန်။
- `-t`: TCP protocol ကိုပြရန်။
- `-l`: Listening (လက်ခံဖို့ စောင့်နေတဲ့) port များကိုသာပြရန်။
- **Columns:** `Proto` (အမျိုးအစား), `Local Address` (မိမိစက် IP နှင့် Port), `State` (အခြေအနေ - LISTEN ဖြစ်နေရင် အလုပ်လုပ်နေသည်)။

### ၁၀။ ss (Socket Statistics)

`netstat` ထက် ပိုမြန်ပြီး ပိုမိုအသေးစိတ်သော command ဖြစ်သည်။ လက်ရှိ ခေတ်ပေါ် Linux များတွင် netstat အစား ၎င်းကို သုံးလာကြသည်။

---

## 📡 nmcli (Network Manager CLI) အသုံးပြုနည်းများ

ဤ command များသည် Network Setting များကို ပြောင်းလဲရာတွင် အလွန်အရေးကြီးသည်။

- **nmcli con show:** လက်ရှိစက်ထဲတွင် ရှိသမျှ Connection profile အားလုံးကို ပြသည်။ (Column များ: `NAME` - ချိတ်ဆက်မှုအမည်၊ `UUID` - သီးသန့် ID၊ `TYPE` - အမျိုးအစား၊ `DEVICE` - ဘယ်ကတ်မှာ သုံးနေသလဲ)။
- **nmcli:** Network ၏ အခြေအနေ အကျဉ်းချုပ်ကို ပြသည်။
- **nmcli con s:** `show` ကို အတိုချုတ် `s` ဟု ရေးခြင်းဖြစ်သည်။ Connection စာရင်းပြသည်။
- **nmcli con -s --active:** လက်ရှိ တကယ် အလုပ်လုပ်နေသော (Active ဖြစ်နေသော) connection များကိုသာ ပြသည်။
- **nmcli connection down [Name]:** ၎င်း Network connection ကို ပိတ်လိုက်ခြင်းဖြစ်သည်။ `down` အစား `d` ဟု အတိုချုတ် ရေး၍ ရပါသည်။
- **nmcli con up [Name]:** ပိတ်ထားသော connection ကို ပြန်ဖွင့်ခြင်းဖြစ်သည်။
- **nmcli con show "Name":** ၎င်း connection တစ်ခုတည်း၏ Setting များ (IP, Gateway, DNS) အားလုံးကို အသေးစိတ် ဖတ်ခြင်းဖြစ်သည်။
- **nmcli con add con-name MyNetwork type ethernet ifname enp0s3:** `MyNetwork` အမည်ဖြင့် connection အသစ်တစ်ခု ဆောက်ခြင်းဖြစ်သည်။ `enp0s3` ဆိုသော ကတ်ကို သုံးမည်ဟု သတ်မှတ်ခြင်းဖြစ်သည်။
- **nmcli con delete "MyNetwork":** တည်ဆောက်ထားသော connection profile ကို ဖျက်ပစ်ခြင်းဖြစ်သည်။
- **nmcli dev status:** Network ကတ် (Device) တစ်ခုချင်းစီ၏ အခြေအနေ (ချိတ်ထားသလား၊ လွတ်နေသလား) ကို ပြသည်။ `status` အစား `s` ဟု ရေးနိုင်သည်။
- **nmcli dev show [Interface]:** ၎င်း network ကတ်၏ အချက်အလက်များကို အသေးစိတ်ပြသည်။

### IP Address အသေ (Static IP) သတ်မှတ်နည်း

```bash
nmcli con add con-name MyNetwork type ethernet ifname enp0s3 ipv4.addresses 192.168.0.160/24 ipv4.gateway 192.168.0.1 ipv4.method manual
```

- `ipv4.method manual` သည် IP ကို ကိုယ်တိုင်အသေသတ်မှတ်မည် (Static) ဟု ဆိုလိုခြင်းဖြစ်သည်။

### Connection Modify (ပြင်ဆင်ခြင်း)

- **nmcli con modify mynetwork connection.autoconnect yes:** စက်တက်လာသည်နှင့် အလိုအလျောက် ချိတ်စေရန်။
- **nmcli con modify mynetwork ipv4.dns 8.8.8.8:** DNS ကို ပြောင်းလဲခြင်း။
- **nmcli con modify mynetwork +ipv4.dns 8.8.4.4:** ရှိပြီးသား DNS ထဲသို့ နောက်ထပ် DNS တစ်ခု ထပ်ပေါင်းထည့်ခြင်း (`+` သင်္ကေတ သုံးသည်)။

---

## 📛 Name Resolution & Hostname

### Hostname ဆိုသည်မှာ

Network ပေါ်တွင် သင့်စက်ကို အခြားစက်များက ခေါ်ဆိုနိုင်သည့် "စက်၏အမည်" ဖြစ်သည်။

- **hostnamectl:** လက်ရှိ စက်အမည်နှင့် OS အချက်အလက်များကို ကြည့်ရန်။
- **hostnamectl set-hostname myserver:** စက်၏ အမည်ကို `myserver` ဟု ပြောင်းလဲသတ်မှတ်ရန်။
- **exec bash:** Hostname ပြောင်းပြီးနောက် Terminal တွင် ချက်ချင်း အမည်သစ်ပေါ်လာစေရန် Shell ကို Refresh လုပ်ခြင်းဖြစ်သည်။
- **hostname test:** စက်အမည်ကို ခေတ္တခဏ (ယာယီ) ပြောင်းခြင်းဖြစ်သည်။ စက် Restart ချလျှင် ပျက်သွားမည်။

### Name Resolution (အမည်မှ IP သို့ ပြောင်းလဲခြင်း)

Linux တွင် အမည်များကို IP အဖြစ် ပြောင်းလဲရန် နေရာ (၂) ခုတွင် သတ်မှတ်နိုင်သည်-

1.  **/etc/hosts:** ကိုယ်ပိုင် အမြန်လမ်း (Local entries)။ (ဥပမာ- `192.168.0.10  my-database-server`)
2.  **/etc/resolv.conf:** အင်တာနက်ပေါ်ရှိ DNS Server လိပ်စာများ။ (ဥပမာ- `nameserver 8.8.8.8`)

---
