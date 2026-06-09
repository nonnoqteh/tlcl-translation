---
layout: default
---
# بابەتی ٣٠ — چارەسەرکردنی کێشەکان
### Chapter 30 – Troubleshooting

---

## جۆرەکانی باوی هەڵە

### ١. هەڵەی ڕێزمانی (Syntactic Error)

```bash
# هەڵە: ji bîr kirin fi
if [[ $x -eq 1 ]]; then
    echo "one"
# fi ji bîr kra!
```

ئاوتپوت:

```
script.sh: line 10: syntax error: unexpected end of file
```

### ٢. هەڵەی لۆژیکی (Logical Error)

ئیسکریپت جێبەجێدەبێت بەڵام ئەنجامی دروست ناداتەوە:

```bash
if [[ $x -gt 10 ]]; then
    echo "x is less than 10"    # پەیام پێچەوانەیە!
fi
```

---

## ئامرازەکانی چارەسەرکردنی کێشە

### ١. `bash -x` — حاڵەتی شوێنکەوتن

```bash
bash -x script.sh
```

یان لە سەرەتای ئیسکریپت:

```bash
#!/bin/bash
set -x    # چالاککردنی شوێنکەوتن
```

ئاوتپوتی نموونە:

```
+ TITLE=System Information
+ date +%x %r %Z
+ CURRENT_TIME=01/01/2025 10:00:00 UTC
+ echo System Information
System Information
```

هەر خێڵێک کە بە `+` دەستدەپێکات فەرمانێکە کە جێبەجێدەبێت.

### ٢. `set -e` — وەستان لە هەڵە

```bash
set -e    # ئەگەر فەرمانێک هەڵە بدات، ئیسکریپت بوەستێت
```

### ٣. `set -u` — هەڵە بۆ گۆڕاوی پێناسەنەکراو

```bash
set -u    # بەکارهێنانی گۆڕاوی پێناسەنەکراو = هەڵە
```

### ٤. تێکەڵکردنی بەسوود

```bash
#!/bin/bash
set -euo pipefail
```

---

## تەکنیکەکانی چارەسەرکردنی کێشە

### زیادکردنی `echo` بۆ پشکنین

```bash
echo "DEBUG: x = $x"
echo "DEBUG: Entering function foo"
```

### پشکنینی کۆدی دەرچوون

```bash
ls /some/path
echo "Exit status: $?"
```

### بەکارهێنانی `trap`

```bash
trap 'echo "Line $LINENO: command failed"' ERR
```

---

## هەڵەکانی باو

### لەبیرکردنی double-quote

```bash
# هەڵە — ئەگەر FILE مەودایەکی تێدابێت
if [ -f $FILE ]; then

# دروست
if [ -f "$FILE" ]; then
```

### بەکارهێنانی `=` بەجێی `-eq`

```bash
# هەڵە — بەراوردکردنی ستریمگ، نەک ژمارەیی
if [ $count = 5 ]; then

# دروست — بەراوردکردنی ژمارەیی
if [ $count -eq 5 ]; then
```

### لەبیرکردنی مەودا لە `[ ]`

```bash
# هەڵە
if [$x -eq 5]; then

# دروست
if [ $x -eq 5 ]; then
```

---

## نووسینی ئیسکریپتی بیربەکار

```bash
#!/bin/bash
set -euo pipefail

readonly CONFIG_FILE="/etc/myapp.conf"

if [[ ! -f "$CONFIG_FILE" ]]; then
    echo "Error: config file not found: $CONFIG_FILE" >&2
    exit 1
fi

echo "Error: something went wrong" >&2
```

---

*بابەتی داهاتوو: [بابەتی ٣١ — کۆنترۆڵی جەریان: لقبەندی بە case](ch31-flow-control-branching-with-case)*
