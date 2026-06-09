---
layout: default
---
# بابەتی ٧ — جیهان لە چاوی شێلەوە
### Chapter 7 – Seeing the World as the Shell Sees It

---

لەم بابەتەدا لە «سیری» خێزانی فەرمان پردەدەهێنین — ئەوانەی کە کاتێک Enter دەفشارین ڕوودەدەن. تەنها فەرمانی نوێ:

- `echo` — نیشاندانی هێڵی دەق

---

## فراوانکردن (Expansion)

هەر جار کە فەرمانێک دەنووسین و Enter دەفشاریین، bash چەند جێگرینێک لەسەر دەقەکە دەکات **پێش** جێبەجێکردنی فەرمان. ئەم پرۆسەیە **فراوانکردن** (expansion) ناودەبردرێت.

```bash
echo this is a test
```

```
this is a test
```

```bash
echo *
```

```
Desktop Documents ls-output.txt Music Pictures Public Templates Videos
```

بۆچی `*` چاپ نەبوو؟ چونکە شێل `*` پێش جێبەجێکردنی `echo` فراوانکرد.

---

## فراوانکردنی ناوی ڕێچکە
### Pathname Expansion

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

> **فایلی شاراوە و فراوانکردن:** `*` فایلی نقطەدار (شاراوە) نانیشاندات. بۆ ئەوانیش، `.[!.]*` بەکاربهێنە.

---

## فراوانکردنی تیلدا
### Tilde Expansion

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

## فراوانکردنی ژمارەیی
### Arithmetic Expansion

```bash
echo $((2 + 2))
```

```
4
```

فۆرمی گشتی: `$((expression))`

عەملگەرەکانی پشتیبانیکراو:

| عەملگەر | ڕوونکردنەوە |
|---------|------------|
| `+` | کۆکردنەوە |
| `-` | لێکردنەوە |
| `*` | لێکدانەوە |
| `/` | دابەشکردن (ژمارەی تەواو) |
| `%` | ماوەی دابەشکردن |
| `**` | توانا |

```bash
echo $(($((5**2)) * 3))
```

```
75
```

---

## فراوانکردنی ئاکۆڵە
### Brace Expansion

```bash
echo Front-{A,B,C}-Back
```

```
Front-A-Back Front-B-Back Front-C-Back
```

```bash
echo Number_{1..5}
```

```
Number_1 Number_2 Number_3 Number_4 Number_5
```

```bash
echo {Z..A}
```

```
Z Y X W V U T S R Q P O N M L K J I H G F E D C B A
```

**بەکارهێنانی کارامە:** دروستکردنی فۆڵدەری ڕێکخراو:

```bash
mkdir Photos
cd Photos
mkdir {2007..2009}-{01..12}
ls
```

---

## فراوانکردنی پارامێتەر
### Parameter Expansion

```bash
echo $USER
```

```
me
```

بۆ بینینی هەموو گۆڕاوەکان:

```bash
printenv | less
```

---

## جێگرینی فەرمان
### Command Substitution

```bash
echo $(ls)
```

```
Desktop Documents ls-output.txt Music Pictures Public Templates Videos
```

```bash
ls -l $(which cp)
```

```
-rwxr-xr-x 1 root root 71516 2007-12-05 08:58 /bin/cp
```

---

## نقلقۆڵی
### Quoting

### نقلقۆڵی دووبارە (Double Quotes)

ناو نقلقۆڵی دووبارەدا، هەموو پیتی تایبەت مانایان لەدەستدەدەن، **جگە لە** `$`، `\` و `` ` ``.

```bash
ls -l "two words.txt"
```

```
-rw-rw-r-- 1 me me 18 2016-02-20 13:03 two words.txt
```

```bash
echo "this is a    test"
```

```
this is a    test
```

### نقلقۆڵی تاک (Single Quotes)

ئەگەر دەمانەوێت **هەموو** فراوانکردنەکان ناچالاک بکەین:

```bash
echo 'text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER'
```

```
text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER
```

### فیرارکردنی پیتەکان (Escaping)

```bash
echo "The balance for user $USER is: \$5.00"
```

```
The balance for user me is: $5.00
```

**زنجیرەی فیرارکردنی بەکسلاش:**

| زنجیرە | مانا |
|--------|------|
| `\a` | دەنگ (زەنگ) |
| `\b` | Backspace |
| `\n` | هێڵی نوێ |
| `\r` | گەڕانەوەی لێخۆر |
| `\t` | Tab |

---

## کورتە

لەگەڵ پێشکەوتن لە بەکارهێنانی شێل، فراوانکردن و نقلقۆڵی زیاتر و زیاتر بەکاردێن. تێگەیشتنی ڕاست لێیان پێویستییەکی بنچینەیییە.

---

*بابەتی داهاتوو: [بابەتی ٨ — ئامرازی پەرەپێدراوی کیبۆرد](ch08-advanced-keyboard)*
