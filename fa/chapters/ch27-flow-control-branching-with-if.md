---
layout: default
---
# فصل ۲۷ — کنترل جریان: شاخه‌بندی با if
### Chapter 27 – Flow Control: Branching with if

---

## دستور `if`

```bash
if commands; then
    commands
elif commands; then
    commands
else
    commands
fi
```

### مثال ساده

```bash
x=5

if [ $x -eq 5 ]; then
    echo "x equals 5"
else
    echo "x does not equal 5"
fi
```

---

## کد خروجی (Exit Status)

هر دستور لینوکس یک **کد خروجی** (exit status) برمی‌گرداند:
- `0` = موفق
- هر عدد غیر از صفر = خطا

```bash
ls /usr/bin
echo $?      # 0

ls /non_existent
echo $?      # 2
```

`if` بر اساس کد خروجی تصمیم می‌گیرد.

---

## دستور `test`

`test` شرط‌ها را ارزیابی می‌کند. دو نوع نوشتار دارد:

```bash
test expression
[ expression ]      # معادل — فاصله‌ها ضروری‌اند
```

### مقایسه‌ی فایل

| عبارت | معنا |
|-------|------|
| `-e file` | فایل وجود دارد |
| `-f file` | فایل معمولی است |
| `-d file` | پوشه است |
| `-r file` | خواندنی است |
| `-w file` | نوشتنی است |
| `-x file` | اجرایی است |
| `-s file` | اندازه‌اش بزرگ‌تر از صفر است |
| `file1 -nt file2` | file1 جدیدتر از file2 است |
| `file1 -ot file2` | file1 قدیمی‌تر از file2 است |

```bash
if [ -d ~/bin ]; then
    echo "directory exists"
fi
```

### مقایسه‌ی رشته

| عبارت | معنا |
|-------|------|
| `str1 = str2` | برابرند |
| `str1 != str2` | نابرابرند |
| `-z string` | رشته خالی است |
| `-n string` | رشته خالی نیست |

```bash
if [ "$USER" = "root" ]; then
    echo "You are root"
fi
```

> **نکته:** همیشه متغیرها را در داخل `"..."` بگذارید تا از خطای فاصله جلوگیری شود.

### مقایسه‌ی عددی

| عبارت | معنا |
|-------|------|
| `n1 -eq n2` | برابر |
| `n1 -ne n2` | نابرابر |
| `n1 -lt n2` | کوچک‌تر |
| `n1 -le n2` | کوچک‌تر یا مساوی |
| `n1 -gt n2` | بزرگ‌تر |
| `n1 -ge n2` | بزرگ‌تر یا مساوی |

```bash
if [ $count -ge 10 ]; then
    echo "count is 10 or more"
fi
```

---

## براکت مدرن `[[ ]]`

`[[ ]]` نسخه‌ی پیشرفته‌ی `[ ]` است:

```bash
[[ expression ]]
```

مزایا:
- پشتیبانی از عملگر `&&` و `||`
- مقایسه با wildcard: `[[ $str == foo* ]]`
- مقایسه با regex: `[[ $str =~ regex ]]`

```bash
if [[ $FILE == *.jpg ]]; then
    echo "JPEG file"
fi

if [[ $str =~ ^[0-9]+$ ]]; then
    echo "is a number"
fi
```

---

## عملگرهای منطقی

### داخل `[ ]`:

| عملگر | معنا |
|-------|------|
| `! expr` | NOT |
| `expr1 -a expr2` | AND |
| `expr1 -o expr2` | OR |

### داخل `[[ ]]` یا بین دستورها:

```bash
[[ expr1 && expr2 ]]    # AND
[[ expr1 || expr2 ]]    # OR

if [ -f file ] && [ -r file ]; then
    echo "file exists and is readable"
fi
```

---

## `(( ))` — ارزیابی حسابی

برای مقایسه‌ی عددی با نماد ریاضی:

```bash
if (( $x == 5 )); then
    echo "x is 5"
fi

if (( $a > $b )); then
    echo "a is greater"
fi
```

---

## مثال کامل — اسکریپت سیستم با شرط

```bash
#!/bin/bash

TITLE="System Information Report"

report_home_space () {
    if [[ "$(id -u)" -eq 0 ]]; then
        echo "<h2>Home Space (All Users)</h2>"
        echo "<pre>$(du -sh /home/*)</pre>"
    else
        echo "<h2>Home Space ($USER)</h2>"
        echo "<pre>$(du -sh $HOME)</pre>"
    fi
    return
}
```

---

*فصل بعدی: [فصل ۲۸ — خواندن ورودی از صفحه‌کلید](ch28-reading-keyboard-input)*
