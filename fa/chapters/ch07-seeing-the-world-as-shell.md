---
layout: default
---
# فصل ۷ — دیدن دنیا از چشم شل
### Chapter 7 – Seeing the World as the Shell Sees It

---

در این فصل از «جادوی» خط فرمان پرده برمی‌داریم — اتفاقاتی که وقتی Enter را می‌فشاریم رخ می‌دهند. تنها دستور جدید این فصل:

- `echo` — نمایش یک خط متن

---

## بسط (Expansion)
### Expansion

هر بار که دستوری تایپ می‌کنیم و Enter می‌زنیم، bash چندین جایگزینی روی متن انجام می‌دهد **قبل از اینکه** دستور را اجرا کند. این فرآیند **بسط** (expansion) نام دارد.

دستور `echo` فقط آرگومان‌هایش را چاپ می‌کند:

```bash
echo this is a test
```

```
this is a test
```

حالا این را امتحان کنید:

```bash
echo *
```

```
Desktop Documents ls-output.txt Music Pictures Public Templates Videos
```

چرا `*` چاپ نشد؟ چون شل `*` را قبل از اجرای `echo` **بسط** داد. `echo` اصلاً ستاره ندید — فقط نتیجه‌ی بسط‌یافته را دید.

---

## بسط نام مسیر (Pathname Expansion)
### Pathname Expansion

مکانیزمی که کاراکترهای جایگزین کار می‌کنند **بسط نام مسیر** نام دارد:

```bash
echo D*
```

```
Desktop Documents
```

```bash
echo *s
```

```
Documents Pictures Templates Videos
```

```bash
echo [[:upper:]]*
```

```
Desktop Documents Music Pictures Public Templates Videos
```

```bash
echo /usr/*/share
```

```
/usr/kerberos/share /usr/local/share
```

> **فایل‌های پنهان و بسط:** بسط `*` فایل‌های پنهان (نقطه‌دار) را نشان نمی‌دهد. برای نمایش آن‌ها از الگوی `.[!.]*` استفاده کنید، یا از `ls -A`.

---

## بسط تیلدا (Tilde Expansion)
### Tilde Expansion

کاراکتر `~` در ابتدای یک کلمه به پوشه‌ی خانگی بسط می‌یابد:

```bash
echo ~
```

```
/home/me
```

```bash
echo ~bob
```

```
/home/bob
```

---

## بسط حسابی (Arithmetic Expansion)
### Arithmetic Expansion

شل می‌تواند عبارات حسابی را ارزیابی کند:

```bash
echo $((2 + 2))
```

```
4
```

فرم کلی: `$((expression))`

عملگرهای پشتیبانی‌شده:

| عملگر | توضیح |
|-------|-------|
| `+` | جمع |
| `-` | تفریق |
| `*` | ضرب |
| `/` | تقسیم (فقط اعداد صحیح) |
| `%` | باقیمانده |
| `**` | توان |

مثال:

```bash
echo $(($((5**2)) * 3))
```

```
75
```

یا با پرانتزگذاری:

```bash
echo $(((5**2) * 3))
```

```
75
```

```bash
echo Five divided by two equals $((5/2))
echo with $((5%2)) left over.
```

```
Five divided by two equals 2
with 1 left over.
```

---

## بسط آکولاد (Brace Expansion)
### Brace Expansion

عجیب‌ترین بسط است! با آن می‌توانیم چندین رشته از یک الگو بسازیم:

```bash
echo Front-{A,B,C}-Back
```

```
Front-A-Back Front-B-Back Front-C-Back
```

با محدوده‌ی اعداد:

```bash
echo Number_{1..5}
```

```
Number_1 Number_2 Number_3 Number_4 Number_5
```

با صفر جلو (bash 4.0+):

```bash
echo {01..15}
```

```
01 02 03 04 05 06 07 08 09 10 11 12 13 14 15
```

با حروف معکوس:

```bash
echo {Z..A}
```

```
Z Y X W V U T S R Q P O N M L K J I H G F E D C B A
```

بسط‌های تودرتو:

```bash
echo a{A{1,2},B{3,4}}b
```

```
aA1b aA2b aB3b aB4b
```

**کاربرد عملی:** ساختن پوشه‌های سازمان‌یافته:

```bash
mkdir Photos
cd Photos
mkdir {2007..2009}-{01..12}
ls
```

```
2007-01  2007-07  2008-01  2008-07  2009-01  2009-07
2007-02  2007-08  2008-02  2008-08  2009-02  2009-08
...
```

---

## بسط پارامتر (Parameter Expansion)
### Parameter Expansion

در این فصل فقط کوتاه به آن می‌پردازیم — بعداً عمیق‌تر بررسی می‌کنیم. متغیرهای محیطی زیادی برای بررسی داریم:

```bash
echo $USER
```

```
me
```

برای دیدن همه‌ی متغیرها:

```bash
printenv | less
```

> **نکته:** اگر نام متغیر را اشتباه تایپ کنید، نتیجه رشته‌ی خالی خواهد بود:
> ```bash
> echo $SUER
> ```
> (هیچ چیزی چاپ نمی‌شود)

---

## جایگزینی دستور (Command Substitution)
### Command Substitution

خروجی یک دستور را می‌توانیم به عنوان بسط استفاده کنیم:

```bash
echo $(ls)
```

```
Desktop Documents ls-output.txt Music Pictures Public Templates Videos
```

یکی از موارد کاربردی:

```bash
ls -l $(which cp)
```

```
-rwxr-xr-x 1 root root 71516 2007-12-05 08:58 /bin/cp
```

مسیر کامل `cp` را نگفتیم — از `which cp` استفاده کردیم. می‌توانیم کل لوله‌کشی‌ها را هم استفاده کنیم:

```bash
file $(ls -d /usr/bin/* | grep zip)
```

نحو قدیمی (با backtick) هم پشتیبانی می‌شود:

```bash
ls -l `which cp`
```

---

## نقل‌قول‌گذاری (Quoting)
### Quoting

حالا باید یاد بگیریم چطور بسط‌ها را کنترل کنیم. مثلاً:

```bash
echo this is a    test
```

```
this is a test
```

شل فاصله‌های اضافی را حذف کرد. یا:

```bash
echo The total is $100.00
```

```
The total is 00.00
```

شل `$1` را به رشته‌ی خالی بسط داد. برای کنترل این رفتار، **نقل‌قول‌گذاری** را به کار می‌بریم.

### نقل‌قول دوتایی (Double Quotes)

داخل نقل‌قول دوتایی، همه‌ی کاراکترهای خاص معنی ویژه‌شان را از دست می‌دهند، **به جز** `$`، `\` و `` ` ``. یعنی:
- تقسیم کلمات حذف می‌شود
- بسط نام مسیر حذف می‌شود
- بسط تیلدا حذف می‌شود
- بسط آکولاد حذف می‌شود
- ولی بسط پارامتر، بسط حسابی و جایگزینی دستور **انجام می‌شوند**

مثال کاربردی — کار با فایل‌هایی که نامشان فاصله دارد:

```bash
ls -l two words.txt
```

```
ls: cannot access two: No such file or directory
ls: cannot access words.txt: No such file or directory
```

```bash
ls -l "two words.txt"
```

```
-rw-rw-r-- 1 me me 18 2016-02-20 13:03 two words.txt
```

```bash
mv "two words.txt" two_words.txt
```

نقل‌قول دوتایی تقسیم کلمات را غیرفعال می‌کند:

```bash
echo "this is a    test"
```

```
this is a    test
```

### نقل‌قول تکی (Single Quotes)

اگر بخواهیم **همه‌ی** بسط‌ها را غیرفعال کنیم:

```bash
echo 'text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER'
```

```
text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER
```

مقایسه‌ی سه حالت:

```bash
echo text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER
```

```
text /home/me/ls-output.txt a b foo 4 me
```

```bash
echo "text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER"
```

```
text ~/*.txt {a,b} foo 4 me
```

```bash
echo 'text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER'
```

```
text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER
```

### فرار از کاراکترها (Escaping Characters)

برای نقل‌قول تنها یک کاراکتر، از `\` (بک‌اسلش) استفاده می‌کنیم:

```bash
echo "The balance for user $USER is: \$5.00"
```

```
The balance for user me is: $5.00
```

برای استفاده از کاراکترهای خاص در نام فایل:

```bash
mv bad\&filename good_filename
```

**دنباله‌های فرار بک‌اسلش:**

| دنباله | معنی |
|--------|------|
| `\a` | زنگ (صدا) |
| `\b` | Backspace |
| `\n` | خط جدید |
| `\r` | بازگشت کالسکه |
| `\t` | Tab |

با `-e` در `echo` می‌توان این دنباله‌ها را تفسیر کرد:

```bash
sleep 10; echo -e "Time's up\a"
```

---

## خلاصه

با پیشرفت در استفاده از شل، بسط‌ها و نقل‌قول‌گذاری بیشتر و بیشتر مورد استفاده قرار می‌گیرند. درک درستی از آن‌ها ضروری است — بدون این درک، شل همیشه منبعی از ابهام و سردرگمی خواهد بود و بخش زیادی از قدرتش هدر می‌رود.

---

## مطالعه‌ی بیشتر

- صفحه‌ی `man bash` بخش‌های کاملی درباره‌ی بسط و نقل‌قول دارد
- مرجع bash: [Bash Reference Manual](http://www.gnu.org/software/bash/manual/bashref.html)

---

*فصل بعدی: [فصل ۸ — ترفندهای پیشرفته‌ی صفحه‌کلید](ch08-advanced-keyboard.md)*
