---
layout: default
---
# فصل ۳۶ — موارد خاص
### Chapter 36 – Exotica

---

این فصل تکنیک‌های پیشرفته‌تری را که کمتر استفاده می‌شوند، اما بسیار قدرتمند هستند، پوشش می‌دهد.

---

## گروه‌بندی دستور (Command Grouping)

دو روش برای گروه‌بندی دستورها:

### ۱. زیرشل `( )`

```bash
(cd /tmp; ls)
```

دستورها در یک **زیرشل** (subshell) جداگانه اجرا می‌شوند. تغییر متغیر و پوشه‌ی جاری روی شل اصلی تأثیر ندارد.

### ۲. گروه‌بند `{ }`

```bash
{ cd /tmp; ls; }
```

دستورها در **شل جاری** اجرا می‌شوند. تغییرات پایدار می‌مانند.

### مثال کاربردی — مسیریابی گروهی

```bash
{ echo "Header"; cat file.txt; echo "Footer"; } > output.txt
```

---

## جایگزینی فرآیند (Process Substitution)

```bash
command <(list)
command >(list)
```

خروجی دستور را مثل یک فایل در نظر می‌گیرد:

```bash
diff <(ls dir1) <(ls dir2)
```

فایل‌های دو پوشه را مقایسه می‌کند — بدون نیاز به فایل موقت.

```bash
tee >(gzip > output.gz) | head
```

---

## `trap` — گرفتن سیگنال

```bash
trap argument signal...
```

```bash
#!/bin/bash

cleanup () {
    echo "Cleaning up..."
    rm -f /tmp/mytemp*
    exit
}

trap cleanup EXIT INT TERM

# ایجاد فایل موقت
tmpfile=$(mktemp /tmp/mytemp.XXXXXX)
echo "Working with $tmpfile"
sleep 30
```

### سیگنال‌های رایج

| سیگنال | معنا |
|--------|------|
| `EXIT` | خروج از اسکریپت |
| `INT` | Ctrl+C |
| `TERM` | درخواست پایان |
| `KILL` | پایان اجباری |
| `HUP` | قطع اتصال |

---

## آرایه‌های ناپیوسته (Sparse Arrays)

```bash
a[0]="first"
a[100]="hundredth"
a[200]="two hundredth"

echo ${#a[@]}       # 3 (تعداد عناصر)
echo ${!a[@]}       # 0 100 200 (اندیس‌ها)
```

---

## `eval` — ارزیابی پویای دستور

```bash
cmd="ls -la"
eval $cmd
```

> ⚠️ **احتیاط:** `eval` ورودی خارجی را هرگز مستقیم اجرا نکنید — خطر امنیتی دارد.

---

## `mapfile` / `readarray` — خواندن فایل در آرایه

```bash
mapfile -t lines < /etc/passwd
echo "Lines: ${#lines[@]}"
echo "First: ${lines[0]}"
```

---

## جایگزینی نام‌فایل پیشرفته

### `**` — globbing بازگشتی

```bash
shopt -s globstar
echo **/*.txt       # همه‌ی فایل‌های .txt در همه‌ی زیرپوشه‌ها
```

### `extglob` — الگوهای پیشرفته

```bash
shopt -s extglob

ls !(*.jpg)         # همه‌چیز بجز .jpg
ls *(foo|bar)       # صفر یا بیشتر از foo یا bar
ls +(foo|bar)       # یک یا بیشتر از foo یا bar
ls ?(foo|bar)       # صفر یا یک از foo یا bar
```

---

## `coprocess` — پردازش همزمان

```bash
coproc COPROC_NAME { commands; }
```

```bash
coproc grep foo
echo "test foo bar" >&${COPROC[1]}
read -r result <&${COPROC[0]}
echo "$result"
```

---

## ماژول‌سازی اسکریپت با `source`

```bash
# lib.sh
greet () {
    echo "Hello, $1!"
}

# main.sh
source ./lib.sh
greet "World"
```

یا با نقطه:

```bash
. ./lib.sh
```

---

## خلاصه‌ی پارت ۴

در این پارت یاد گرفتیم:

| فصل | موضوع |
|-----|-------|
| ۲۴ | نوشتن اسکریپت اول |
| ۲۵ | متغیر و here document |
| ۲۶ | توابع و طراحی از بالا به پایین |
| ۲۷ | شرط با `if` |
| ۲۸ | خواندن ورودی با `read` |
| ۲۹ | حلقه‌ی `while` و `until` |
| ۳۰ | رفع اشکال |
| ۳۱ | شاخه‌بندی با `case` |
| ۳۲ | پارامترهای موضعی |
| ۳۳ | حلقه‌ی `for` |
| ۳۴ | رشته‌ها و اعداد |
| ۳۵ | آرایه‌ها |
| ۳۶ | تکنیک‌های پیشرفته |

---

*کتاب «خط فرمان لینوکس» — پایان ترجمه‌ی فارسی*

*بازگشت به [فهرست فصل‌ها](../../README)*
