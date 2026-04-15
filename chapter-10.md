# Linux Log များအား စစ်ဆေးခြင်းနှင့် သိမ်းဆည်းခြင်း (Analyzing and Storing Logs)

Linux system တစ်ခုမှာ ဘာတွေဖြစ်ပျက်နေသလဲ (ဥပမာ - application တစ်ခု error တက်တာ၊ user တစ်ယောက် login ဝင်တာ) ဆိုတာကို အချိန်နဲ့တပြေးညီ သိနိုင်ဖို့ **Logs** တွေကို ကြည့်ရှုစစ်ဆေးရပါတယ်။

Linux မှာ log စနစ် (၂) မျိုး အဓိကရှိပါတယ်။

1.  **Syslog (System Log):** System တစ်ခုလုံးရဲ့ ဖြစ်ပျက်မှုတွေကို သိမ်းဆည်းပေးပါတယ်။ `rsyslogd` (Remote System Log Daemon) က ထိန်းချုပ်ပါတယ်။
2.  **Journald:** System စတင်ပွင့်လာချိန် (Boot time) နှင့် Kernel ပိုင်းဆိုင်ရာ ဖြစ်ပျက်မှုတွေကို သိမ်းဆည်းပါတယ်။ `systemd-journald` က ထိန်းချုပ်ပါတယ်။

---

## ၁။ Rsyslogd အကြောင်း သိကောင်းစရာ

`rsyslogd` ဆိုတာ system မှာ ဖြစ်ပျက်သမျှ log message တွေကို လက်ခံရယူပြီး သတ်မှတ်ထားတဲ့ file တွေထဲကို ခွဲခြားသိမ်းဆည်းပေးတဲ့ background process (Daemon) တစ်ခု ဖြစ်ပါတယ်။

### /etc/rsyslog.conf Rule များအကြောင်း

ဤ file သည် rsyslog ၏ အဓိက configuration file ဖြစ်သည်။ ၎င်းတွင် rule များကို **Selector** ဟုခေါ်သော format ဖြင့် ရေးသားသည်-
`facility.priority      target_file`

- **Facility:** မည်သည့်နေရာမှ log လာသနည်း (ဥပမာ - auth, mail, cron, kern)။
- **Priority (Severity):** မည်မျှ အရေးကြီးသနည်း (ဥပမာ - info, warning, err, crit)။
- **Target:** မည်သည့် file ထဲတွင် သိမ်းမည်နည်း (ဥပမာ - `/var/log/auth.log`)။

### Ubuntu ရှိ အရေးကြီးသော Log File များ

Ubuntu မှာ log တွေကို `/var/log` directory အောက်မှာ သိမ်းဆည်းပါတယ်။

| Log File            | ဖော်ပြချက် (Description)                                  |
| :------------------ | :-------------------------------------------------------- |
| `/var/log/syslog`   | **Overall messages** (System တစ်ခုလုံး၏ အထွေထွေ log များ) |
| `/var/log/auth.log` | Login ဝင်ခြင်းနှင့် security ဆိုင်ရာ log များ             |
| `/var/log/kern.log` | Kernel မှ ထုတ်ပေးသော message များ                         |
| `/var/log/cron.log` | Scheduled tasks (Cron job) ဆိုင်ရာ log များ               |
| `/var/log/apache2/` | Apache web server log များ (ရှိလျှင်)                     |

---

## ၂။ Logger Command ဖြင့် Custom Log ပို့ခြင်း

`logger` command ကို script များ သို့မဟုတ် terminal မှတစ်ဆင့် log system ထဲသို့ message လှမ်းပို့ရန် သုံးသည်။

- `logger "This is log message"`: Message တစ်ခုကို syslog ထဲသို့ ပို့သည်။ `journalctl -f` ဖြင့် real-time စောင့်ကြည့်နိုင်သည်။
- `logger -p [priority_name] "message"`: Log ကို အရေးကြီးအဆင့် သတ်မှတ်ပြီး ပို့ခြင်းဖြစ်သည်။
  - _ဥပမာ:_ `logger -p local0.err "Database connection failed"` (local0 facility ဖြင့် error log ပို့ခြင်း)

### Custom Log File ဖန်တီးနည်း (Step-by-Step)

အကယ်၍ သင်က logger နဲ့ ပို့တဲ့ message တွေကို သီးသန့် file တစ်ခုထဲမှာ သိမ်းချင်ရင်:

1.  `/etc/rsyslog.d/` အောက်သို့သွားပါ။
2.  `customlog.conf` ဆိုပြီး file အသစ်ဆောက်ပါ။
    ```bash
    sudo nano /etc/rsyslog.d/customlog.conf
    ```
3.  အောက်ပါ rule ကို ထည့်ရေးပါ-
    ```text
    local0.* /var/log/custom.log
    ```
4.  Service ကို restart ချပါ- `sudo systemctl restart rsyslog`
5.  စမ်းသပ်ရန်: `logger -p local0.info "Testing custom log"`
6.  စစ်ဆေးရန်: `cat /var/log/custom.log`

---

## ၃။ Journal Log စစ်ဆေးခြင်း (Journalctl)

`journalctl` သည် binary format ဖြင့် သိမ်းထားသော log များကို ဖတ်ရန် သုံးသည်။

- **အဘယ်ကြောင့်ကြည့်သနည်း:** Boot လုပ်စဉ် ဖြစ်ပျက်သမျှနှင့် systemd service များ၏ error များကို အသေးစိတ် သိနိုင်ရန် ဖြစ်သည်။
- **journalctl -k:** Kernel log များကိုသာ သီးသန့်ကြည့်ရန် (dmesg နှင့် ဆင်တူသည်)။
- **journalctl --disk-usage:** Journal log များက disk ပေါ်မှာ နေရာ (storage) ဘယ်လောက် ယူထားလဲ ကြည့်ရန်။
- **journalctl --since [Time]:** သတ်မှတ်ချိန်မှစ၍ ဖြစ်ပျက်သော log များကြည့်ရန်။
  - `--since "today"` (ယနေ့ log များ)
  - `--since "yesterday"` (မနေ့က log များ)
  - `--since "2024-05-01 10:00:00"` (သတ်မှတ်ရက်စွဲ/အချိန်)
- **--until:** သတ်မှတ်ချိန် အထိသာ ကြည့်ရန်။
  - _Example:_ `journalctl --since "2024-05-01" --until "2024-05-02"`

### အခြားအသုံးဝင်သော Command များ

- **journalctl -o verbose:** Log တစ်ခုချင်းစီ၏ metadata (ဥပမာ - User ID, Process ID) အားလုံးကို အသေးစိတ် ပြပေးသည်။
- **journalctl -p err:** Error အဆင့်ရှိသော log များကိုသာ စစ်ထုတ် (filter) ပြပေးသည်။ (Priority: `emerg`, `alert`, `crit`, `err`, `warning`, `notice`, `info`, `debug`)
- **journalctl -u [service_name]:** သတ်မှတ်ထားသော service (ဥပမာ- ssh, apache) တစ်ခုတည်း၏ log ကိုကြည့်ရန်။

---

## ၄။ Journal Log ကို Permanent (အမြဲတမ်း) သိမ်းဆည်းနည်း

မူလအားဖြင့် journal log များသည် `/run/log/journal` (Temporary memory) ထဲမှာ ရှိပြီး reboot လုပ်ရင် ပျက်သွားတတ်သည်။ အမြဲတမ်းသိမ်းလိုလျှင်:

1.  Configuration file ကို ဖွင့်ပါ- `sudo nano /etc/systemd/journald.conf`
2.  `[Journal]` အောက်တွင် `#Storage=auto` ကို `Storage=persistent` ဟု ပြောင်းပေးပါ။
3.  File ကို save ပြီး ထွက်ပါ။
4.  Service ကို restart ချပါ- `sudo systemctl restart systemd-journald`
5.  ယခုဆိုလျှင် log များကို `/var/log/journal/` အောက်တွင် အမြဲတမ်း သိမ်းဆည်းနေမည် ဖြစ်သည်။

---

## ၅။ အချိန်နှင့် ရက်စွဲ ထိန်းချုပ်ခြင်း (Timedatectl)

Log တွေ မှန်ကန်ဖို့ system အချိန် (Timezone) မှန်ဖို့ အလွန်အရေးကြီးသည်။

- **timedatectl set-ntp [boolean]:** Network Time Protocol (NTP) ကို သုံး/မသုံး သတ်မှတ်သည်။
  - _ဥပမာ:_ `timedatectl set-ntp true` (အင်တာနက်မှ အချိန်ကို အလိုအလျောက် ညှိမည်)
- **timedatectl list-timezones:** ကမ္ဘာပေါ်ရှိ အချိန်ဇုန် (Timezone) စာရင်းကို ပြပေးသည်။
- **timedatectl set-timezone [timezone]:** မိမိစက်၏ အချိန်ဇုန်ကို ပြောင်းသည်။
  - _ဥပမာ:_ `timedatectl set-timezone Asia/Yangon`

---

## ၆။ Chrony (Network Time Sync)

`chrony` သည် NTP server များမှတစ်ဆင့် စက်၏အချိန်ကို တိကျအောင် ညှိပေးသော service ဖြစ်သည်။

- **chronyc sources -v:** မိမိစက်က အချိန်ရယူနေသော source (Server) များ၏ အခြေအနေကို အသေးစိတ် ပြပေးသည်။

**Example Output:**

```text
MS Name/IP address         Stratum Poll Reach LastRx Last sample
===============================================================================
^- 103.11.141.22                 2   6   377    23   -255us[ -255us] +/-   63ms
^* time.google.com               1   6   377    25   +120us[ +145us] +/-   28ms
```

- `*` ပါသော server သည် လက်ရှိ အဓိက အချိန်ညှိနေသော (Current sync source) ဖြစ်သည်။
- `^` သည် server ဖြစ်ကြောင်း ပြသည်။
- `Last sample` သည် မိမိစက်နှင့် server ကြား အချိန်ကွာဟချက်ကို ပြသည်။
