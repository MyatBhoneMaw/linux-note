# SSH (Secure Shell) Configuration နှင့် လုံခြုံရေး လမ်းညွှန်

SSH ဆိုသည်မှာ Network တစ်ခုပေါ်ရှိ ကွန်ပျူတာတစ်လုံးမှ အခြားကွန်ပျူတာတစ်လုံးသို့ **Secure (လုံခြုံစိတ်ချစွာ)** ဝင်ရောက်အသုံးပြုနိုင်ရန် ဖန်တီးထားသော Protocol (ဆက်သွယ်ရေးနည်းလမ်း) တစ်ခု ဖြစ်ပါသည်။ ၎င်းသည် Linux စနစ်များတွင် Port 22 ကို အသုံးပြု၍ အလုပ်လုပ်ပါသည်။

## 1. SSH ၏ အခြေခံ အစိတ်အပိုင်းများ (SSH Software Packages)

SSH တွင် အဓိကအားဖြင့် Package (ဆော့ဖ်ဝဲလ်အစုအဝေး) ၂ ခု ရှိပါသည်။

- **openssh-client:** အခြားစက် (Server) ထဲသို့ လှမ်းဝင်မည့်စက် (Client) တွင် ရှိရပါမည်။
- **openssh-server:** အဝေးမှ လှမ်းဝင်အသုံးပြုခြင်းကို လက်ခံမည့်စက် (Server) တွင် ရှိရပါမည်။

### Installation (ဆော့ဖ်ဝဲလ် ထည့်သွင်းခြင်း)

Server စက်တွင် SSH Server မရှိသေးပါက အောက်ပါ Command ဖြင့် ထည့်သွင်းနိုင်သည် -

```bash
sudo apt update
sudo apt install openssh-server
```

---

## 2. SSH အလုပ်လုပ်ပုံ အဆင့်ဆင့် (How SSH Works)

Client နှင့် Server ချိတ်ဆက်ရာတွင် အောက်ပါအဆင့်များအတိုင်း လုပ်ဆောင်သည် -

1.  **Connection Setup:** Client က Server ၏ Port 22 သို့ ချိတ်ဆက်ရန် တောင်းဆိုသည်။
2.  **Encryption Negotiation:** ဒေတာများကို လျှို့ဝှက်ကုဒ်ပြောင်းရန် (Encryption) မည်သည့်နည်းလမ်းသုံးမည်ကို နှစ်ဖက်သဘောတူညီမှု ရယူသည်။
3.  **Authentication:** အသုံးပြုသူသည် အစစ်အမှန်ဖြစ်ကြောင်း သက်သေပြသည်။ (Password သို့မဟုတ် Key အသုံးပြုခြင်း)
4.  **Secure Channel:** အောင်မြင်ပါက လုံခြုံသော လမ်းကြောင်းတစ်ခု ပွင့်သွားပြီး Command များကို စိတ်ချစွာ ရိုက်နှိပ်အသုံးပြုနိုင်မည်ဖြစ်သည်။

---

## 3. Authentication နည်းလမ်းများ (စစ်ဆေးအတည်ပြုခြင်း)

### A. Password-based Authentication (စကားဝှက်ဖြင့် ဝင်ရောက်ခြင်း)

၎င်းသည် ပုံမှန်အသုံးပြုနေကျနည်းလမ်းဖြစ်ပြီး User ၏ Password ကို ရိုက်ထည့်၍ ဝင်ရောက်ခြင်းဖြစ်သည်။

- **အားနည်းချက်:** Password ကို ခန့်မှန်း၍ တိုက်ခိုက်ခြင်း (Brute-force attack) ခံရနိုင်ခြေ များသည်။

### B. Key-based Authentication (သော့အသုံးပြု၍ ဝင်ရောက်ခြင်း) - **အလွန်အရေးကြီး**

၎င်းသည် Password အစား **Cryptographic Keys (လျှို့ဝှက်ကုဒ်သော့များ)** ကို အသုံးပြုသည်။ ၎င်းတွင် အပိုင်း ၂ ပိုင်းရှိသည် -

1.  **Private Key:** မိမိ၏ Client စက်တွင်သာ သိမ်းဆည်းထားရမည့် လျှို့ဝှက်သော့။
2.  **Public Key:** မည်သည့် Server ကိုမဆို ပေးပို့ထားနိုင်သည့် သော့။

**အလုပ်လုပ်ပုံ:** Server ဆီသို့ Public Key ပို့ထားပြီးနောက်၊ ချိတ်ဆက်သည့်အခါ Client ရှိ Private Key နှင့် တိုက်ဆိုင်စစ်ဆေးသည်။ ကိုက်ညီမှသာ ပေးဝင်သည်။ ၎င်းသည် Password ထက် အဆပေါင်းများစွာ ပိုမိုလုံခြုံသည်။

---

## 4. SSH Key ပြုလုပ်ခြင်းနှင့် ပေးပို့ခြင်း

### Step 1: Key ထုတ်ယူခြင်း (Key Generation)

```bash
ssh-keygen
```

_(ဤ Command သည် `~/.ssh/` ထဲတွင် `id_rsa` (Private Key) နှင့် `id_rsa.pub` (Public Key) ကို ထုတ်ပေးလိမ့်မည်။)_

### Step 2: Server ထံသို့ Key ပေးပို့ခြင်း

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub student@192.168.1.10
```

**ရှင်းလင်းချက်:**

- `ssh-copy-id`: မိမိ၏ Public Key ကို Server ထဲရှိ `authorized_keys` ဖိုင်ထဲသို့ သွားရောက်ထည့်သွင်းပေးသည့် Command ဖြစ်သည်။
- `-i` (Identity File): မည်သည့် Key ဖိုင်ကို ပို့မည်နည်းဟု သတ်မှတ်ပေးသည့် Option ဖြစ်သည်။ (ဥပမာ - `~/.ssh/id_rsa.pub`)

---

## 5. SSH Configuration နှင့် လုံခြုံရေး မြှင့်တင်ခြင်း

SSH နှင့်ပတ်သက်သော အဓိက Configuration (စနစ်ပုံစံချခြင်း) ဖိုင်များသည် `/etc/ssh/` လမ်းကြောင်းထဲတွင် ရှိသည်။

- **sshd_config:** ၎င်းသည် Server ၏ Setting ဖိုင်ဖြစ်ပြီး လုံခြုံရေးအတွက် ဤဖိုင်ကို ပြင်ဆင်ရသည်။

### ပိုမိုလုံခြုံအောင် ပြုလုပ်နည်း (Best Practices)

လုံခြုံရေးအတွက် `/etc/ssh/sshd_config` ဖိုင်ကို `sudo nano` ဖြင့်ဖွင့်၍ အောက်ပါတို့ကို ပြင်ဆင်သင့်သည် -

1.  **Port ပြောင်းလဲခြင်း:** ပုံမှန် Port 22 အစား အခြားနံပါတ် (ဥပမာ - 2222) သို့ ပြောင်းပါ။
    `Port 2222`
2.  **Root Login ပိတ်ခြင်း:** Root User တိုက်ရိုက်ဝင်ခြင်းကို တားဆီးပါ။
    `PermitRootLogin no`
3.  **Password ဖြင့်ဝင်ခြင်းကို ပိတ်ခြင်း:** Key ရှိသူသာ ဝင်ခွင့်ပေးပါ။
    `PasswordAuthentication no`

ပြင်ဆင်ပြီးတိုင်း SSH Service ကို Restart ချပေးရမည် -

```bash
sudo systemctl restart ssh
```

---

## 6. Example Outputs (နမူနာထွက်ရှိချက်များ)

### SSH ချိတ်ဆက်သည့်အခါ (Login Example):

```text
student@client:~$ ssh student@192.168.1.10
student@192.168.1.10's password:
Welcome to Ubuntu 22.04 LTS (GNU/Linux 5.15.0-generic x86_64)
student@server:~$
```

### SSH Key စစ်ဆေးခြင်း:

Client စက်ရှိ Key များ တည်ရှိရာနေရာ -

```bash
ls ~/.ssh/
# output: id_rsa  id_rsa.pub  known_hosts
```

Server စက်ပေါ်တွင် Client ထံမှ လက်ခံရရှိထားသော Key များ ရှိမည့်နေရာ -

```bash
cat ~/.ssh/authorized_keys
# output: ssh-rsa AAAAB3Nza... student@client
```

---

> **သတိပြုရန်:** `/etc/ssh/sshd_config` ကို ပြင်ဆင်သည့်အခါ အမှားအယွင်းရှိပါက SSH ဝင်မရတော့ဘဲ ဖြစ်တတ်သောကြောင့် မပြင်မီ မူရင်းဖိုင်ကို Backup (မိတ္တူ) ကူးထားသင့်ပါသည်။
