---
layout: default
---
# فصل ۱۸ — آرشیو و پشتیبان‌گیری
### Chapter 18 – Archiving and Backup

---

دستورات این فصل:

- `gzip` — فشرده‌سازی/باز کردن فایل
- `bzip2` — فشرده‌سازی/باز کردن فایل (نسبت بهتر)
- `tar` — آرشیو فایل
- `zip` — بسته‌بندی و فشرده‌سازی فایل
- `rsync` — همگام‌سازی فایل و پوشه از راه دور

---

## فشرده‌سازی فایل
### Compressing Files

### `gzip` — فشرده‌ساز پرکاربرد

```bash
gzip foo.txt
```

فایل `foo.txt` را فشرده می‌کند و `foo.txt.gz` می‌سازد (اصلی حذف می‌شود).

```bash
ls -l foo.txt.gz
```

برای باز کردن:

```bash
gunzip foo.txt.gz
```

یا:

```bash
gzip -d foo.txt.gz
```

**گزینه‌های مفید:**

| گزینه | عملکرد |
|-------|--------|
| `-c` | نوشتن به stdout (حفظ فایل اصلی) |
| `-d` | باز کردن فشرده |
| `-f` | اجبار (force) |
| `-l` | نمایش آمار فشرده‌سازی |
| `-r` | بازگشتی (recursive) |
| `-v` | نمایش پیام‌های تفصیلی |
| `-1` تا `-9` | سطح فشرده‌سازی (۱ = سریع، ۹ = بهترین) |

```bash
gzip -v foo.txt
```

```
foo.txt:    73.4% -- replaced with foo.txt.gz
```

مشاهده‌ی فایل فشرده بدون باز کردن:

```bash
gunzip -c foo.txt.gz | less
```

یا با `zcat`:

```bash
zcat foo.txt.gz | less
```

### `bzip2` — فشرده‌ساز با نسبت بهتر

`bzip2` فشرده‌سازی بهتری دارد اما کندتر است:

```bash
bzip2 foo.txt          # فشرده‌سازی → foo.txt.bz2
bunzip2 foo.txt.bz2    # باز کردن
bzcat foo.txt.bz2      # مشاهده بدون باز کردن
```

---

## آرشیو فایل با `tar`
### Archiving Files with tar

`tar` ابزار کلاسیک لینوکس برای **آرشیو** (Tape ARchive) است — چند فایل را در یک فایل واحد جمع می‌کند.

**حالت‌های `tar`:**

| حالت | معنی |
|------|------|
| `c` | ساختن آرشیو |
| `x` | استخراج آرشیو |
| `r` | افزودن فایل به آرشیو |
| `t` | فهرست محتوا |

**گزینه‌های رایج:**

| گزینه | معنی |
|-------|------|
| `f` | نام فایل آرشیو |
| `v` | نمایش تفصیلی |
| `z` | فشرده‌سازی با gzip |
| `j` | فشرده‌سازی با bzip2 |
| `J` | فشرده‌سازی با xz |

### ساختن آرشیو

```bash
tar cf archive.tar playground
```

```bash
tar cvf archive.tar playground    # با نمایش تفصیلی
```

### مشاهده‌ی محتوا

```bash
tar tvf archive.tar | less
```

### استخراج آرشیو

```bash
tar xf archive.tar
```

با نمایش تفصیلی:

```bash
tar xvf archive.tar
```

در پوشه‌ی خاص:

```bash
tar xf archive.tar -C /tmp
```

استخراج یک فایل خاص:

```bash
tar xf archive.tar --wildcards 'home/me/playground/dir-*/file-A'
```

### آرشیو فشرده

```bash
# با gzip
tar czf archive.tar.gz playground
tar xzf archive.tar.gz

# با bzip2
tar cjf archive.tar.bz2 playground
tar xjf archive.tar.bz2
```

### `tar` با `find`

ترکیب قدرتمند برای پشتیبان‌گیری از فایل‌های تغییریافته:

```bash
find playground -name 'reg-ex[0-9]' -newer playground/timestamp | tar cf - --files-from=- | gzip > backup.tar.gz
```

---

## `zip` — سازگاری با ویندوز

`zip` هم آرشیو و هم فشرده‌سازی می‌کند — برای سازگاری با ویندوز:

```bash
zip -r backup.zip playground    # ساختن
unzip backup.zip                # باز کردن
unzip -l backup.zip             # فهرست محتوا
```

---

## `rsync` — همگام‌سازی پیشرفته
### Synchronizing Files and Directories with rsync

`rsync` قدرتمندترین ابزار پشتیبان‌گیری لینوکس است. تنها فایل‌های **تغییریافته** را کپی می‌کند.

```bash
rsync options source destination
```

**همگام‌سازی محلی:**

```bash
rsync -av playground foo
```

دفعه‌ی دوم که اجرا می‌کنیم:

```bash
rsync -av playground foo
```

```
sending incremental file list
sent 57,643 bytes  received 1,592 bytes  118,470.00 bytes/sec
total size is 3,344,380  speedup is 56.55
```

هیچ فایلی کپی نشد چون همه یکسانند.

**گزینه‌های مهم `rsync`:**

| گزینه | معنی |
|-------|------|
| `-a` | حالت آرشیو (حفظ مجوزها، مالک، زمان، لینک‌ها) |
| `-v` | نمایش تفصیلی |
| `-r` | بازگشتی |
| `-n` | آزمایشی — نشان می‌دهد چه اتفاقی می‌افتد |
| `--delete` | حذف فایل‌هایی که در مبدأ نیستند |
| `--exclude` | استثنا کردن فایل‌ها |
| `--progress` | نمایش پیشرفت |

### همگام‌سازی با سیستم راه دور

```bash
rsync -av source/ user@remote-sys:backup/
```

```bash
rsync -av user@remote-sys:backup/ destination/
```

### پشتیبان‌گیری دوره‌ای

```bash
rsync -av --delete /home /media/backup-drive/
```

> `--delete` فایل‌هایی که از مبدأ حذف شده‌اند را از مقصد هم حذف می‌کند — آینه‌ای واقعی از مبدأ.

---

## خلاصه

در این فصل ابزارهای آرشیو و پشتیبان‌گیری را دیدیم. `tar` برای آرشیو کلاسیک، `gzip`/`bzip2` برای فشرده‌سازی، و `rsync` برای پشتیبان‌گیری هوشمند. ترکیب اینها با `cron` یک سیستم پشتیبان‌گیری خودکار کامل می‌سازد.

---

*فصل بعدی: [فصل ۱۹ — عبارات باقاعده](ch19-regular-expressions)*
