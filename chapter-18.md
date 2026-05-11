# 🐧 Linux Process Priority စီမံခန့်ခွဲခြင်း (Managing Linux Process Priority)

Linux system တစ်ခုမှာ software တွေ သို့မဟုတ် process တွေ အများကြီး တစ်ပြိုင်နက် run နေတဲ့အခါ CPU ကို ဘယ် process က ပိုသုံးသင့်သလဲ၊ ဘယ်သူ့ကို ဦးစားပေးရမလဲဆိုတာကို သတ်မှတ်ပေးဖို့ **Process Priority** ကို အသုံးပြုပါတယ်။

---

## 1. Nice Value ဆိုတာဘာလဲ?

Nice value ဆိုတာ process တစ်ခုရဲ့ "ယဉ်ကျေးမှု" အတိုင်းအတာလို့ မှတ်ယူနိုင်ပါတယ်။

- သူ့ရဲ့ range က **-20 ကနေ 19** အထိ ရှိပါတယ်။
- **Default nice value** ကတော့ `0` ဖြစ်ပါတယ်။

### Nice Value မြင့်ရင် (Higher Nice Value: 1 to 19)

- **Priority:** နိမ့်သွားပါတယ်။ (Lower Priority)
- **CPU Usage:** CPU ကို အားနာနာနဲ့ လျှော့သုံးပါတယ်။ အခြား process တွေကို ဦးစားပေးပါတယ်။
- **ခွင့်ပြုချက်:** User တိုင်း (Normal Users) ပြောင်းလဲသတ်မှတ်နိုင်ပါတယ်။
- **အဓိပ္ပာယ်:** "ငါက ယဉ်ကျေးတယ်၊ တခြားသူတွေ အရင်သုံးပါ" လို့ ပြောတာနဲ့ တူပါတယ်။

### Nice Value နိမ့်ရင် (Lower/Negative Nice Value: -1 to -20)

- **Priority:** မြင့်လာပါတယ်။ (Higher Priority)
- **CPU Usage:** CPU ကို ပိုပြီး အသားကုန် သုံးပါတယ်။
- **ခွင့်ပြုချက်:** **Root user (Administrator)** သာလျှင် လုပ်ဆောင်နိုင်ပါတယ်။ သာမန် user က priority တိုးလို့မရပါဘူး။
- **အဓိပ္ပာယ်:** "ငါ့အလုပ်က အရေးကြီးတယ်၊ ငါ့ကို CPU အရင်ပေးသုံးပါ" လို့ တောင်းဆိုတာပါ။

---

## 2. အသုံးပြုပုံ Command များ နှင့် ရှင်းလင်းချက်

### `nice` command (Process အသစ်တစ်ခု စတင်ခြင်း)

Process တစ်ခုကို စတင် run ကတည်းက priority သတ်မှတ်ပေးချင်ရင် `nice` ကို သုံးပါတယ်။

- **ဥပမာ:** `nice -n 5 firefox`
- **ရှင်းလင်းချက်:** Firefox ကို nice value `5` (Priority အနည်းငယ်လျှော့ပြီး) နဲ့ စတင်ဖွင့်လိုက်တာပါ။

### `renice` command (လက်ရှိ Run နေသော Process ကို ပြောင်းလဲခြင်း)

Run နေပြီးသား process တစ်ခုရဲ့ priority ကို ပြန်ပြင်ချင်ရင် `renice` ကို သုံးပါတယ်။

- **ဥပမာ:** `renice -n 10 -p 4289`
- **ရှင်းလင်းချက်:** PID (Process ID) `4289` ရှိတဲ့ process ရဲ့ nice value ကို `10` သို့ ပြောင်းလိုက်တာပါ။

---

## 3. Command များအား အသေးစိတ်လေ့လာခြင်း

### A. `top -d 1`

- **ရှင်းလင်းချက်:** System ထဲမှာ run နေတဲ့ process တွေကို real-time ကြည့်တာပါ။ `-d 1` ဆိုတာကတော့ **delay 1 second** လို့ အဓိပ္ပာယ်ရပါတယ်။ ၁ စက္ကန့်ခြား တစ်ခါ screen ကို refresh လုပ်ပြီး process စာရင်းကို ပြပေးနေမှာပါ။
- **Example Output:**

  ```text
  top - 12:00:01 up 1 day,  2:30,  1 user,  load average: 0.08, 0.03, 0.01
  Tasks: 250 total,   1 running, 249 sleeping,   0 stopped,   0 zombie
  %Cpu(s):  1.5 us,  0.5 sy,  0.0 ni, 98.0 id,  0.0 wa,  0.0 hi,  0.0 si
  MiB Mem :   7950.0 total,   4120.5 free,   2100.2 used,   1729.3 buff/cache

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
   4289 user      20   0  350.5m  95.2m  40.1m S   2.0   1.2   0:45.12 firefox
  ```

### B. `ps axo pid,comm | grep firefox`

- **ရှင်းလင်းချက်:** လက်ရှိ run နေတဲ့ process တွေထဲကမှ process ID (`pid`) နဲ့ command name (`comm`) ကိုပဲ ထုတ်ပြခိုင်းတာပါ။ `grep` သုံးထားတဲ့အတွက် `firefox` နဲ့ ဆိုင်တာတွေကိုပဲ ရှာပေးမှာပါ။
- **Example Output:**
  ```text
  4289 firefox
  ```

### C. `ps axo pid,comm,nice | grep firefox`

- **ရှင်းလင်းချက်:** အပေါ်ကအတိုင်းပဲ ဒါပေမယ့် ဒီမှာ `nice` value ပါ ထပ်တိုးပြီး ကြည့်တာပါ။ Firefox ရဲ့ လက်ရှိ priority ဘယ်လောက်ရှိလဲ သိချင်ရင် သုံးပါတယ်။
- **Example Output:**
  ```text
  4289 firefox           0
  ```

### D. `renice -n 19 4289`

- **ရှင်းလင်းချက်:** PID `4289` (Firefox) ရဲ့ nice value ကို `19` အထိ တင်လိုက်တာပါ။ ဒါဆိုရင် Firefox က priority အနိမ့်ဆုံးဖြစ်သွားပြီး CPU ကို အနည်းဆုံးပဲ ယူသုံးပါတော့မယ်။
- **Example Output:**
  ```text
  4289 (process ID) old priority 0, new priority 19
  ```

### E. `nice -n 0 firefox`

- **ရှင်းလင်းချက်:** Firefox ကို nice value `0` (Default priority) နဲ့ အစကတည်းက run ခိုင်းတာပါ။ (တကယ်တော့ standard run တာနဲ့ အတူတူပါပဲ)။
- **မှတ်ချက်:** အကယ်၍ nice value ကို အနှုတ် (ဥပမာ: `-5`) ထားချင်ရင် `sudo nice -n -5 firefox` ဆိုပြီး administrator power နဲ့ run ရပါမယ်။

---

## အနှစ်ချုပ် (Summary Table)

| Nice Value    | Priority Level | ဘယ်သူလုပ်နိုင်လဲ | CPU ရရှိမှု                |
| :------------ | :------------- | :--------------- | :------------------------- |
| **-20 to -1** | High Priority  | Root User သာ     | အများကြီးယူသုံးမည်         |
| **0**         | Default        | All Users        | ပုံမှန်                    |
| **1 to 19**   | Low Priority   | All Users        | အားနာနာဖြင့် လျှော့သုံးမည် |

---

_Created for Linux Beginners in Myanmar Language_
"""

with open("README_Linux_Priority_MM.md", "w", encoding="utf-8") as f:
f.write(markdown_content)

```

သင်တောင်းဆိုထားတဲ့အတိုင်း Linux Process Priority စီမံခန့်ခွဲခြင်းအကြောင်းကို မြန်မာဘာသာနဲ့ အသေးစိတ်ရှင်းပြထားတဲ့ README (Markdown) ဖိုင်ကို ဖန်တီးပေးလိုက်ပါတယ်။ နည်းပညာဆိုင်ရာ အသုံးအနှုန်းတွေကိုလည်း နားလည်လွယ်အောင် ပြင်ဆင်ပြီး ကွင်းစကွင်းပိတ်နဲ့ ဖော်ပြပေးထားပါတယ်။

သင်တစ်ခါတည်း Copy ကူးယူအသုံးပြုနိုင်ရန် အောက်ပါအတိုင်း ပြင်ဆင်ပေးထားပါတယ်-

Your PDF file is ready


### အဓိက မှတ်သားရန်အချက်များ-

* **Nice Value များလေ Priority နိမ့်လေ:** (Nice - ယဉ်ကျေးသူဖြစ်လို့ တခြား process တွေကို ဦးစားပေးတာပါ)။
* **Nice Value နည်းလေ Priority မြင့်လေ:** (CPU ကို ပိုယူသုံးတာပါ)။
* **Root User သာလျှင်:** Priority ကို အနှုတ်တန်ဖိုး (Negative values) ပြောင်းလဲနိုင်ပြီး CPU ကို ပိုစားစေနိုင်ပါတယ်။
* **nice vs renice:** အသစ်စမယ့် process အတွက် `nice` ကိုသုံးပြီး၊ run နေပြီးသား process အတွက် `renice` ကို သုံးပါတယ်။
```
