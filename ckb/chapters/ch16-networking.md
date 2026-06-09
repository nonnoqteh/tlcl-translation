---
layout: default
---
# بابەتی ١٦ — تۆڕبەندی
### Chapter 16 – Networking

---

کاتێک قسە لە تۆڕبەندی دەکرێت، لینوکس بێهاوریوە. فەرمانەکانی ئەم بابەتە:

- `ping` — ناردنی بستەی تاقیکردنەوەی ICMP بۆ میزبان
- `traceroute` — نیشاندانی ڕێگای بستەکان بۆ میزبان
- `ip` — نیشاندان/دەستکاریکردنی ڕووتنگ و ئامێرەکان
- `netstat` — نیشاندانی پەیوەندییەکانی تۆڕ
- `ftp` — پرۆتۆکۆلی گواستنەوەی فایل
- `wget` — دانلۆدکردنی فایل
- `ssh` — شێلی ئەمن (OpenSSH)

---

## پشکنینی تۆڕ

### `ping` — تاقیکردنەوەی پەیوەندی

```bash
ping linuxcommand.org
```

```
PING linuxcommand.org (216.105.38.11) 56(84) bytes of data.
64 bytes from 216.105.38.11: icmp_seq=1 ttl=53 time=16.5 ms
64 bytes from 216.105.38.11: icmp_seq=2 ttl=53 time=16.3 ms
...
```

بە Ctrl-c دەوەستێت.

### `traceroute` — ردیابیکردنی ڕێگا

```bash
traceroute slashdot.org
```

```
 1  router.local (192.168.1.1)    2.452 ms
 2  10.0.0.1 (10.0.0.1)          7.234 ms
...
11  216.105.38.11                 16.503 ms
```

هەر هێڵ یەک **هاپ** — یەک رووتەر لە ڕێگادا — نیشاندەدات.

### `ip` — زانیاری تۆڕ

```bash
ip a
```

```
1: lo: <LOOPBACK,UP,LOWER_UP>
    inet 127.0.0.1/8 scope host lo
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>
    inet 192.168.1.102/24 brd 192.168.1.255 scope global eth0
```

### `netstat` — پەیوەندییە چالاکەکان

```bash
netstat -ie    # نیشاندانی ئینتەرفەیسەکانی تۆڕ
netstat -r     # نیشاندانی جەدوەلی ڕووتنگ
```

---

## گواستنەوەی فایل لە ڕێگای تۆڕ

### `ftp` — پرۆتۆکۆلی گواستنەوەی فایل

```bash
ftp fileserver
```

```
Connected to fileserver.
Name (fileserver:me): anonymous
Password:
ftp>
```

**فەرمانی باوی `ftp`:**

| فەرمان | کارکردن |
|--------|--------|
| `ls` | فێرستی فایلەکان |
| `cd dir` | گۆڕینی پووشپەر |
| `get file` | دانلۆدکردنی فایل |
| `put file` | ئەپلۆدکردنی فایل |
| `mget *.txt` | دانلۆدکردنی چەند فایل |
| `bye` | دەرچوون |

> **تێبینی:** ftp نائەمن — وشەی نهێنی بە دەقی ساده دەنێرێت. بۆ پەیوەندی ئەمن لە `sftp` بەکاربهێنە.

### `wget` — دانلۆدکردن لە خێزانی فەرمان

```bash
wget http://linuxcommand.org/index.php
```

**ئۆپشنەکانی بەسوود:**

```bash
wget -c url              # بەردەوامکردنی دانلۆدی نیمەتەواو
wget -r url              # دانلۆدی بازگشتی (تەواوی ماڵپەڕ)
wget -O output.html url  # پاراستن بە ناوی دلخوازی
wget -q url              # بەبێ ئاوتپوت (quiet)
```

---

## پەیوەندی لە دووری ئەمن بە `ssh`

`ssh` پەیوەندییەکی کۆدکراوی بۆ سیستەمی دوور دروستدەکات.

### پەیوەندی بۆ میزبانی دوور

```bash
ssh remote-sys
ssh user@remote-sys
ssh 192.168.1.10
```

یەکەم پەیوەندی:

```
The authenticity of host 'remote-sys' can't be established.
Are you sure you want to continue connecting (yes/no)? yes
```

### جێبەجێکردنی فەرمان لە دووری

```bash
ssh remote-sys free
ssh remote-sys 'ls *' | grep Desktop
```

### `scp` — کۆپیکردنی ئەمنی فایل

```bash
# کۆپی لە دوور بۆ ناوخۆ
scp user@remote-sys:document.txt .

# کۆپی لە ناوخۆ بۆ دوور
scp file.txt user@remote-sys:~/Desktop/

# کۆپیکردنی تەواوی پووشپەر
scp -r user@remote-sys:~/backup .
```

### `sftp` — FTP ئەمن

```bash
sftp remote-sys
```

مانەندی `ftp` کار دەکات بەڵام کۆدکراوە.

---

## ناسنامەکردن بە کلیلی SSH

بە جێی وشەی نهێنی، دەتوانین بە **جووتی کلیل** (key pair) بچینەژوورەوە:

```bash
# دروستکردنی جووتی کلیل
ssh-keygen
```

دوو فایل دروست دەبێت:
- `~/.ssh/id_rsa` — کلیلی تاکەکەس (شارابێت)
- `~/.ssh/id_rsa.pub` — کلیلی گشتی (لەسەر سێرڤەر نصب دەکرێت)

کۆپیکردنی کلیلی گشتی بۆ سێرڤەر:

```bash
ssh-copy-id user@remote-sys
```

لەمەوە دواتر بەبێ وشەی نهێنی چوونەژوورەوە دەکات.

---

## کورتە

ئامرازەکانی تۆڕی لینوکس زۆر هێزدار و لێبەردەون. لە `ping` ی سادەوە تا `ssh` کە ئەمنیەتی تەواوی پەیوەندی دووری دابین دەکات.

---

*بابەتی داهاتوو: [بابەتی ١٧ — گەڕان لە فایلەکان](ch17-searching-for-files)*
