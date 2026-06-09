# فصل ۲۵ — شروع یک پروژه
### Chapter 25 – Starting a Project

---

در این فصل یک پروژه‌ی واقعی شروع می‌کنیم — اسکریپتی که **گزارش وضعیت سیستم** تولید می‌کند. این پروژه در فصل‌های بعدی تکمیل می‌شود.

---

## طراحی اولیه

```bash
#!/bin/bash

# Program to output a system information page

TITLE="System Information Report For $HOSTNAME"
CURRENT_TIME=$(date +"%x %r %Z")
TIMESTAMP="Generated $CURRENT_TIME, by $USER"

echo "<html>
    <head>
        <title>$TITLE</title>
    </head>
    <body>
        <h1>$TITLE</h1>
        <p>$TIMESTAMP</p>
    </body>
</html>"
```

---

## متغیرها و ثابت‌ها

### تعریف متغیر

```bash
NAME="value"
```

### استفاده از متغیر

```bash
echo $NAME
echo "$NAME"       # بهتر است — در داخل double-quote
echo "${NAME}"     # صریح‌ترین حالت
```

### ثابت‌ها (Constants)

طبق قرارداد، نام ثابت‌ها با حروف بزرگ نوشته می‌شود:

```bash
TITLE="System Report"
MAX_SIZE=100
```

### متغیرهای از پیش تعریف‌شده

| متغیر | مقدار |
|-------|-------|
| `$HOSTNAME` | نام میزبان |
| `$USER` | نام کاربر جاری |
| `$HOME` | پوشه‌ی خانه |
| `$PATH` | مسیرهای جستجو |
| `$SHELL` | شل جاری |

### جایگزینی دستور (Command Substitution)

```bash
CURRENT_TIME=$(date +"%x %r %Z")
echo $CURRENT_TIME
```

همچنین با backtick (قدیمی‌تر):

```bash
CURRENT_TIME=`date +"%x %r %Z"`
```

---

## Here Document

برای نوشتن چند خط متن، از **here document** استفاده می‌کنیم:

```bash
cat << _EOF_
<html>
    <head>
        <title>$TITLE</title>
    </head>
    <body>
        <h1>$TITLE</h1>
        <p>$TIMESTAMP</p>
    </body>
</html>
_EOF_
```

- `<< _EOF_` شروع here document را نشان می‌دهد
- `_EOF_` (یا هر کلمه‌ای دیگر) پایان آن را مشخص می‌کند
- داخل here document، متغیرها گسترش می‌یابند

اگر بخواهیم متغیرها گسترش نیابند:

```bash
cat << '_EOF_'
این $TITLE گسترش نمی‌یابد
_EOF_
```

---

## ذخیره‌ی خروجی در فایل

```bash
./sys_info_page > sys_info.html
```

---

## اسکریپت کامل فصل

```bash
#!/bin/bash

# Program to output a system information page

TITLE="System Information Report For $HOSTNAME"
CURRENT_TIME=$(date +"%x %r %Z")
TIMESTAMP="Generated $CURRENT_TIME, by $USER"

cat << _EOF_
<html>
    <head>
        <title>$TITLE</title>
    </head>
    <body>
        <h1>$TITLE</h1>
        <p>$TIMESTAMP</p>
    </body>
</html>
_EOF_
```

---

*فصل بعدی: [فصل ۲۶ — طراحی از بالا به پایین](ch26-top-down-design.md)*
