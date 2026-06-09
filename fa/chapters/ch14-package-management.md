---
layout: default
---
# فصل ۱۴ — مدیریت بسته
### Chapter 14 – Package Management

---

اگر یک نفر در تمام تاریخ لینوکس یک چیز نبود، آن نصب نرم‌افزار بود! در گذشته این فرآیند سخت و دشوار بود. امروز با **سیستم‌های مدیریت بسته** (package management systems) همه چیز بسیار ساده شده.

دستورات این فصل (بسته به توزیع):

**توزیع‌های مبتنی بر Debian (Ubuntu, Mint):**
- `apt` — ابزار مدیریت بسته‌ی سطح بالا
- `dpkg` — ابزار مدیریت بسته‌ی سطح پایین

**توزیع‌های مبتنی بر Red Hat (Fedora, CentOS):**
- `dnf` / `yum` — ابزار مدیریت بسته‌ی سطح بالا
- `rpm` — ابزار مدیریت بسته‌ی سطح پایین

---

## سیستم بسته چیست؟
### Packaging Systems

اکثر توزیع‌های لینوکس سیستم مدیریت بسته دارند که:
- نرم‌افزار را در قالب **بسته** (package) توزیع می‌کنند
- وابستگی‌ها (dependencies) را به‌طور خودکار حل می‌کنند
- نصب، به‌روزرسانی و حذف نرم‌افزار را مدیریت می‌کنند
- یکپارچگی سیستم را حفظ می‌کنند

دو خانواده‌ی اصلی سیستم بسته:

| سیستم | توزیع‌ها | ابزار سطح پایین | ابزار سطح بالا |
|-------|---------|----------------|----------------|
| Debian | Debian, Ubuntu, Mint | `dpkg` | `apt` |
| Red Hat | Fedora, CentOS, RHEL | `rpm` | `dnf`/`yum` |

---

## عملیات رایج بسته
### Common Package Management Tasks

### جستجوی بسته

```bash
# Debian/Ubuntu
apt search search_string

# Fedora/CentOS
dnf search search_string
```

### نصب بسته

```bash
# Debian/Ubuntu
apt install package_name

# Fedora/CentOS
dnf install package_name
```

### حذف بسته

```bash
# Debian/Ubuntu
apt remove package_name

# Fedora/CentOS
dnf erase package_name
```

### به‌روزرسانی همه‌ی بسته‌ها

```bash
# Debian/Ubuntu
apt update        # به‌روزرسانی فهرست بسته‌ها
apt upgrade       # ارتقای بسته‌های نصب‌شده

# Fedora/CentOS
dnf update
```

### فهرست بسته‌های نصب‌شده

```bash
# Debian/Ubuntu
dpkg -l

# Fedora/CentOS
rpm -qa
```

### تعیین اینکه کدام بسته یک فایل را نصب کرده

```bash
# Debian/Ubuntu
dpkg -S file_name

# Fedora/CentOS
rpm -qf file_name
```

مثال:

```bash
dpkg -S /usr/bin/vim
```

```
vim: /usr/bin/vim
```

### نمایش اطلاعات یک بسته‌ی نصب‌شده

```bash
# Debian/Ubuntu
apt show package_name

# Fedora/CentOS
dnf info package_name
```

### نصب از فایل بسته

گاهی بسته‌ای را مستقیم دانلود می‌کنیم و می‌خواهیم نصب کنیم:

```bash
# Debian/Ubuntu (.deb)
dpkg -i package_file.deb

# Fedora/CentOS (.rpm)
rpm -i package_file.rpm
```

---

## مدیریت مخازن
### Working with Repositories

**مخزن** (repository) جایی است که بسته‌ها در آن ذخیره می‌شوند. توزیع‌ها معمولاً مخازن رسمی و شخص‌ثالث دارند.

### افزودن مخزن (Ubuntu)

```bash
# افزودن PPA (Personal Package Archive)
sudo add-apt-repository ppa:owner/ppa-name
sudo apt update
```

فایل‌های پیکربندی مخزن در:
- Debian/Ubuntu: `/etc/apt/sources.list` و `/etc/apt/sources.list.d/`
- Fedora/CentOS: `/etc/yum.repos.d/`

---

## خلاصه

سیستم مدیریت بسته یکی از برترین امکانات لینوکس است. نه تنها نرم‌افزار را به‌آسانی نصب می‌کند، بلکه وابستگی‌ها را حل می‌کند، بسته‌ها را به‌روز نگه می‌دارد و یکپارچگی سیستم را تضمین می‌کند.

---

## مطالعه‌ی بیشتر

- مستندات Ubuntu: [Ubuntu Package Management](https://help.ubuntu.com/community/AptGet/Howto)
- مستندات Fedora: [DNF Documentation](https://dnf.readthedocs.io/)

---

*فصل بعدی: [فصل ۱۵ — رسانه‌ی ذخیره‌سازی](ch15-storage-media.md)*
