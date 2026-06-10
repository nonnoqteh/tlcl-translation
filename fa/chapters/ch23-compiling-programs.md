---
layout: default
---
# فصل ۲۳ — کامپایل کردن برنامه‌ها
### Chapter 23 – Compiling Programs

---

در این فصل یاد می‌گیریم چطور برنامه‌ها را از **کد منبع** (source code) کامپایل کنیم. دستور اصلی:

- `make` — ابزار نگهداری برنامه

---

## چرا کد منبع را کامپایل کنیم؟

اگر مدیریت بسته‌ها در لینوکس این‌قدر خوب است، چرا باید کد منبع کامپایل کنیم؟

- **برنامه‌ای وجود دارد که در مخازن توزیعت نیست**
- **نسخه‌ی جدیدتری می‌خواهی** که هنوز بسته‌بندی نشده
- **می‌خواهی رفتار برنامه را سفارشی کنی** از طریق گزینه‌های کامپایل
- **یاد می‌گیری** برنامه‌ها چطور کار می‌کنند

---

## کامپایلر چیست؟

**کامپایلر** (compiler) برنامه‌ای است که کد نوشته‌شده به زبان برنامه‌نویسی (مثل C) را به **کد ماشین** (زبانی که پردازنده می‌فهمد) تبدیل می‌کند.

لینوکس از **GCC** (GNU Compiler Collection) استفاده می‌کند.

برای بررسی نصب بودن gcc:

```bash
which gcc
```

```
/usr/bin/gcc
```

اگر نصب نبود:

```bash
# Ubuntu/Debian
sudo apt install build-essential

# Fedora/CentOS
sudo dnf groupinstall "Development Tools"
```

---

## کامپایل کردن یک برنامه‌ی ساده

یک فایل C ساده بسازیم:

```bash
nano hello_world.c
```

```c
#include <stdio.h>

int main(void) {
    printf("Hello World!\n");
    return 0;
}
```

کامپایل:

```bash
gcc -o hello_world hello_world.c
```

اجرا:

```bash
./hello_world
```

```
Hello World!
```

---

## فرآیند معمول — کامپایل از کد منبع

اکثر برنامه‌های متن‌باز از این فرآیند استاندارد پیروی می‌کنند:

### ۱. دریافت کد منبع

```bash
wget https://ftp.gnu.org/gnu/diction/diction-1.11.tar.gz
```

### ۲. باز کردن فایل فشرده

```bash
tar xzf diction-1.11.tar.gz
ls
```

```
diction-1.11  diction-1.11.tar.gz
```

```bash
cd diction-1.11
ls
```

```
config.guess  COPYING  diction.c  getopt.c  Makefile.in  README  style.c
```

### ۳. خواندن README و INSTALL

```bash
less README
less INSTALL
```

همیشه این فایل‌ها را بخوان — اطلاعات مهمی درباره‌ی نیازمندی‌ها و مراحل نصب دارند.

### ۴. اجرای `configure`

```bash
./configure
```

```
checking build system type... i386-pc-linux-gnu
checking host system type... i386-pc-linux-gnu
checking for gcc... gcc
checking for C compiler default output file name... a.out
...
config.status: creating Makefile
```

اسکریپت `configure` سیستم را بررسی می‌کند و **Makefile** می‌سازد.

### ۵. کامپایل با `make`

```bash
make
```

```
gcc -c -I. -DHAVE_CONFIG_H diction.c
gcc -c -I. -DHAVE_CONFIG_H sentence.c
gcc -c -I. -DHAVE_CONFIG_H misc.c
gcc -o diction diction.o sentence.o misc.o getopt.o
```

### ۶. نصب

```bash
sudo make install
```

```
install -d /usr/local/bin
install diction /usr/local/bin
install style /usr/local/bin
install -d /usr/local/share/man/man1
install diction.1 /usr/local/share/man/man1
```

### ۷. آزمایش

```bash
which diction
```

```
/usr/local/bin/diction
```

```bash
diction README
```

---

## `make` — ابزار ساخت

`make` یک ابزار مدیریت ساخت (build system) است که:
- وابستگی‌های بین فایل‌ها را می‌فهمد
- فقط فایل‌هایی که تغییر کرده‌اند را دوباره کامپایل می‌کند
- دستورالعمل‌ها را از فایل **Makefile** می‌خواند

یک Makefile ساده:

```makefile
hello_world: hello_world.c
	gcc -o hello_world hello_world.c

clean:
	rm -f hello_world
```

```bash
make            # ساخت
make clean      # پاک‌سازی
```

---

## نصب در مکان سفارشی

پیش‌فرض `make install` برنامه را در `/usr/local` نصب می‌کند. برای تغییر:

```bash
./configure --prefix=/opt/myprogram
make
sudo make install
```

---

## خلاصه

کامپایل از کد منبع در ابتدا پیچیده به نظر می‌رسد، اما با سه دستور انجام می‌شود:

```bash
./configure
make
sudo make install
```

این مهارت دسترسی به طیف بسیار گسترده‌تری از نرم‌افزار را می‌دهد.

---

## مطالعه‌ی بیشتر

- مستندات GCC: [gcc.gnu.org](https://gcc.gnu.org)
- راهنمای GNU Make: [GNU Make Manual](https://www.gnu.org/software/make/manual/)

---

*پایان پارت ۳*

*فصل بعدی: [فصل ۲۴ — نوشتن اولین اسکریپت](ch24-writing-your-first-script)*
