---
layout: default
---
# فصل ۲۴ — نوشتن اولین اسکریپت
### Chapter 24 – Writing Your First Script

---

دستورهای این فصل:

- `bash` — اجرای اسکریپت شل
- `chmod` — تغییر مجوز فایل

---

## اسکریپت شل چیست؟

**اسکریپت شل** (shell script) فایلی متنی است که حاوی دنباله‌ای از دستورهای شل است. به جای اینکه هر بار دستورها را یک‌به‌یک تایپ کنیم، آن‌ها را در فایل ذخیره می‌کنیم و یک‌باره اجرا می‌کنیم.

---

## چطور اسکریپت بنویسیم؟

### ۱. ایجاد فایل

```bash
nano hello_world
```

محتوای فایل:

```bash
#!/bin/bash
# This is our first script.
echo "Hello World!"
```

### خط اول — Shebang

```bash
#!/bin/bash
```

این خط به سیستم‌عامل می‌گوید که این فایل را با `/bin/bash` اجرا کن. **همیشه باید اولین خط اسکریپت باشد.**

### کامنت‌گذاری

```bash
# این یک کامنت است
```

هر چیزی بعد از `#` تا پایان خط، نادیده گرفته می‌شود. کامنت‌گذاری عادتی خوب است.

---

## ۲. قابل اجرا کردن اسکریپت

```bash
chmod 755 hello_world
```

یا:

```bash
chmod +x hello_world
```

---

## ۳. اجرای اسکریپت

```bash
./hello_world
```

```
Hello World!
```

> **چرا `./`؟** شل برای اجرا در مسیرهای `PATH` جستجو می‌کند. چون پوشه‌ی جاری معمولاً در `PATH` نیست، باید با `./` مسیر دقیق را مشخص کنیم.

---

## ۴. قرار دادن اسکریپت در مسیر `PATH`

برای دسترسی از هرجایی:

```bash
mkdir -p ~/bin
cp hello_world ~/bin/
```

اگر `~/bin` در `PATH` نیست:

```bash
export PATH="$HOME/bin:$PATH"
```

این خط را به `~/.bashrc` اضافه کن تا دائمی شود:

```bash
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

حالا می‌توان از هرجا اجرا کرد:

```bash
hello_world
```

---

## یک اسکریپت کاربردی‌تر

```bash
#!/bin/bash

# script_info — نمایش اطلاعات سیستم

echo "System Information Report"
echo "========================="
echo ""
echo "Hostname: $(hostname)"
echo "Uptime: $(uptime)"
echo "Disk Usage:"
df -h
echo ""
echo "Memory Usage:"
free -h
```

```bash
chmod 755 script_info
./script_info
```

---

## خلاصه

| مرحله | دستور |
|-------|-------|
| ایجاد فایل | `nano script_name` |
| شروع با shebang | `#!/bin/bash` |
| قابل اجرا کردن | `chmod 755 script_name` |
| اجرا | `./script_name` |

---

*فصل بعدی: [فصل ۲۵ — شروع یک پروژه](ch25-starting-a-project.md)*
