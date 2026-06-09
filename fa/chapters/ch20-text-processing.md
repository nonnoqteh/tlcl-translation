# فصل ۲۰ — پردازش متن
### Chapter 20 – Text Processing

---

لینوکس محیط عالی برای پردازش متن است. این فصل ابزارهای کلاسیک پردازش متن را معرفی می‌کند:

- `cat` — اتصال فایل‌ها (با گزینه‌های بیشتر)
- `sort` — مرتب‌سازی خطوط متن
- `uniq` — گزارش یا حذف خطوط تکراری
- `cut` — حذف بخش‌هایی از هر خط فایل
- `paste` — ادغام خطوط فایل‌ها
- `join` — پیوستن خطوط دو فایل بر اساس فیلد مشترک
- `comm` — مقایسه‌ی دو فایل مرتب‌شده خط به خط
- `diff` — مقایسه‌ی فایل‌ها خط به خط
- `patch` — اعمال تفاوت به فایل اصلی
- `tr` — جابجایی یا حذف کاراکترها
- `sed` — ویرایشگر جریانی
- `awk` — زبان برنامه‌نویسی برای اسکن/پردازش الگو

---

## `cat` — گزینه‌های بیشتر

```bash
cat -A foo.txt    # نمایش کاراکترهای خاص ($، ^I)
cat -n foo.txt    # شماره‌گذاری خطوط
cat -s foo.txt    # فشرده‌سازی خطوط خالی متوالی
```

---

## `sort` — مرتب‌سازی

```bash
sort file.txt
```

```bash
sort -r file.txt        # ترتیب معکوس
sort -n file.txt        # مرتب‌سازی عددی
sort -k 2 file.txt      # مرتب‌سازی بر اساس فیلد ۲
sort -t ':' -k 7 /etc/passwd   # با جداکننده‌ی ':'
sort -u file.txt        # مرتب‌سازی + حذف تکراری‌ها
```

**مثال عملی** — بزرگ‌ترین پوشه‌ها:

```bash
du -sh /* 2>/dev/null | sort -rh | head -5
```

```
4.0G  /usr
1.1G  /var
336M  /lib
...
```

---

## `uniq` — حذف یا شمارش تکراری‌ها

```bash
sort file.txt | uniq          # حذف تکراری‌ها
sort file.txt | uniq -d       # فقط خطوط تکراری
sort file.txt | uniq -c       # شمارش هر خط
sort file.txt | uniq -c | sort -rn  # بیشترین تکرار اول
```

---

## `cut` — استخراج ستون

```bash
cut -f 1 file.txt           # فیلد ۱ (جداکننده: Tab)
cut -f 1,3 file.txt         # فیلدهای ۱ و ۳
cut -d ':' -f 1 /etc/passwd  # جداکننده‌ی ':'
cut -c 1-10 file.txt         # کاراکترهای ۱ تا ۱۰
```

**مثال — استخراج نام کاربران:**

```bash
cut -d ':' -f 1 /etc/passwd | sort | head
```

```
avahi
bin
daemon
...
```

---

## `paste` — ادغام فایل‌ها

برعکس `cut`، `paste` فایل‌ها را ستون به ستون ادغام می‌کند:

```bash
paste file1.txt file2.txt
```

---

## `comm` — مقایسه‌ی دو فایل

```bash
comm file1.txt file2.txt
```

خروجی سه ستون:
- ستون ۱: فقط در file1
- ستون ۲: فقط در file2
- ستون ۳: در هر دو

```bash
comm -12 file1.txt file2.txt    # فقط خطوط مشترک
```

---

## `diff` — مقایسه‌ی تفاوت

```bash
diff file1.txt file2.txt
```

```
1c1
< aaaa
---
> bbbb
```

فرمت یونیفاید (رایج‌تر):

```bash
diff -u file1.txt file2.txt
```

```
--- file1.txt   2025-01-01
+++ file2.txt   2025-01-01
@@ -1,4 +1,4 @@
-aaaa
+bbbb
 bbbb
```

---

## `patch` — اعمال تغییرات

```bash
diff -Naur old_file new_file > patchfile
patch < patchfile
```

---

## `tr` — جابجایی کاراکتر

```bash
echo "lowercase letters" | tr a-z A-Z
```

```
LOWERCASE LETTERS
```

```bash
echo "hello world" | tr ' ' '_'
```

```
hello_world
```

حذف کاراکترها:

```bash
echo "hello world" | tr -d ' '
```

```
helloworld
```

فشرده‌سازی تکراری‌ها:

```bash
echo "aaabbbccc" | tr -s 'abc'
```

```
abc
```

---

## `sed` — ویرایشگر جریانی
### Stream Editor

`sed` برنامه‌ای است که برای ویرایش جریان متن طراحی شده:

```bash
sed 's/old/new/' file.txt        # جایگزینی اولین مورد هر خط
sed 's/old/new/g' file.txt       # جایگزینی همه موارد
sed 's/old/new/2' file.txt       # جایگزینی مورد دوم
sed -i 's/old/new/g' file.txt    # ویرایش درجا (تغییر فایل)
sed 's/old/new/gi' file.txt      # بدون توجه به بزرگی/کوچکی
```

**حذف خطوط:**

```bash
sed '1,3d' file.txt              # حذف خطوط ۱ تا ۳
sed '/pattern/d' file.txt        # حذف خطوط منطبق
```

**چاپ خطوط:**

```bash
sed -n '5,10p' file.txt          # چاپ خطوط ۵ تا ۱۰
sed -n '/pattern/p' file.txt     # چاپ خطوط منطبق
```

---

## `awk` — پردازش داده‌ی جدولی

`awk` برای پردازش داده‌های با ساختار جدولی (مثل فهرست‌ها):

```bash
ls -l | awk '{print $1, $9}'    # ستون ۱ و ۹
```

```
-rw-r--r-- README.md
-rwxr-xr-x script.sh
...
```

**مجموع اندازه‌ی فایل‌ها:**

```bash
ls -l | awk '{total += $5} END {print total}'
```

**با شرط:**

```bash
ls -l | awk '$5 > 10000 {print $9}'   # فایل‌های بزرگ‌تر از ۱۰KB
```

**جداکننده‌ی سفارشی:**

```bash
awk -F ':' '{print $1}' /etc/passwd   # نام کاربران
```

---

## خلاصه

این فصل ابزارهای پردازش متن کلاسیک لینوکس را معرفی کرد. ترکیب آن‌ها با لوله‌کشی تقریباً هر تبدیل متنی را ممکن می‌کند. `sed` و `awk` به‌تنهایی کتاب‌های کاملی دارند — امروز مبانی را آموختیم.

---

## مطالعه‌ی بیشتر

- صفحه‌ی `man sed`، `man awk`، `man tr`
- کتاب Sed & Awk توسط O'Reilly
- آموزش awk: [GNU awk User's Guide](https://www.gnu.org/software/gawk/manual/)

---

*فصل بعدی: [فصل ۲۱ — قالب‌بندی خروجی](ch21-formatting-output.md)*
