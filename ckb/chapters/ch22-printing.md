---
layout: default
---
# بابەتی ٢٢ — چاپکردن
### Chapter 22 – Printing

---

فەرمانەکانی ئەم بابەتە:

- `pr` — ئامادەکردنی دەق بۆ چاپ
- `lpr` — چاپکردن
- `lp` — چاپکردن (System V)
- `a2ps` — گواستنەوەی هەر شتێک بۆ PostScript
- `lpstat` — نیشاندانی حاڵەتی چاپگەر
- `lpq` — نیشاندانی ڕیزی چاپ
- `lprm` / `cancel` — سڕینەوەی کاری چاپ

---

## مێژووی کورتی چاپکردن لە لینوکس

لینوکس سیستەمی چاپکردنی خۆی هەیە کە **CUPS** (Common Unix Printing System) ناودەبرێت. CUPS دریڤەری چاپگەر و ئامرازی بەڕێوەبردنی چاپ دابین دەکات.

---

## چاپکردن لە کۆمەڵەی فەرمان

### `lpr` — چاپکردنی ساده

```bash
lpr file.txt
lpr -P printer_name file.txt     # چاپگەری دیاری
lpr -# 3 file.txt                # ٣ کۆپی
lpr -P HP_Printer -# 2 report.txt
```

### `lp` — ئامرازی System V

```bash
lp file.txt
lp -d printer_name file.txt      # چاپگەری دیاری
lp -n 3 file.txt                 # ٣ کۆپی
```

---

## بینینی حاڵەت و بەڕێوەبردنی ڕیز

### `lpstat` — حاڵەتی چاپگەر

```bash
lpstat -a            # نیشاندانی چاپگەرە بەردەستەکان
lpstat -s            # وضعیتی گشتی سیستەم
lpstat -p            # فێرستی چاپگەرەکان
```

### `lpq` — ڕیزی چاپ

```bash
lpq              # ڕیزی بنچینەیی چاپگەر
lpq -P printer   # ڕیزی چاپگەری دیاری
```

```
Rank    Owner   Job     File(s)            Total Size
1st     me      123     report.txt         2048 bytes
```

### سڕینەوەی کاری چاپ

```bash
lprm 123                     # بەپێی ژمارەی کار
cancel 123                   # ئامرازی System V
lprm -                       # هەموو کاری خۆت
```

---

## `a2ps` — ئامادەکردنی پێشکەوتوو بۆ چاپ

`a2ps` هەر فایلی دەقێک بۆ PostScript دەگوازێتەوە و تایبەتمەندییەکانی جوان هەیە:

```bash
a2ps file.txt                        # چاپکردن بە مودی بنچینەیی
a2ps -2 file.txt                     # دوو صفحە لەسەر یەک پەڕ
a2ps --columns=2 file.txt            # دوو ستوون
a2ps -P printer_name file.txt        # چاپگەری دیاری
a2ps --pretty-print file.txt         # رەنگاوکردنی کۆد
a2ps --output=output.ps file.txt     # پاراستن بۆ فایل
```

**دروستکردنی PDF:**

```bash
a2ps file.txt --output=- | ps2pdf - output.pdf
```

---

## کار لەگەڵ PostScript و PDF

### گواستنەوەی PDF بۆ PS

```bash
pdf2ps document.pdf document.ps
```

### گواستنەوەی PS بۆ PDF

```bash
ps2pdf document.ps document.pdf
```

### بینینی پۆستسکریپت

```bash
gs document.ps    # Ghostscript
```

---

## ڕێکخستنی چاپ بە CUPS

### ڕووکاری وێبی CUPS

```
http://localhost:631
```

لەم ئادرەسەوە دەتوانی:
- چاپگەر زیاد بکەی
- کارەکانی چاپ بەڕێوە ببەی
- ڕێکخستنی چاپگەر گۆڕ بکەی

### فەرمانی `lpadmin`

```bash
sudo lpadmin -p MyPrinter -E -v socket://192.168.1.100 -m everywhere
```

---

## کورتە

لینوکس دەستگەیشتنی تەواو بۆ چاپکردن دابین دەکات — لە فەرمانی سادەی `lpr` تا بەڕێوەبردنی پێشکەوتوو بە CUPS. `a2ps` هەر فایلی دەقێک جوان دەکاتەوە بۆ چاپ.

---

*بابەتی داهاتوو: [بابەتی ٢٣ — کۆمپایڵکردنی بەرنامەکان](ch23-compiling-programs)*
