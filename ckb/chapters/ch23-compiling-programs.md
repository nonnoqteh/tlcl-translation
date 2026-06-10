---
layout: default
---
# بابەتی ٢٣ — کۆمپایڵکردنی بەرنامەکان
### Chapter 23 – Compiling Programs

---

فەرمانەکانی ئەم بابەتە:

- `make` — بەڕێوەبردنی پرۆسەی بیناکردن
- `configure` / `./configure` — ئامادەکردنی سۆرس بۆ بیناکردن

---

## چرا کۆمپایڵکردن؟

گەرچی زۆری نرمەواڵەکانی لینوکس بە `apt` یان `dnf` نصب دەبن، کەواتە هەندێ جار پێویستە:

- نرمەواڵەکەی بە مەخزەندا نییە
- دەتەوێت ئۆپشنی تایبەتی نصب بکەی
- نوێترین وەرسیۆن دەتەوێت
- کۆدی سەرچاوەی خۆت ئامادەکردووە

---

## کۆمپایڵکردن چییە؟

**کۆمپایڵکردن** گواستنەوەی کۆدی سەرچاوەیە بۆ **کۆدی ئامێر** (machine code) کە پرۆسێسۆر ڕاستەوخۆ جێبەجێی دەکات.

```
کۆدی سەرچاوه (C/C++)  →  کۆمپایلەر  →  فایلی جێبەجێکراو
```

لینوکس بە **GCC** (GNU Compiler Collection) کار دەکات.

---

## ئامادەکردن: نصبکردنی ئامرازەکانی بیناکردن

```bash
# Debian/Ubuntu
sudo apt install build-essential

# Fedora/CentOS
sudo dnf groupinstall "Development Tools"
```

---

## پرۆسەی ئاسایی کۆمپایڵکردن

زۆری نرمەواڵەی سەرچاوەباز ئەم ترتیبە بەکاردەهێنن:

### ١. دانلۆدی کۆدی سەرچاوە

```bash
wget https://example.com/program-1.0.tar.gz
tar xzf program-1.0.tar.gz
cd program-1.0
```

### ٢. کوڵکردنی فایلەکانی گرنگ

```bash
ls
cat README
cat INSTALL
```

### ٣. جێبەجێکردنی `configure`

`configure` سیستەم دەپشکنێت و `Makefile` دروستدەکات:

```bash
./configure
```

ئۆپشنی باو:

```bash
./configure --prefix=/usr/local    # پووشپەری نصب
./configure --help                 # نیشاندانی هەموو ئۆپشنەکان
```

ئەگەر سەرکەوتوو بوو:

```
checking for C compiler... gcc
checking for make... /usr/bin/make
...
configure: creating ./Makefile
```

### ٤. بیناکردن بە `make`

```bash
make
```

ئەم فەرمانە کۆمپایڵکردنی کۆدی سەرچاوە دەکات. دەتوانی ئازادانە پشتیم ببندرێت.

```
gcc -DHAVE_CONFIG_H -I. -I.. -g -O2 -c main.c
gcc -g -O2 -o program main.o utils.o
```

### ٥. نصبکردن

```bash
sudo make install
```

ئەم فەرمانە فایلەکان لە پووشپەری نصبدا دادەنێت (بنچینەیی `/usr/local`).

---

## نموونەی ڕاستەقینە

```bash
# دانلۆد و دەرهێنان
wget https://ftp.gnu.org/gnu/hello/hello-2.12.tar.gz
tar xzf hello-2.12.tar.gz
cd hello-2.12

# ئامادەکردن
./configure --prefix=$HOME/.local

# بیناکردن
make

# تاقیکردنەوە بەبێ نصب
src/hello

# نصبکردن
make install

# تاقیکردنەوه
~/.local/bin/hello
```

```
Hello, world!
```

---

## `make` — ئامرازی بیناکردن

`make` یەکێک لە گرنگترین ئامرازەکانی پێشکەوتنی نرمەواڵەیە:

```bash
make              # بیناکردن
make clean        # سڕینەوەی فایلی بیناکراو
make install      # نصبکردن
make uninstall    # لابردنی نصب
make check        # جێبەجێکردنی تێستەکان
```

### `Makefile` چییە؟

`Makefile` فایلێکە کە ڕێنمایی بیناکردن لێیەوە:

```makefile
CC = gcc
CFLAGS = -O2 -Wall

program: main.o utils.o
    $(CC) $(CFLAGS) -o program main.o utils.o

main.o: main.c
    $(CC) $(CFLAGS) -c main.c

clean:
    rm -f *.o program
```

---

## کۆمپایڵکردنی فایلی C ی سادە

```c
// hello.c
#include <stdio.h>
int main(void) {
    printf("Hello, World!\n");
    return 0;
}
```

```bash
gcc hello.c -o hello
./hello
```

```
Hello, World!
```

### ئۆپشنی گرنگی GCC

| ئۆپشن | مانا |
|-------|------|
| `-o name` | ناوی فایلی ئاوتپوت |
| `-O2` | ئاستی باشکردنەوەی ٢ |
| `-g` | زانیاری دیباگ |
| `-Wall` | هەموو ئاگادارکردنەوە |
| `-I path` | پووشپەری هێدەر زیاد بکە |
| `-l lib` | پەیوەستکردن بە کتێبخانە |

---

## کورتە

کۆمپایڵکردنی نرمەواڵە لە لینوکس ئامرازەکانی گرنگی هەیە:
- `./configure` — ئامادەکردن
- `make` — بیناکردن
- `sudo make install` — نصبکردن

ئەم پرۆسەیە دەستگەیشتن بە نرمەواڵەی هەر سەرچاوەیەک دەدات — لە کۆمپایلەری سادەوە تا سیستەمی کاری تەواو.

---

*کۆتایی پارتی سێ — کارەکانی باو و ئامرازە بنچینەیییەکان*

*بابەتی داهاتوو: [بابەتی ٢٤ — نووسینی یەکەم ئیسکریپت](ch24-writing-your-first-script)*
