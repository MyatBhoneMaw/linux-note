## 1. Linux Process ဆိုတာဘာလဲ?

Linux မှာ program တစ်ခုခုကို run လိုက်တဲ့အခါ memory ပေါ်မှာ အလုပ်လုပ်နေတဲ့ အစိတ်အပိုင်းကို **Process** လို့ ခေါ်ပါတယ်။

- **PID (Process ID):** Process တိုင်းမှာ သီးသန့်ခွဲခြားလို့ရတဲ့ ID နံပါတ်တစ်ခုစီ ရှိပါတယ်။
- **PPID (Parent Process ID):** Process တစ်ခုကို တခြား Process တစ်ခုကနေ မွေးထုတ်ပေးလိုက်ရင် မူလ Process ကို Parent လို့ခေါ်ပြီး သူ့ရဲ့ ID ကို PPID လို့ ခေါ်ပါတယ်။
- **Child Process:** Parent ကနေ ခွဲထွက်လာတဲ့ process အသစ် ဖြစ်ပါတယ်။
- **Zombie Process:** Child process တစ်ခုက အလုပ်ပြီးသွားပေမယ့် Parent process က သူ့ရဲ့ status ကို ပြန်မသိမ်းသေးတဲ့အခါ (သေသွားပေမယ့် စာရင်းမပယ်ဖျက်ရသေးတဲ့) အခြေအနေကို ခေါ်ပါတယ်။

### Process Life Cycle (အလုပ်လုပ်ပုံ အဆင့်ဆင့်)

1.  **Fork:** Parent process ကနေ child process တစ်ခု အသစ်ပွားယူလိုက်ခြင်း။
2.  **Exec:** ပွားယူထားတဲ့ process ထဲမှာ program အသစ်ကို run ခြင်း။
3.  **Wait:** Parent က child ပြီးအောင် စောင့်နေခြင်း။
4.  **Exit/Terminate:** အလုပ်ပြီးလို့ ပိတ်သွားခြင်း။

---

## 2. Process States (Process များ၏ အခြေအနေများ)

Linux process တစ်ခုဟာ အောက်ပါ state (အခြေအနေ) တွေထဲက တစ်ခုခုမှာ ရှိနေတတ်ပါတယ်။

1.  **Runnable / Ready State (R):** CPU ပေါ်တက်ပြီး run ဖို့ အသင့်ဖြစ်နေတဲ့ အခြေအနေ။ (CPU မအားသေးလို့ စောင့်နေရတာမျိုး)
2.  **Running State (R):** CPU ပေါ်မှာ တကယ်အလုပ်လုပ်နေတဲ့ အခြေအနေ။ ၎င်းကို **Kernel Mode** (System အပိုင်းလုပ်ဆောင်ချက်) နှင့် **User Mode** (User app အပိုင်းလုပ်ဆောင်ချက်) ဆိုပြီး ခွဲခြားနိုင်ပါတယ်။
3.  **Sleeping State:** \* **Interruptible Sleep (S):** တစ်ခုခုကို စောင့်နေတဲ့ အခြေအနေ (ဥပမာ - keyboard နှိပ်မှာကို စောင့်နေခြင်း)။ ကြားဖြတ်နှောင့်ယှက် (kill) လို့ ရပါတယ်။
    - **Uninterruptible Sleep (D - Deep Sleep):** Disk (HDD/SSD) ဆီက data ပြန်လာတာကို စောင့်နေတာမျိုးပါ။ ကြားဖြတ်နှောင့်ယှက်လို့ မရပါဘူး။
4.  **Stopped State (T):** Process ကို ခေတ္တရပ်ဆိုင်း (Suspend) ထားတဲ့ အခြေအနေ။ (Kill လိုက်တာ မဟုတ်ပါ)
5.  **Zombie State (Z):** Process က အလုပ်ပြီးသွားပြီ (Terminate/Exit ဖြစ်သွားပြီ) ဒါပေမယ့် process table ထဲမှာ စာရင်းရှိနေသေးတဲ့ အခြေအနေ။
6.  **Dead/Exited (X):** Process လုံးဝ ပျက်စီးသွားပြီး memory ပေါ်က ဆင်းသွားတဲ့ အခြေအနေ။

---

## 3. Process ကြည့်ရှုခြင်း (Viewing Processes)

### `ps` Command (Process Status)

လက်ရှိ run နေတဲ့ process တွေကို ဓာတ်ပုံရိုက်သလို (Snapshot) တစ်ချက်ကြည့်တာပါ။

```bash
$ ps
  PID TTY          TIME CMD
 4285 pts/0    00:00:00 bash
 6337 pts/0    00:00:00 ps
```

- **PID:** Process Identification number.
- **TTY:** Terminal type (pts က GUI terminal ကို ဆိုလိုပြီး၊ tty က system console/physical terminal ကို ဆိုလိုပါတယ်)။
- **TIME:** CPU အသုံးပြုခဲ့တဲ့ စုစုပေါင်းကြာချိန်။
- **CMD:** Run ထားတဲ့ command အမည်။

### Options ၃ မျိုး

1.  **UNIX Option:** `-e`, `-f` (Dash တစ်ခုပါသည်)
2.  **BSD Option:** `aux`, `ax` (Dash မပါပါ)
3.  **GNU Option:** `--help` (Dash နှစ်ခုပါသည်)

**အသုံးများသော Command များ:**

- `ps aux`: System တစ်ခုလုံးရှိ process အားလုံးကို user အသေးစိတ်နဲ့ ပြပေးပါတယ်။
- `ps lax`: Process တွေကို long format (အသေးစိတ်) ပြပေးပြီး ပိုမြန်ပါတယ်။ (User name ရှာစရာမလိုလို့)

---

## 4. Jobs: Foreground vs Background

Linux မှာ process တွေကို နေရာ ၂ နေရာမှာ run နိုင်ပါတယ်။

- **Foreground:** Screen ပေါ်မှာ မြင်နေရပြီး terminal ကို သုံးလို့မရဘဲ စောင့်နေရတဲ့ အခြေအနေ။
- **Background:** နောက်ကွယ်မှာ အလုပ်လုပ်နေပြီး terminal ကို ဆက်သုံးလို့ရတဲ့ အခြေအနေ။ (Command နောက်မှာ `&` ထည့်ပေးရပါတယ်)

**နမူနာ:**

```bash
$ gedit &       # gedit ကို background မှာ တန်း run ခြင်း
[1] 7596        # [1] က Job ID ဖြစ်ပြီး 7596 က PID ဖြစ်ပါတယ်
```

- **Ctrl + Z:** Foreground ကောင်ကို **Stop (Suspend)** လုပ်ပြီး ရပ်လိုက်တာပါ။
- **Ctrl + C:** Process ကို အပြီးသတ် **Kill/Interrupt** လုပ်လိုက်တာပါ။
- **`jobs`:** လက်ရှိ terminal မှာ run/stop ဖြစ်နေတဲ့ job တွေကို ကြည့်တာပါ။
- **`bg %1`:** Job ID 1 ကို background မှာ ဆက် run ခိုင်းတာပါ။
- **`fg %1`:** Job ID 1 ကို foreground (ရှေ့မျက်နှာပြင်) ပြန်ခေါ်တာပါ။

---

## 5. Controlling Processes (Killing Processes)

Process တွေကို ထိန်းချုပ်ဖို့ **Signals** တွေကို သုံးပါတယ်။

| Signal Number | Signal Name | ရှင်းလင်းချက်                                            |
| :------------ | :---------- | :------------------------------------------------------- |
| 1             | SIGHUP      | Reload/Restart လုပ်ခြင်း                                 |
| 2             | SIGINT      | Keyboard ကနေ ပိတ်လိုက်ခြင်း (Ctrl+C)                     |
| 9             | SIGKILL     | အတင်းအဓမ္မ သတ်ပစ်ခြင်း (Force Kill)                      |
| 15            | SIGTERM     | ယဉ်ကျေးစွာ ပိတ်ခိုင်းခြင်း (Default)                     |
| 18            | SIGCONT     | ရပ်ထားတဲ့ process ကို ပြန်လည်အလုပ်လုပ်စေခြင်း (Continue) |
| 19            | SIGSTOP     | Process ကို ခေတ္တရပ်ထားခြင်း (Suspend)                   |

### Kill Commands

- **`kill -9 6131`:** PID 6131 ကို အတင်းအဓမ္မ သတ်ပစ်ခြင်း။
- **`killall -19 -u bue gedit firefox`:**
  > ရှင်းလင်းချက်: User **'bue'** run ထားတဲ့ **gedit** နဲ့ **firefox** process အားလုံးကို signal **19 (STOP)** ပို့လိုက်တာပါ။ ဆိုလိုတာက ထို program တွေကို ခေတ္တ ရပ်ဆိုင်း (suspend) လိုက်တာ ဖြစ်ပါတယ်။

### `pkill` နှင့် `pgrep`

- **`pgrep [process_name]`:** Process ရဲ့ နာမည်ကို ရိုက်ပြီး PID ကို ရှာတာပါ။ ဥပမာ: `pgrep firefox`
- **`pkill [process_name]`:** PID ရှာစရာမလိုဘဲ နာမည်နဲ့ တန်းသတ်တာပါ။ ဥပမာ: `pkill -9 chrome`

---

## 6. အခြားအသုံးဝင်သော Commands များ

- **`pstree`:** Process တွေကို မိသားစုဝင်တွေလို သစ်ပင်ပုံစံ (Hierarchy) ပြပေးတာပါ။ Parent နဲ့ Child ဘယ်သူလဲဆိုတာ သိနိုင်ပါတယ်။
- **`uptime`:** System စတက်ကတည်းက ဘယ်လောက်ကြာပြီလဲ၊ user ဘယ်နှစ်ယောက်ရှိလဲနဲ့ load average (CPU ဝန်ထုပ်ဝန်ပိုး) ကို ပြပါတယ်။
  ```text
  02:30:05 up 3 days, 10:20,  1 user,  load average: 0.15, 0.08, 0.02
  ```
- **`cat /proc/cpuinfo`:** မိမိ computer ရဲ့ CPU core တွေ၊ model တွေအကြောင်း အသေးစိတ် ကြည့်တာပါ။

---

## 7. `top` Command (Table of Processes)

`top` သည် real-time process တွေကို ပြပေးတဲ့ Dashboard တစ်ခုလိုပါပဲ။

- **Z, B:** အရောင်ပြောင်းခြင်း၊ စာလုံးအထူလုပ်ခြင်း။
- **u:** သီးသန့် User တစ်ယောက်တည်းရဲ့ process ကို စစ်ကြည့်ခြင်း။
- **k:** `top` ထဲကနေတင် process ကို PID ပေးပြီး kill ခြင်း။
- **q:** `top` ကနေ ပြန်ထွက်ခြင်း။
- **M:** Memory အများဆုံးသုံးတဲ့ကောင်ကို ထိပ်ဆုံးတင်ခြင်း။
- **P:** CPU အများဆုံးသုံးတဲ့ကောင်ကို ထိပ်ဆုံးတင်ခြင်း။

### `ps` နှင့် `top` ကွာခြားချက်

| Feature      | `ps`                                 | `top`                                   |
| :----------- | :----------------------------------- | :-------------------------------------- |
| **Type**     | Static (Snapshot)                    | Dynamic (Real-time)                     |
| **Updating** | တစ်ခါရိုက်ရင် တစ်ခါပဲပြတယ်           | အချိန်နဲ့အမျှ update ဖြစ်နေတယ်          |
| **Usage**    | Script တွေထဲမှာ သုံးရတာ ပိုကောင်းတယ် | အခြေအနေကို စောင့်ကြည့်ဖို့ ပိုကောင်းတယ် |
