# فصل ۳۰ — رفع اشکال
### Chapter 30 – Troubleshooting

---

## انواع رایج خطا

### ۱. خطای نحوی (Syntactic Error)

خطاهایی که قوانین دستوری زبان را نقض می‌کنند:

```bash
# خطا: فراموش کردن fi
if [[ $x -eq 1 ]]; then
    echo "one"
# fi فراموش شد!
```

خروجی:

```
script.sh: line 10: syntax error: unexpected end of file
```

### ۲. خطای منطقی (Logical Error)

اسکریپت اجرا می‌شود اما نتیجه‌ی درست نمی‌دهد:

```bash
# منطق اشتباه
if [[ $x -gt 10 ]]; then
    echo "x is less than 10"    # پیغام برعکس است!
fi
```

---

## ابزارهای رفع اشکال

### ۱. `bash -x` — حالت ردیابی

```bash
bash -x script.sh
```

یا در اول اسکریپت:

```bash
#!/bin/bash
set -x    # فعال کردن ردیابی
```

خروجی نمونه:

```
+ TITLE=System Information
+ date +%x %r %Z
+ CURRENT_TIME=01/01/2025 10:00:00 UTC
+ echo System Information
System Information
```

هر خطی که با `+` شروع می‌شود دستوری است که اجرا می‌شود.

### ۲. `set -e` — توقف در خطا

```bash
set -e    # اگر دستوری خطا دهد، اسکریپت متوقف شود
```

### ۳. `set -u` — خطا برای متغیر تعریف‌نشده

```bash
set -u    # استفاده از متغیر تعریف‌نشده = خطا
```

### ۴. ترکیب مفید

```bash
#!/bin/bash
set -euo pipefail
```

- `-e` — توقف در خطا
- `-u` — خطا برای متغیر تعریف‌نشده
- `-o pipefail` — خطای pipe را منتشر کن

---

## تکنیک‌های رفع اشکال

### افزودن `echo` برای بررسی

```bash
echo "DEBUG: x = $x"
echo "DEBUG: Entering function foo"
```

### بررسی کد خروجی

```bash
ls /some/path
echo "Exit status: $?"
```

### استفاده از `trap`

```bash
trap 'echo "Line $LINENO: command failed"' ERR
```

---

## خطاهای رایج

### فراموش کردن double-quote

```bash
# خطا — اگر FILE حاوی فاصله باشد
if [ -f $FILE ]; then

# درست
if [ -f "$FILE" ]; then
```

### استفاده از `=` به جای `-eq`

```bash
# اشتباه — مقایسه‌ی رشته‌ای، نه عددی
if [ $count = 5 ]; then

# درست — مقایسه‌ی عددی
if [ $count -eq 5 ]; then
```

### فراموش کردن فاصله در `[ ]`

```bash
# خطا
if [$x -eq 5]; then

# درست
if [ $x -eq 5 ]; then
```

---

## نوشتن اسکریپت دفاعی (Defensive Scripting)

```bash
#!/bin/bash
set -euo pipefail

# بررسی وجود فایل قبل از استفاده
readonly CONFIG_FILE="/etc/myapp.conf"

if [[ ! -f "$CONFIG_FILE" ]]; then
    echo "Error: config file not found: $CONFIG_FILE" >&2
    exit 1
fi

# پیغام‌های خطا به stderr برود
echo "Error: something went wrong" >&2
```

---

*فصل بعدی: [فصل ۳۱ — کنترل جریان: شاخه‌بندی با case](ch31-flow-control-branching-with-case.md)*
