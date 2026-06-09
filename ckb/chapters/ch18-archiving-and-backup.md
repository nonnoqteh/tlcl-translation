---
layout: default
---
# بابەتی ١٨ — ئەرشیڤ و پشتیوانی
### Chapter 18 – Archiving and Backup

---

فەرمانەکانی ئەم بابەتە:

- `gzip` — پرچکردن/کردنەوەی فایل
- `bzip2` — پرچکردن/کردنەوەی فایل (ڕێژەی باشتر)
- `tar` — ئەرشیڤکردنی فایل
- `zip` — بستەبەندی و پرچکردنی فایل
- `rsync` — هاوکاربوونی فایل و پووشپەر لە دووری

---

## پرچکردنی فایل

### `gzip` — پرچکەری پرکاربوو

```bash
gzip foo.txt
```

`foo.txt` پرچدەکات و `foo.txt.gz` دروستدەکات (ئەسل سڕدەبێتەوە).

بۆ کردنەوە:

```bash
gunzip foo.txt.gz
```

**ئۆپشنەکانی بەسوود:**

| ئۆپشن | کارکردن |
|-------|--------|
| `-c` | نووسین بۆ stdout (پارستنی ئەسل) |
| `-d` | کردنەوەی پرچکراو |
| `-l` | نیشاندانی ئامارەکانی پرچکردن |
| `-r` | بازگشتی |
| `-v` | تفصیلی |
| `-1` تا `-9` | ئاستی پرچکردن (١ = خێرا، ٩ = باشترین) |

```bash
gzip -v foo.txt
```

```
foo.txt:    73.4% -- replaced with foo.txt.gz
```

بینینی فایلی پرچکراو بەبێ کردنەوە:

```bash
gunzip -c foo.txt.gz | less
zcat foo.txt.gz | less
```

### `bzip2` — ڕێژەی پرچکردنی باشتر

```bash
bzip2 foo.txt           # پرچکردن → foo.txt.bz2
bunzip2 foo.txt.bz2     # کردنەوە
bzcat foo.txt.bz2       # بینین بەبێ کردنەوە
```

---

## ئەرشیڤکردنی فایل بە `tar`

`tar` ئامرازی کلاسیکی لینوکسە بۆ **ئەرشیڤکردن** — چەند فایل لە یەک فایلی تەکدا کۆدەکاتەوە.

**حاڵەتەکانی `tar`:**

| حاڵەت | مانا |
|-------|------|
| `c` | دروستکردنی ئەرشیڤ |
| `x` | دەرهێنانی ئەرشیڤ |
| `r` | زیادکردنی فایل بۆ ئەرشیڤ |
| `t` | فێرستی ناوەرۆک |

**ئۆپشنی باو:**

| ئۆپشن | مانا |
|-------|------|
| `f` | ناوی فایلی ئەرشیڤ |
| `v` | تفصیلی |
| `z` | پرچکردن بە gzip |
| `j` | پرچکردن بە bzip2 |
| `J` | پرچکردن بە xz |

### دروستکردنی ئەرشیڤ

```bash
tar cf archive.tar playground
tar cvf archive.tar playground     # بە تفصیل
```

### بینینی ناوەرۆک

```bash
tar tvf archive.tar | less
```

### دەرهێنانی ئەرشیڤ

```bash
tar xf archive.tar
tar xvf archive.tar

# لە پووشپەری دیاری
tar xf archive.tar -C /tmp
```

### ئەرشیڤی پرچکراو

```bash
# بە gzip
tar czf archive.tar.gz playground
tar xzf archive.tar.gz

# بە bzip2
tar cjf archive.tar.bz2 playground
tar xjf archive.tar.bz2
```

---

## `zip` — گونجاندن لەگەڵ ویندۆز

```bash
zip -r backup.zip playground    # دروستکردن
unzip backup.zip                # کردنەوە
unzip -l backup.zip             # فێرستی ناوەرۆک
```

---

## `rsync` — هاوکاربوونی پێشکەوتوو

`rsync` هێزداترین ئامرازی پشتیوانیی لینوکسە. تەنها فایلی **گۆڕدراو** کۆپیدەکات.

```bash
rsync options source destination
```

**هاوکاربوونی ناوخۆ:**

```bash
rsync -av playground foo
```

جارێکی دووەم:

```bash
rsync -av playground foo
```

```
sent 57,643 bytes  received 1,592 bytes
total size is 3,344,380  speedup is 56.55
```

هیچ فایلێک کۆپی نەبوو چونکە هەموو یەکسانن.

**ئۆپشنی گرنگی `rsync`:**

| ئۆپشن | مانا |
|-------|------|
| `-a` | حاڵەتی ئەرشیڤ (پارستنی مۆڵەت، مالک، کات) |
| `-v` | تفصیلی |
| `-n` | تاقیکردنەوە — نیشان دەدات چی ڕوودەدات |
| `--delete` | سڕینەوەی فایلی نەبوون لە سەرچاوە |
| `--progress` | نیشاندانی پێشکەوتن |

### هاوکاربوون لەگەڵ سیستەمی دوور

```bash
rsync -av source/ user@remote-sys:backup/
rsync -av user@remote-sys:backup/ destination/
```

### پشتیوانیی دەورەیی

```bash
rsync -av --delete /home /media/backup-drive/
```

---

## کورتە

`tar` بۆ ئەرشیڤکردنی کلاسیک، `gzip`/`bzip2` بۆ پرچکردن، و `rsync` بۆ پشتیوانیی زیرەک. تێکەڵکردنیان لەگەڵ `cron` سیستەمی پشتیوانیی خۆکاری تەواو دروستدەکات.

---

*بابەتی داهاتوو: [بابەتی ١٩ — دەقی ئایەتی](ch19-regular-expressions)*
