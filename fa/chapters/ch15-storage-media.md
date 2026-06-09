# فصل ۱۵ — رسانه‌ی ذخیره‌سازی
### Chapter 15 – Storage Media

---

در این فصل با دستوراتی آشنا می‌شویم که برای مدیریت دستگاه‌های ذخیره‌سازی استفاده می‌شوند:

- `mount` — سوار کردن سیستم فایل
- `umount` — پیاده کردن سیستم فایل
- `fsck` — بررسی و ترمیم سیستم فایل
- `fdisk` — دستکاری جدول پارتیشن
- `mkfs` — ساختن سیستم فایل
- `dd` — تبدیل و کپی فایل
- `genisoimage` / `mkisofs` — ساختن فایل تصویر ISO 9660
- `wodim` / `cdrecord` — نوشتن روی رسانه‌ی نوری
- `md5sum` — محاسبه‌ی چک‌سام MD5

---

## مدیریت دستگاه‌های ذخیره‌سازی
### Managing Storage Devices

### سوار کردن و پیاده کردن
#### Mounting and Unmounting

در لینوکس، دستگاه‌های ذخیره‌سازی به درخت سیستم فایل **سوار** (mount) می‌شوند.

برای مشاهده‌ی دستگاه‌های سوارشده:

```bash
mount
```

```
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
/dev/sda1 on / type ext4 (rw,relatime)
/dev/sdb1 on /media/disk type vfat (rw,nosuid,nodev)
```

#### سوار کردن فلش USB یا CD

```bash
mount /dev/sdb1 /mnt/usb
```

برای ایجاد نقطه‌ی سوارکردن:

```bash
mkdir /mnt/usb
mount /dev/sdb1 /mnt/usb
ls /mnt/usb
```

#### پیاده کردن

```bash
umount /mnt/usb
```

یا:

```bash
umount /dev/sdb1
```

> **مهم:** قبل از جدا کردن دستگاه، همیشه آن را پیاده کنید تا داده‌ها آسیب نبینند.

---

## بررسی سیستم فایل
### Checking and Repairing File Systems

دستور `fsck` برای بررسی و ترمیم سیستم فایل:

```bash
sudo fsck /dev/sdb1
```

> **هشدار:** هرگز `fsck` را روی سیستم فایل سوارشده اجرا نکنید!

---

## پارتیشن‌بندی
### Partitioning a Disk

`fdisk` ابزار پارتیشن‌بندی تعاملی است:

```bash
sudo fdisk /dev/sdb
```

دستورات مهم در `fdisk`:

| کلید | عملکرد |
|------|--------|
| `p` | نمایش جدول پارتیشن |
| `n` | ساختن پارتیشن جدید |
| `d` | حذف پارتیشن |
| `t` | تغییر نوع پارتیشن |
| `w` | نوشتن تغییرات و خروج |
| `q` | خروج بدون ذخیره |
| `m` | نمایش راهنما |

---

## ساختن سیستم فایل جدید
### Creating a New File System

بعد از پارتیشن‌بندی، باید سیستم فایل بسازیم:

```bash
# ساختن سیستم فایل ext4
sudo mkfs -t ext4 /dev/sdb1

# ساختن FAT32 (برای سازگاری با ویندوز)
sudo mkfs -t vfat /dev/sdb1
```

---

## دستور `dd` — کپی سطح پایین
### Moving Data Directly to and from Devices

`dd` بلاک به بلاک داده کپی می‌کند — برای پشتیبان‌گیری از پارتیشن یا ساختن فلش بوتابل:

```bash
dd if=input_file of=output_file [bs=block_size] [count=blocks]
```

| پارامتر | معنی |
|---------|------|
| `if=` | فایل ورودی (input file) |
| `of=` | فایل خروجی (output file) |
| `bs=` | اندازه‌ی بلاک |
| `count=` | تعداد بلاک‌ها |

**مثال‌ها:**

```bash
# کپی کامل یک درایو به درایو دیگر
dd if=/dev/sda of=/dev/sdb

# ساختن فایل تصویر از فلش USB
dd if=/dev/sdb of=usb_backup.img

# نوشتن فایل ISO روی فلش USB
dd if=ubuntu.iso of=/dev/sdb bs=4M status=progress
```

> ⚠️ **احتیاط:** `dd` بدون هشدار داده‌ها را بازنویسی می‌کند. مطمئن شوید `if` و `of` را درست تایپ کرده‌اید!

---

## کار با CD/DVD

### ساختن فایل ISO از CD

```bash
dd if=/dev/cdrom of=disc_image.iso
```

### ساختن فایل ISO از فایل‌ها

```bash
genisoimage -o cd-rom.iso -R -J ~/cd-rom-files/
```

### سوزاندن روی CD/DVD

```bash
wodim dev=/dev/cdrw image.iso
```

### تأیید صحت با MD5

```bash
md5sum image.iso
```

```
34e354760f9bb7fbf85c96f6a3f94eda  image.iso
```

---

## فایل `/etc/fstab`
### `/etc/fstab` — Table of File Systems

این فایل دستگاه‌هایی را تعریف می‌کند که هنگام بوت به‌طور خودکار سوار می‌شوند:

```bash
less /etc/fstab
```

```
UUID=fcba...  /        ext4  defaults  1 1
UUID=9a8b...  /boot    ext4  defaults  1 2
UUID=3c4d...  /home    ext4  defaults  1 2
```

---

## خلاصه

در این فصل مدیریت دستگاه‌های ذخیره‌سازی در لینوکس را دیدیم — از سوار/پیاده کردن تا پارتیشن‌بندی، ساختن سیستم فایل و کار با رسانه‌ی نوری. این ابزارها برای هر مدیر سیستمی ضروری هستند.

---

*فصل بعدی: [فصل ۱۶ — شبکه](ch16-networking.md)*
