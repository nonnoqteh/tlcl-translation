---
layout: default
---
# بابەتی ١٤ — بەڕێوەبردنی پاکێج
### Chapter 14 – Package Management

---

ئەگەر یەک شت لە تەواوی مێژووی لینوکس نەبوو، ئەوە نصبکردنی نرمەواڵە بوو! لە کاتی کۆندا ئەم پرۆسەیە سەخت و ئازاردەر بوو. ئەمڕۆ بە **سیستەمی بەڕێوەبردنی پاکێج** (package management systems) هەموو شت زۆر ئاسان بوو.

فەرمانەکانی ئەم بابەتە:

**توزیعی بەپێی Debian (Ubuntu, Mint):**
- `apt` — ئامرازی بەڕێوەبردنی پاکێجی ئاستی بەرز
- `dpkg` — ئامرازی بەڕێوەبردنی پاکێجی ئاستی نزم

**توزیعی بەپێی Red Hat (Fedora, CentOS):**
- `dnf` / `yum` — ئامرازی بەڕێوەبردنی پاکێجی ئاستی بەرز
- `rpm` — ئامرازی بەڕێوەبردنی پاکێجی ئاستی نزم

---

## سیستەمی پاکێج چییە؟

زۆری توزیعەکانی لینوکس سیستەمی بەڕێوەبردنی پاکێجیان هەیە کە:
- نرمەواڵە بە فۆرماتی **پاکێج** دابەشدەکات
- **وابەستەکان** (dependencies) بە خۆکاری چارەسەردەکات
- نصبکردن، نوێکردنەوە و سڕینەوەی نرمەواڵە بەڕێدەبات

دوو خێزانی سەرەکی سیستەمی پاکێج:

| سیستەم | توزیعەکان | ئامرازی ئاستی نزم | ئامرازی ئاستی بەرز |
|--------|----------|-------------------|-------------------|
| Debian | Debian, Ubuntu, Mint | `dpkg` | `apt` |
| Red Hat | Fedora, CentOS, RHEL | `rpm` | `dnf`/`yum` |

---

## کارەکانی باوی بەڕێوەبردنی پاکێج

### گەڕان بۆ پاکێج

```bash
# Debian/Ubuntu
apt search search_string

# Fedora/CentOS
dnf search search_string
```

### نصبکردنی پاکێج

```bash
# Debian/Ubuntu
apt install package_name

# Fedora/CentOS
dnf install package_name
```

### سڕینەوەی پاکێج

```bash
# Debian/Ubuntu
apt remove package_name

# Fedora/CentOS
dnf erase package_name
```

### نوێکردنەوەی هەموو پاکێجەکان

```bash
# Debian/Ubuntu
apt update        # نوێکردنەوەی فێرستی پاکێجەکان
apt upgrade       # بەرزکردنەوەی پاکێجە نصبکراوەکان

# Fedora/CentOS
dnf update
```

### فێرستی پاکێجە نصبکراوەکان

```bash
# Debian/Ubuntu
dpkg -l

# Fedora/CentOS
rpm -qa
```

### دیاریکردنی کام پاکێج فایلێک نصبکردووە

```bash
# Debian/Ubuntu
dpkg -S file_name

# Fedora/CentOS
rpm -qf file_name
```

نموونە:

```bash
dpkg -S /usr/bin/vim
```

```
vim: /usr/bin/vim
```

### نیشاندانی زانیاری پاکێجێکی نصبکراو

```bash
# Debian/Ubuntu
apt show package_name

# Fedora/CentOS
dnf info package_name
```

### نصبکردن لە فایلی پاکێج

```bash
# Debian/Ubuntu (.deb)
dpkg -i package_file.deb

# Fedora/CentOS (.rpm)
rpm -i package_file.rpm
```

---

## بەڕێوەبردنی مەخزەنەکان

**مەخزەن** (repository) شوێنێکە کە پاکێجەکان تێدا پارێزراون.

### زیادکردنی مەخزەن (Ubuntu)

```bash
sudo add-apt-repository ppa:owner/ppa-name
sudo apt update
```

فایلی ڕێکخستنی مەخزەن لە:
- Debian/Ubuntu: `/etc/apt/sources.list`
- Fedora/CentOS: `/etc/yum.repos.d/`

---

## کورتە

سیستەمی بەڕێوەبردنی پاکێج یەکێک لە باشترین تایبەتمەندییەکانی لینوکسە. نەتەنها نرمەواڵە بە ئاسانی نصبدەکات، بەڵکو وابەستەکانیش چارەسەردەکات، پاکێجەکان نوێ دەخاتەوە و یەکپارچایی سیستەم دەپارێزێت.

---

*بابەتی داهاتوو: [بابەتی ١٥ — میدیای ستۆرکردن](ch15-storage-media.md)*
