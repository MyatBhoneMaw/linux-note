# 🐧 Linux System Administration: Controlling Services and Daemons

ဒီ Guide မှာ Linux OS ရဲ့ နောက်ကွယ်က အလုပ်လုပ်နေတဲ့ process တွေကို ဘယ်လို စီမံခန့်ခွဲမလဲဆိုတာကို အသေးစိတ် လေ့လာနိုင်ပါတယ်။

## 1. Systemd ဆိုတာ ဘာလဲ?

**Systemd** ဆိုတာ Linux Operating System တက်လာကတည်းက စတင်အလုပ်လုပ်တဲ့ **System and Service Manager** (init system) ဖြစ်ပါတယ်။ သူက System ရဲ့ ပထမဆုံး process (PID 1) အနေနဲ့ run ပြီး ကျန်တဲ့ service တွေအားလုံးကို သူကပဲ ထိန်းချုပ်မောင်းနှင်ပေးတာပါ။

### Unit Files (ယူနစ် ဖိုင်များ)

Systemd က အရာအားလုံးကို **Units** အနေနဲ့ မြင်ပါတယ်။ အသုံးများတဲ့ Unit အမျိုးအစားတွေကတော့ -

- **Service Unit (.service):** Linux မှာ run နေတဲ့ application ဒါမှမဟုတ် process တွေကို ကိုယ်စားပြုပါတယ်။ (ဥပမာ- Apache, MySQL)
- **Socket Unit (.socket):** Service တစ်ခုနဲ့တစ်ခု ချိတ်ဆက်ဖို့ ဒါမှမဟုတ် network ကနေ data လက်ခံဖို့ အသုံးပြုတဲ့ လမ်းကြောင်း (Inter-process communication) ဖြစ်ပါတယ်။
- **Path Unit (.path):** သတ်မှတ်ထားတဲ့ File ဒါမှမဟုတ် Folder လမ်းကြောင်းမှာ အပြောင်းအလဲဖြစ်ရင် Service တစ်ခုခုကို trigger လုပ်ဖို့ (အလုပ်ပေးဖို့) သုံးပါတယ်။

## 2. Daemons (ဒေမွန်များ)

**Daemon** ဆိုတာကတော့ User ရဲ့ တိုက်ရိုက်ထိန်းချုပ်မှုမပါဘဲ Background မှာ အမြဲတမ်း run နေတဲ့ process တွေကို ခေါ်တာပါ။ များသောအားဖြင့် သူတို့ရဲ့ နာမည်အဆုံးမှာ `d` ဆိုတဲ့ စာလုံးလေး ပါလေ့ရှိပါတယ်။ (ဥပမာ - `sshd`, `httpd`)

---

## 3. Systemctl Command အသုံးပြုနည်းများ

`systemctl` ကတော့ service တွေကို control လုပ်တဲ့ အဓိက command ဖြစ်ပါတယ်။

### Basic Management

- **Start a service:** Service တစ်ခုကို စတင်မောင်းနှင်ခြင်း။
  ```bash
  sudo systemctl start nginx
  ```
- **Check Status:** Service တစ်ခုရဲ့ အခြေအနေ (Running လား၊ Stopped လား) ကို စစ်ဆေးခြင်း။
  ```bash
  sudo systemctl status nginx
  ```
  _Output Example:_
  ```text
  ● nginx.service - A high performance web server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2026-04-15 20:28:40 UTC; 5min ago
  ```

### Reload vs Restart (အရေးကြီးသော ခြားနားချက်)

- **Reload:** Process ကို မပိတ်ဘဲ Config file အသစ်တွေကိုပဲ ပြန်ဖတ်ခိုင်းတာပါ။ Process ID (PID) မပြောင်းလဲပါဘူး။
  ```bash
  sudo systemctl reload nginx
  ```
- **Restart:** Service ကို အပြီးသတ်သတ် (Kill) ပြီးမှ အစကနေ ပြန်ဖွင့်တာပါ။ **Process ID (PID) အသစ် ပြောင်းသွားမှာဖြစ်သလို** လက်ရှိသုံးနေတဲ့ memory နဲ့ session တွေလည်း ပြတ်တောက်သွားမှာ ဖြစ်ပါတယ်။
  ```bash
  sudo systemctl restart nginx
  ```

---

## 4. Service အခြေအနေများကို စစ်ဆေးခြင်း

| Command                                         | Explanation (ရှင်းလင်းချက်)                                                                          |
| :---------------------------------------------- | :--------------------------------------------------------------------------------------------------- |
| `sudo systemctl`                                | လက်ရှိ active ဖြစ်နေတဲ့ unit တွေနဲ့ သူတို့ရဲ့ dependencies (မှီခိုမှု) အားလုံးကို ပြပေးပါတယ်။        |
| `sudo systemctl --type=service`                 | System မှာရှိတဲ့ Service unit တွေကိုပဲ သီးသန့်စစ်ထုတ်ကြည့်တာပါ။                                      |
| `sudo systemctl list-unit-files --type=service` | မောင်းနှင်လို့ရတဲ့ service file အားလုံးနဲ့ သူတို့ရဲ့ state (enabled/disabled) ကို စာရင်းပြုစုပြတာပါ။ |
| `sudo systemctl is-enabled [name]`              | Service က စက်ဖွင့်တာနဲ့ အလိုအလျောက် ပွင့်အောင် လုပ်ထားသလား (Enabled ဖြစ်လား) စစ်တာပါ။                |
| `sudo systemctl is-active [name]`               | Service က လက်ရှိ အလုပ်လုပ်နေသလား (Running ဖြစ်လား) စစ်တာပါ။                                          |
| `sudo systemctl is-failed [name]`               | Service က run ဖို့ ကြိုးစားရင်း error တက်ပြီး ရပ်သွားသလား (Failed state) စစ်တာပါ။                    |

---

## 5. Enable, Disable နှင့် Masking

Service တစ်ခုကို စက်တက်လာရင် အလိုအလျောက် ပွင့်စေချင်တာလား၊ လုံးဝ ပိတ်ထားချင်တာလား ဆိုတာကို ဒီမှာ စီမံပါတယ်။

### Enable & Disable

- **Enable:** စက် (Boot) တက်လာတာနဲ့ Service ကို အလိုအလျောက် စတင်ခိုင်းတာပါ။ (Link ချိတ်ပေးတာ ဖြစ်လို့ Startup မှာ အလုပ်လုပ်သွားမယ်)
  ```bash
  sudo systemctl enable nginx
  ```
- **Disable:** စက်တက်လာရင် အလိုအလျောက် မပွင့်အောင် လုပ်တာပါ။ (ဒါပေမဲ့ `start` command နဲ့ လက်တန်း ဖွင့်လို့ရသေးတယ်)
  ```bash
  sudo systemctl disable nginx
  ```

### Masking (အဆင့်မြင့် ပိတ်ဆို့ခြင်း)

- **Mask:** Service တစ်ခုကို ဘယ်နည်းနဲ့မှ (Manual ရော Auto ရော) run လို့မရအောင် `/dev/null` ဆီ Link ချိတ်ပြီး လုံးဝ ပိတ်ပစ်တာပါ။ တခြား service တစ်ခုခုက လှမ်းခေါ်ရင်တောင် ပွင့်လာမှာ မဟုတ်ပါဘူး။
  ```bash
  sudo systemctl mask nginx
  ```
- **Unmask:** ပိတ်ထားတဲ့ Service ကို ပြန်ဖွင့်ခွင့်ပေးတာပါ။
  ```bash
  sudo systemctl unmask nginx
  ```

### Static Service ဆိုတာဘာလဲ?

**Static Service** ဆိုတာကတော့ သူ့အလိုလို `enable` လုပ်လို့မရတဲ့ service တွေပါ။ သူတို့ဟာ များသောအားဖြင့် တခြား service တစ်ခုခုက လိုအပ်မှသာ Systemd က လှမ်းခေါ်ပြီး အလုပ်လုပ်ပေးရတဲ့ အကူ (Dependency) service တွေ ဖြစ်ပါတယ်။ ဒါကြောင့် သူတို့ကို User က manually enable/disable လုပ်လို့ မရတာပါ။

---

> **Tip:** System configuration တွေ ပြောင်းလဲပြီးတိုင်း unit files တွေကို systemd က သိအောင် `sudo systemctl daemon-reload` လုပ်ပေးဖို့ မမေ့ပါနဲ့။
