---
layout: default
---
# فصل ۱۶ — شبکه
### Chapter 16 – Networking

---

وقتی صحبت از شبکه می‌شود، لینوکس بی‌رقیب است. این فصل ابزارهای شبکه‌ی خط فرمان را معرفی می‌کند:

- `ping` — ارسال بسته‌ی آزمایشی ICMP به میزبان
- `traceroute` — نمایش مسیر بسته‌ها به میزبان
- `ip` — نمایش/دستکاری مسیریابی، دستگاه‌ها و تانل‌ها
- `netstat` — نمایش اتصالات شبکه
- `ftp` — پروتکل انتقال فایل
- `wget` — دانلود غیرتعاملی فایل
- `ssh` — پوسته‌ی امن (OpenSSH)

---

## بررسی شبکه
### Examining and Monitoring a Network

### `ping` — آزمایش اتصال

`ping` بسته‌های ICMP ECHO_REQUEST به یک میزبان می‌فرستد:

```bash
ping linuxcommand.org
```

```
PING linuxcommand.org (216.105.38.11) 56(84) bytes of data.
64 bytes from 216.105.38.11: icmp_seq=1 ttl=53 time=16.5 ms
64 bytes from 216.105.38.11: icmp_seq=2 ttl=53 time=16.3 ms
...
```

با Ctrl-c متوقف می‌شود. آمار پایانی نشان می‌دهد چند بسته فرستاده و چند بسته دریافت شد.

### `traceroute` — ردیابی مسیر

```bash
traceroute slashdot.org
```

```
traceroute to slashdot.org (216.105.38.11)
 1  router.local (192.168.1.1)    2.452 ms
 2  10.0.0.1 (10.0.0.1)          7.234 ms
 3  ...
 ...
11  216.105.38.11                 16.503 ms
```

هر خط یک **هاپ** (hop) — یک روتر در مسیر — نشان می‌دهد. `*` یعنی پاسخی نرسید.

### `ip` — اطلاعات شبکه

```bash
ip a
```

```
1: lo: <LOOPBACK,UP,LOWER_UP> ...
    inet 127.0.0.1/8 scope host lo
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> ...
    inet 192.168.1.102/24 brd 192.168.1.255 scope global eth0
```

### `netstat` — اتصالات فعال

```bash
netstat -ie    # نمایش رابط‌های شبکه
netstat -r     # نمایش جدول مسیریابی
```

---

## انتقال فایل از طریق شبکه

### `ftp` — پروتکل انتقال فایل

```bash
ftp fileserver
```

```
Connected to fileserver.
Name (fileserver:me): anonymous
Password:
ftp>
```

**دستورات رایج `ftp`:**

| دستور | عملکرد |
|-------|--------|
| `ls` | فهرست فایل‌ها |
| `cd dir` | تغییر پوشه |
| `get file` | دانلود فایل |
| `put file` | آپلود فایل |
| `mget *.txt` | دانلود چند فایل |
| `bye` | خروج |

> **نکته:** ftp ناامن است — رمز عبور به‌صورت متن ساده ارسال می‌شود. برای اتصال امن از `sftp` استفاده کنید.

### `wget` — دانلود از خط فرمان

`wget` برای دانلود محتوای وب:

```bash
wget http://linuxcommand.org/index.php
```

```
--2025-06-09 11:02:09--  http://linuxcommand.org/index.php
Resolving linuxcommand.org... 216.105.38.11
Connecting... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3071 (3.0K) [text/html]
Saving to: 'index.php'

index.php  100%[==========>]   3.00K  --.-KB/s    in 0s
```

**گزینه‌های مفید `wget`:**

```bash
wget -c url             # ادامه‌ی دانلود نیمه‌تمام
wget -r url             # دانلود بازگشتی (کل سایت)
wget -O output.html url # ذخیره با نام دلخواه
wget -q url             # بدون خروجی (quiet)
```

---

## ارتباط از راه دور امن با `ssh`
### Secure Communication with Remote Hosts

`ssh` (Secure Shell) یک اتصال رمزگذاری‌شده به سیستم راه دور برقرار می‌کند.

### اتصال به میزبان راه دور

```bash
ssh remote-sys
```

یا با نام کاربری:

```bash
ssh user@remote-sys
```

یا با آدرس IP:

```bash
ssh 192.168.1.10
```

اولین اتصال:

```
The authenticity of host 'remote-sys (192.168.1.10)' can't be established.
ECDSA key fingerprint is SHA256:...
Are you sure you want to continue connecting (yes/no)? yes
```

بعد از `yes`، کلید به `~/.ssh/known_hosts` اضافه می‌شود.

### اجرای دستور از راه دور

```bash
ssh remote-sys free
```

```
             total       used       free
Mem:        319496     314860       4636
```

با لوله‌کشی:

```bash
ssh remote-sys 'ls *' | grep Desktop
```

### `scp` — کپی امن فایل

```bash
# کپی از راه دور به محلی
scp user@remote-sys:document.txt .

# کپی از محلی به راه دور
scp file.txt user@remote-sys:~/Desktop/

# کپی کل پوشه
scp -r user@remote-sys:~/backup .
```

### `sftp` — FTP امن

```bash
sftp remote-sys
```

```
sftp>
```

مثل `ftp` کار می‌کند اما رمزگذاری‌شده است.

---

## احراز هویت با کلید SSH
### SSH Key-Based Authentication

به جای رمز عبور، می‌توانیم با **جفت کلید** (key pair) ورود کنیم:

```bash
# ساختن جفت کلید
ssh-keygen
```

```
Generating public/private rsa key pair.
Enter file in which to save the key (/home/me/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
```

دو فایل ساخته می‌شود:
- `~/.ssh/id_rsa` — کلید خصوصی (مخفی نگه داشته می‌شود)
- `~/.ssh/id_rsa.pub` — کلید عمومی (روی سرور نصب می‌شود)

کپی کلید عمومی به سرور:

```bash
ssh-copy-id user@remote-sys
```

از این پس بدون رمز عبور وارد می‌شویم.

---

## خلاصه

ابزارهای شبکه‌ی لینوکس بسیار قدرتمند و انعطاف‌پذیرند. از `ping` ساده تا `ssh` که امنیت کامل ارتباط راه دور را فراهم می‌کند. تسلط بر این ابزارها برای هر کاربر جدی لینوکس ضروری است.

---

## مطالعه‌ی بیشتر

- مستندات OpenSSH: [openssh.com](https://www.openssh.com)
- درباره‌ی پروتکل SSH: [Wikipedia: SSH](http://en.wikipedia.org/wiki/Secure_Shell)

---

*فصل بعدی: [فصل ۱۷ — جستجو در فایل‌ها](ch17-searching-for-files)*
