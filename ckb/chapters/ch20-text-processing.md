# بابەتی ٢٠ — پرۆسێسکردنی دەق
### Chapter 20 – Text Processing

---

فەرمانەکانی ئەم بابەتە:

- `cat` — پەیوەستکردن و چاپکردنی فایل
- `sort` — ریزکردنی هێڵەکانی دەق
- `uniq` — ڕەتکردنەوەی هێڵی دووبارە
- `cut` — سڕینەوەی بەشەکانی هێڵ
- `paste` — پەیوەستکردنی هێڵەکانی فایل
- `join` — یەکخستنی فایلەکانی دەق
- `comm` — بەراوردکردنی فایلەکانی دەق
- `diff` — بەراوردکردنی فایلەکانی دەق (فائق)
- `patch` — جێبەجێکردنی دیفی دەق
- `tr` — وەرگرتن و سڕینەوەی کاراکتەر
- `sed` — ئامرازی دەستکاریکردنی دەق
- `aspell` — پشکنەری رووماڵەی ئینتەراکتیف

---

## `cat` — بینین و پەیوەستکردن

```bash
cat file.txt
cat file1.txt file2.txt > combined.txt
cat -n file.txt        # لەگەڵ ژمارەی هێڵ
cat -s file.txt        # بەندکردنی هێڵە خاڵیی دووبارە
cat -A file.txt        # نیشاندانی کاراکتەرە نەبیستەکان
```

---

## `sort` — ریزکردن

```bash
sort file.txt                    # ریزکردنی پیتی
sort -r file.txt                 # پێچەوانە
sort -n file.txt                 # ژمارەیی
sort -k 2 file.txt               # بەپێی ستوونی ٢
sort -k 1,1 -k 2n file.txt       # بەپێی ستوونی ١ پیتی، دوا ژمارەیی
sort -t ':' -k 7 /etc/passwd     # بەپێی دیلیمیتەری دیاری
sort -u file.txt                 # ریزکردن و سڕینەوەی دووبارە
```

**نموونەی بەکارهێنان:**

```bash
du -sh /usr/share/* | sort -rh | head -10
```

دەخاتەوە گەورەترین ١٠ پووشپەری `/usr/share`.

---

## `uniq` — سڕینەوەی دووبارە

`uniq` هەمیشە لەدوای `sort` دێت:

```bash
sort file.txt | uniq             # سڕینەوەی دووبارە
sort file.txt | uniq -d          # تەنها هێڵی دووبارە
sort file.txt | uniq -u          # تەنها یەکتاکان
sort file.txt | uniq -c          # ژمارەی دووبارەبوون
```

---

## `cut` — قووڵکردنی ستوون

```bash
cut -f 1,3 file.txt             # ستوونی ١ و ٣ (tab-separated)
cut -d ':' -f 1 /etc/passwd     # یەکەم ستوون بە دیلیمیتەری :
cut -c 1-10 file.txt            # کاراکتەری ١ تا ١٠
```

```bash
# ناوی هەموو بەکارهێنەر
cut -d ':' -f 1 /etc/passwd | sort
```

---

## `paste` — پەیوەستکردنی ستوون

```bash
paste file1.txt file2.txt
paste -d ',' file1.txt file2.txt    # بە کاما
```

---

## `join` — یەکخستنی فایلەکان بەپێی کلیل

```bash
join file1.txt file2.txt
```

مانەندی `JOIN` ی SQL کاردەکات.

---

## `comm` — بەراوردکردنی فایلەکانی ریزکراو

```bash
comm file1.txt file2.txt
```

سێ ستوون دەدات: ١) تەنها لە فایلی ١، ٢) تەنها لە فایلی ٢، ٣) هەردووکیان.

```bash
comm -12 file1.txt file2.txt   # تەنها هاوبەش
```

---

## `diff` — بەراوردکردنی پێشکەوتوو

```bash
diff file1.txt file2.txt
diff -u file1.txt file2.txt    # فۆرماتی unified (باوتر)
```

```
--- file1.txt  2025-01-01
+++ file2.txt  2025-01-02
@@ -1,3 +1,4 @@
 line one
-line two
+line 2
+new line
 line three
```

---

## `patch` — جێبەجێکردنی دیف

```bash
diff -u original.txt changed.txt > changes.patch
patch < changes.patch
```

---

## `tr` — وەرگرتن و سڕینەوەی کاراکتەر

```bash
echo "Hello World" | tr 'a-z' 'A-Z'    # بچووک → گەورە
echo "Hello World" | tr -d 'l'         # سڕینەوەی 'l'
echo "Hello   World" | tr -s ' '       # بەندکردنی فاسیلەی دووبارە
```

---

## `sed` — دەستکاریکردنی پێشکەوتوو

### جێگرینەوە

```bash
sed 's/original/replacement/' file
sed 's/original/replacement/g' file     # گلۆبال
sed 's/original/replacement/gi' file    # گلۆبال، بەبێ ئاگاداری گەورە/بچووک
```

### ئامرازی چالاک

```bash
# سڕینەوەی هێڵی ١ تا ٥
sed '1,5d' file

# چاپکردنی هێڵی ١ تا ٥
sed -n '1,5p' file

# سڕینەوەی هێڵی خاڵی
sed '/^$/d' file

# زیادکردنی هێڵ بەسەر هەر هێڵێک
sed 's/.*/ &/' file

# جێبەجێکردنی چەند فەرمان
sed -e 's/foo/bar/' -e 's/baz/qux/' file
```

### بەکارهێنانی گروپ

```bash
echo "2025-06-09" | sed 's/\([0-9]*\)-\([0-9]*\)-\([0-9]*\)/\3\/\2\/\1/'
```

```
09/06/2025
```

---

## `aspell` — پشکنەری رووماڵە

```bash
aspell check file.txt        # پشکنینی ئینتەراکتیف
aspell list < file.txt       # فێرستی غەلەتەکان
```

---

## کورتە

ئامرازەکانی پرۆسێسکردنی دەق لە لینوکس زنجیرەیەکی بێ کۆتاین. تێکەڵکردنیان لە لووڵەی bash توانایی بێژماریان دەبەخشێت — بێ ئەوەی بۆ نووسینی کۆدی تایبەت پێویست بکات.

---

*بابەتی داهاتوو: [بابەتی ٢١ — فۆرماتکردنی ئاوتپوت](ch21-formatting-output.md)*
