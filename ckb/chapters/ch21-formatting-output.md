---
layout: default
---
# بابەتی ٢١ — فۆرماتکردنی ئاوتپوت
### Chapter 21 – Formatting Output

---

فەرمانەکانی ئەم بابەتە:

- `nl` — ژمارەکردنی هێڵ
- `fold` — شکاندنی هێڵ بەپێی پانی
- `fmt` — فۆرماتکردنی ساپانەی دەق
- `pr` — ئامادەکردنی دەق بۆ چاپ
- `printf` — فۆرماتکردن و چاپکردنی داتا
- `groff` — سیستەمی فۆرماتکردنی دۆکیومێنت

---

## `nl` — ژمارەکردنی هێڵ

```bash
nl file.txt
```

```
     1	first line
     2	second line
     3	third line
```

**ئۆپشنەکان:**

```bash
nl -ba file.txt     # هەموو هێڵ (ئەوەی خاڵیش دەژمێرێت)
nl -bt file.txt     # تەنها هێڵی دەق (بنچینەیی)
nl -bn file.txt     # بەبێ ژمارەکردن
```

---

## `fold` — شکاندنی هێڵ

```bash
echo "This is a very long line that needs to be wrapped" | fold -w 20
```

```
This is a very long
line that needs to
be wrapped
```

```bash
fold -w 40 -s file.txt    # شکاندن لەسەر فاسیلە (نە ناو وشە)
```

---

## `fmt` — فۆرماتکردنی دەق

```bash
fmt -w 60 file.txt        # پانی ٦٠ کاراکتەر
fmt -u file.txt           # فاسیلەی یەکجۆر
```

`fmt` هێڵەکان دووبارە دەڕووبەندێت بەپێی پان.

---

## `pr` — ئامادەکردن بۆ چاپ

```bash
pr file.txt | head -40
```

```
2025-06-09 10:00                  file.txt                   Page 1


first line
second line
...
```

**ئۆپشنەکانی بەسوود:**

```bash
pr -n file.txt              # لەگەڵ ژمارەی هێڵ
pr -2 file.txt              # دوو ستوون
pr -d file.txt              # دووبارەکردنی فاسیلە
pr -o 5 file.txt            # مارجینی ٥ فاسیلە
```

---

## `printf` — فۆرماتکردنی کودکراو

`printf` لە `echo` بەهێزتره بۆ فۆرماتکردنی خستنەدەر.

```bash
printf "format" arguments
```

### نیشانەکانی فۆرمات

| نیشانە | مانا |
|--------|------|
| `%d` | ژمارەی تەواو (integer) |
| `%f` | ژمارەی ئێستا (floating point) |
| `%s` | دەق (string) |
| `%e` | نیشاندانی زانستی |
| `%%` | نیشانەی `%` ئاسایی |

```bash
printf "%d\n" 42
printf "%.2f\n" 3.14159
printf "%s has %d items\n" "List" 5
printf "%-10s %5d\n" "Item" 42    # راستکاری
```

```
42
3.14
List has 5 items
Item           42
```

### رێکخستنی ستوون

```bash
printf "%s\t%s\t%s\n" "Name" "Score" "Grade"
printf "%-10s %5d %5s\n" "Alice"  95 "A"
printf "%-10s %5d %5s\n" "Bob"    82 "B"
printf "%-10s %5d %5s\n" "Carol"  74 "C"
```

```
Name       Score Grade
Alice         95     A
Bob           82     B
Carol         74     C
```

---

## `groff` — فۆرماتکردنی دۆکیومێنت

`groff` سیستەمی فۆرماتکردنی دۆکیومێنتی هێزداره — بنچینەی کتێبەکانی man.

```bash
# دیاریکردنی صفحەی man
zcat /usr/share/man/man1/ls.1.gz | groff -man -T ascii | less

# گواستنەوەی بۆ PDF
groff -man -T pdf ls.1 > ls.pdf
```

### نموونەی سادەی groff/ms

```bash
cat << EOF | groff -ms -T ascii
.TL
Title Here
.AU
Author Name
.PP
This is the first paragraph of the document.
EOF
```

---

## کورتە

فۆرماتکردنی ئاوتپوت لە لینوکس زنجیرەیەکی فراوانی ئامرازی هەیە — لە ژمارەکردنی سادەی هێڵ بە `nl` تا دروستکردنی PDF بە `groff`. بۆ بەکارهێنانی ڕۆژانە `printf` و `fold` بەسوودترین.

---

*بابەتی داهاتوو: [بابەتی ٢٢ — چاپکردن](ch22-printing.md)*
