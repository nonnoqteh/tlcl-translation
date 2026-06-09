---
layout: default
---
# بابەتی ١٧ — گەڕان لە فایلەکان
### Chapter 17 – Searching for Files

---

فەرمانەکانی ئەم بابەتە:

- `locate` — گەڕان بۆ فایل بە ناو
- `find` — گەڕان بۆ فایل لە درەختی پووشپەر
- `xargs` — دروستکردن و جێبەجێکردنی فەرمان لە ئینپوتی ستاندارد
- `touch` — گۆڕینی کاتی فایل
- `stat` — نیشاندانی زانیاری فایل

---

## `locate` — گەڕانی خێرا

`locate` لە **پایگەی داتا**یەک لە ناوی فایلەکان دەگەڕێت:

```bash
locate bin/zip
```

```
/usr/bin/zip
/usr/bin/zipcloak
/usr/bin/zipgrep
/usr/bin/zipinfo
```

بە لووڵەکەشی بۆ `grep` نتیجەکان فیلتەر دەکەین:

```bash
locate zip | grep bin
```

> **تێبینی:** پایگەی داتای `locate` بە بنچینە ڕۆژانە نوێدەبێتەوە. بۆ نوێکردنەوەی دەستی:
> ```bash
> sudo updatedb
> ```

---

## `find` — گەڕانی پێشکەوتوو

`find` لە کاتی ڕاستەقینە لە سیستەمی فایل دەگەڕێت — لە `locate` کوندتر بەڵام زۆر هێزدارتر.

### گەڕانی سادە

```bash
find ~
find ~ | wc -l
```

### گەڕان بەپێی جۆر

```bash
find ~ -type f    # تەنها فایلی ئاسایی
find ~ -type d    # تەنها پووشپەر
find ~ -type l    # تەنها لینکی سیمبۆلی
```

### گەڕان بەپێی ناو

```bash
find ~ -name "*.jpg"
find ~ -iname "*.jpg"    # بەبێ ئاگاداری گەورە/بچووک
```

### گەڕان بەپێی قەبارە

```bash
find ~ -size +1M          # گەورەتر لە ١ مێگابایت
find ~ -size -10k         # بچووکتر لە ١٠ کیلۆبایت
find ~ -size +100M
```

واحیدەکان: `b` (بلۆک)، `c` (بایت)، `k` (کیلۆبایت)، `M` (مێگابایت)، `G` (گیگابایت)

### گەڕان بەپێی کات

```bash
find ~ -mtime -3        # گۆڕدراو لە ٣ ڕۆژی ڕابردوو
find ~ -newer file.txt  # نوێتر لە file.txt
```

### گەڕان بەپێی مۆڵەت

```bash
find ~ -perm 0600
find ~ -perm /0222
```

### گەڕان بەپێی بەکارهێنەر/گروپ

```bash
find ~ -user me
find ~ -group staff
find /tmp -nouser
```

### عەملگەرەکانی لۆژیکی

```bash
find ~ -type f -name "*.jpg"           # AND (بنچینەیی)
find ~ -name "*.jpg" -o -name "*.png"  # OR
find ~ -not -name "*.jpg"              # NOT
```

---

## جێبەجێکردنی کارەکان لەسەر نتیجەکان

### `find -exec` — جێبەجێکردنی ڕاستەوخۆ

```bash
find ~ -type f -name "*.jpg" -exec ls -lh {} \;
```

- `{}` جێگرینی هەر ناوی فایلێکی دۆزراوە
- `\;` کۆتایی فەرمان

بۆ خێراتر:

```bash
find ~ -type f -name "*.jpg" -exec ls -lh {} +
```

### دۆزینەوە و سڕینەوەی فایلی دیاری

```bash
# یەکەم تماشا بکە
find /tmp -name "tmp*" -type f

# پاشان بسڕەرەوە
find /tmp -name "tmp*" -type f -delete
```

> ⚠️ لەگەڵ `-delete` ئاگادار بە! یەکەم بەبێ `-delete` جێبەجێبکە.

---

## `xargs` — دروستکردنی فەرمان لە ئینپوت

```bash
find ~ -type f -name "*.jpg" | xargs ls -lh
```

بۆ فایلی کە ناویاندا فاسیلەیە:

```bash
find ~ -type f -name "*.jpg" -print0 | xargs --null ls -lh
```

---

## `touch` — گۆڕینی کاتی فایل

```bash
touch file.txt
```

ئەگەر فایل نەبوو، خالی دروستدەبێت. ئەگەر بوو، کاتەکەی نوێدەبێتەوە.

---

## `stat` — زانیاری تەواوی فایل

```bash
stat file.txt
```

```
  File: file.txt
  Size: 1234
Access: (0644/-rw-r--r--)  Uid: ( 1000/    me)
Modify: 2025-06-09 10:59:30
```

---

## کورتە

`find` یەکێک لە هێزداترین ئامرازەکانی لینوکسە. تێکەڵکردنی لەگەڵ `-exec` یان `xargs` ئەمکانیەکی بێژمارە دەدات.

---

*بابەتی داهاتوو: [بابەتی ١٨ — ئەرشیڤ و پشتیوانی](ch18-archiving-and-backup.md)*
