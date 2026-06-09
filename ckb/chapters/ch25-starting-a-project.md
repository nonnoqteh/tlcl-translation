---
layout: default
---
# بابەتی ٢٥ — دەستپێکردنی پڕۆژەیەک
### Chapter 25 – Starting a Project

---

لەم بابەتەدا پڕۆژەیەکی ڕاستەقینە دەستپێدەکەین — ئیسکریپتێک کە **ڕاپۆرتی حاڵەتی سیستەم** دروستدەکات. ئەم پڕۆژەیە لە بابەتە داهاتووەکاندا تەواودەبێت.

---

## دیزاینی سەرەتایی

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

## گۆڕاوەکان و ئەنجامەکان

### دیاریکردنی گۆڕاو

```bash
NAME="value"
```

### بەکارهێنانی گۆڕاو

```bash
echo $NAME
echo "$NAME"       # باشتر — لە ناو double-quote
echo "${NAME}"     # ڕوونترین شێواز
```

### ئەنجامەکان (Constants)

بەپێی ڕێکارەکان، ناوی ئەنجامەکان بە پیتی گەورە دەنووسرێن:

```bash
TITLE="System Report"
MAX_SIZE=100
```

### گۆڕاوەکانی پێش پێناسەکراو

| گۆڕاو | بەها |
|-------|-----|
| `$HOSTNAME` | ناوی میزبان |
| `$USER` | ناوی بەکارهێنەری ئێستا |
| `$HOME` | پووشپەری خانە |
| `$PATH` | ڕێگاکانی گەڕان |
| `$SHELL` | شێلی ئێستا |

### جێگرینی فەرمان (Command Substitution)

```bash
CURRENT_TIME=$(date +"%x %r %Z")
echo $CURRENT_TIME
```

---

## Here Document

بۆ نووسینی چەند خێڵی دەق، لە **here document** بەکاردەهێنین:

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

- `<< _EOF_` دەستپێکی here document نیشاندەدات
- `_EOF_` (یان هەر وشەیەکی تر) کۆتایییەکەی دیاردەکات
- لە ناو here document گۆڕاوەکان فراوان دەبن

ئەگەر نەمانەوێت گۆڕاوەکان فراوان ببن:

```bash
cat << '_EOF_'
ئەمە $TITLE فراوان نابێت
_EOF_
```

---

## پاراستنی ئاوتپوت لە فایل

```bash
./sys_info_page > sys_info.html
```

---

## ئیسکریپتی تەواوی بابەت

```bash
#!/bin/bash

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

*بابەتی داهاتوو: [بابەتی ٢٦ — دیزاینی لە سەرەوە بۆ خوارەوە](ch26-top-down-design)*
