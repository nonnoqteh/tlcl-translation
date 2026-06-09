---
layout: default
---
# بابەتی ٢٧ — کۆنترۆڵی جەریان: لقبەندی بە if
### Chapter 27 – Flow Control: Branching with if

---

## فەرمانی `if`

```bash
if commands; then
    commands
elif commands; then
    commands
else
    commands
fi
```

### نموونەی سادە

```bash
x=5

if [ $x -eq 5 ]; then
    echo "x equals 5"
else
    echo "x does not equal 5"
fi
```

---

## کۆدی دەرچوون (Exit Status)

هەر فەرمانێکی لینوکس **کۆدی دەرچوون** (exit status) دەگەڕێنێتەوە:
- `0` = سەرکەوتوو
- هەر ژمارەیەکی جگە لە سفر = هەڵە

```bash
ls /usr/bin
echo $?      # 0

ls /non_existent
echo $?      # 2
```

`if` بەپێی کۆدی دەرچوون بڕیار دەدات.

---

## فەرمانی `test`

`test` مەرجەکان دەسەنجێنێت. دوو شێوازی نووسینی هەیە:

```bash
test expression
[ expression ]      # هاوتاکە — مەودای خاڵ پێویستە
```

### بەراوردکردنی فایل

| دەربڕین | مانا |
|---------|------|
| `-e file` | فایل هەیە |
| `-f file` | فایلی ئاسایییە |
| `-d file` | پووشپەرە |
| `-r file` | خوێندنەوەی پێیەتی |
| `-w file` | نووسینی پێیەتی |
| `-x file` | جێبەجێکردنی پێیەتی |
| `-s file` | قەبارەکەی لە سفر گەورەترە |
| `file1 -nt file2` | file1 لە file2 نوێترە |
| `file1 -ot file2` | file1 لە file2 کۆنترە |

```bash
if [ -d ~/bin ]; then
    echo "directory exists"
fi
```

### بەراوردکردنی ستریمگ

| دەربڕین | مانا |
|---------|------|
| `str1 = str2` | یەکسانن |
| `str1 != str2` | نایەکسانن |
| `-z string` | ستریمگ خاڵییە |
| `-n string` | ستریمگ خاڵی نییە |

```bash
if [ "$USER" = "root" ]; then
    echo "You are root"
fi
```

> **تێبینی:** هەمیشە گۆڕاوەکان لە ناو `"..."` بنێ تا لە هەڵەی مەودا دوور بێیتەوە.

### بەراوردکردنی ژمارەیی

| دەربڕین | مانا |
|---------|------|
| `n1 -eq n2` | یەکسان |
| `n1 -ne n2` | نایەکسان |
| `n1 -lt n2` | بچووکتر |
| `n1 -le n2` | بچووکتر یان یەکسان |
| `n1 -gt n2` | گەورەتر |
| `n1 -ge n2` | گەورەتر یان یەکسان |

```bash
if [ $count -ge 10 ]; then
    echo "count is 10 or more"
fi
```

---

## براکێتی مۆدێرن `[[ ]]`

`[[ ]]` وەرزیۆنی پێشکەوتووی `[ ]`ە:

```bash
[[ expression ]]
```

تایبەتمەندیەکان:
- پشتیوانی لە `&&` و `||`
- بەراوردکردن بە wildcard: `[[ $str == foo* ]]`
- بەراوردکردن بە regex: `[[ $str =~ regex ]]`

```bash
if [[ $FILE == *.jpg ]]; then
    echo "JPEG file"
fi

if [[ $str =~ ^[0-9]+$ ]]; then
    echo "is a number"
fi
```

---

## عەملگەرەکانی لۆژیکی

```bash
[[ expr1 && expr2 ]]    # AND
[[ expr1 || expr2 ]]    # OR

if [ -f file ] && [ -r file ]; then
    echo "file exists and is readable"
fi
```

---

## `(( ))` — سەنجینی ژمارەیی

```bash
if (( $x == 5 )); then
    echo "x is 5"
fi

if (( $a > $b )); then
    echo "a is greater"
fi
```

---

*بابەتی داهاتوو: [بابەتی ٢٨ — خوێندنەوەی تێخستنی کیبۆرد](ch28-reading-keyboard-input)*
