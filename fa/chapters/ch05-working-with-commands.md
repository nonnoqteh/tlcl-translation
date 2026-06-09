# فصل ۵ — کار با دستورها
### Chapter 5 – Working with Commands

---

تا اینجا با دستوراتی روبرو شدیم که هرکدام گزینه‌ها و آرگومان‌های مرموز خودشان را داشتند. در این فصل سعی می‌کنیم بخشی از این رمز و راز را کنار بزنیم و حتی دستورات خودمان را بسازیم. دستورات معرفی‌شده در این فصل:

- `type` — نمایش نوع یک دستور
- `which` — نمایش مسیر یک برنامه‌ی اجرایی
- `help` — راهنمای دستورات داخلی شل
- `man` — نمایش صفحه‌ی راهنمای دستور
- `apropos` — نمایش دستورات مرتبط
- `info` — نمایش اطلاعات یک دستور
- `whatis` — نمایش توضیح یک‌خطی
- `alias` — ساختن نام مستعار برای دستور

---

## دستور دقیقاً چیست؟
### What Exactly Are Commands?

یک دستور می‌تواند یکی از چهار چیز باشد:

1. **برنامه‌ی اجرایی** — مثل همه‌ی فایل‌هایی که در `/usr/bin` دیدیم. برنامه‌های کامپایل‌شده (مثل C/C++) یا اسکریپت‌های نوشته‌شده به زبان‌هایی مثل Python یا Ruby.
2. **دستور داخلی شل** — bash تعدادی دستور دارد که درون خودش پیاده‌سازی شده‌اند. مثلاً `cd`.
3. **تابع شل** — اسکریپت‌های کوچک شل که در محیط گنجانده شده‌اند.
4. **نام مستعار (Alias)** — دستوراتی که ما خودمان از دستورات دیگر می‌سازیم.

---

## شناسایی دستورها
### Identifying Commands

### `type` — نمایش نوع دستور

دستور `type` نشان می‌دهد که شل چه نوع دستوری را اجرا خواهد کرد:

```bash
type type
```

```
type is a shell builtin
```

```bash
type ls
```

```
ls is aliased to 'ls --color=auto'
```

```bash
type cp
```

```
cp is /usr/bin/cp
```

### `which` — نمایش مسیر برنامه

گاهی چند نسخه از یک برنامه‌ی اجرایی روی سیستم نصب است. برای یافتن مسیر دقیق:

```bash
which ls
```

```
/usr/bin/ls
```

> **نکته:** `which` فقط برای برنامه‌های اجرایی کار می‌کند، نه دستورات داخلی یا نام‌های مستعار.

---

## مستندات یک دستور
### Getting a Command's Documentation

### `help` — راهنمای دستورات داخلی

bash برای هر دستور داخلی‌اش راهنما دارد:

```bash
help cd
```

```
cd: cd [-L|[-P [-e]] [-@]] [dir]
    Change the shell working directory.
    ...
```

> **نکته درباره‌ی نشانه‌گذاری:** وقتی کروشه `[]` می‌بینید، یعنی آن قسمت اختیاری است. خط عمودی `|` یعنی «یا».

### `--help` — نمایش اطلاعات استفاده

اکثر برنامه‌های اجرایی از گزینه‌ی `--help` پشتیبانی می‌کنند:

```bash
mkdir --help
```

```
Usage: mkdir [OPTION] DIRECTORY...
Create the DIRECTORY(ies), if they do not already exist.
  -m, --mode=MODE   set file mode
  -p, --parents     make parent directories as needed
  -v, --verbose     print a message for each created directory
  ...
```

### `man` — صفحه‌ی راهنما

اکثر برنامه‌های خط فرمان مستنداتی به نام **man page** دارند:

```bash
man ls
```

صفحات man با `less` نمایش داده می‌شوند، پس همه‌ی دستورات `less` کار می‌کنند.

ساختار کلی man page:
- عنوان صفحه
- خلاصه‌ی نحو دستور
- توضیح هدف دستور
- فهرست گزینه‌ها

**بخش‌های راهنما:**

| بخش | محتوا |
|-----|-------|
| ۱ | دستورات کاربر |
| ۲ | رابط برنامه‌نویسی هسته |
| ۳ | رابط برنامه‌نویسی کتابخانه‌ی C |
| ۴ | فایل‌های خاص (دستگاه‌ها) |
| ۵ | فرمت فایل‌ها |
| ۶ | بازی‌ها |
| ۷ | متفرقه |
| ۸ | دستورات مدیریت سیستم |

برای باز کردن یک بخش خاص:

```bash
man 5 passwd
```

### `apropos` — نمایش دستورات مرتبط

برای جستجو در فهرست man page‌ها بر اساس کلیدواژه:

```bash
apropos partition
```

```
addpart (8)    - simple wrapper around the "add partition"...
fdisk (8)      - manipulate disk partition table
parted (8)     - a partition manipulation program
...
```

### `whatis` — توضیح یک‌خطی

```bash
whatis ls
```

```
ls (1)  - list directory contents
```

### `info` — اطلاعات دقیق‌تر

پروژه‌ی گنو برای برنامه‌هایش جایگزینی به نام **info** فراهم کرده که مثل صفحات وب هایپرلینک دارد:

```bash
info coreutils
```

**دستورات `info`:**

| دستور | عملکرد |
|-------|--------|
| `?` | نمایش راهنما |
| `PgUp` | صفحه‌ی قبل |
| `PgDn` | صفحه‌ی بعد |
| `n` | نود بعدی |
| `p` | نود قبلی |
| `u` | نود والد |
| `Enter` | دنبال کردن هایپرلینک |
| `q` | خروج |

### فایل‌های README

بسیاری از بسته‌های نرم‌افزاری مستنداتشان را در `/usr/share/doc` ذخیره می‌کنند:

```bash
ls /usr/share/doc
```

بیشتر آن‌ها متن ساده هستند و با `less` می‌توان خواند. فایل‌های با پسوند `.gz` با `zless` قابل مشاهده‌اند.

---

## ساختن دستورات خودمان با `alias`
### Creating Our Own Commands with alias

حالا اولین تجربه‌ی برنامه‌نویسی‌مان! می‌توانیم چند دستور را با `;` در یک خط ترکیب کنیم:

```bash
cd /usr; ls; cd -
```

```
bin  games  include  lib  local  sbin  share  src
/home/me
```

حالا این را به یک دستور جدید تبدیل کنیم. اول بررسی کنیم نام موردنظر آزاد است:

```bash
type test
```

```
test is a shell builtin
```

`test` گرفته شده. `foo` را امتحان می‌کنیم:

```bash
type foo
```

```
bash: type: foo: not found
```

عالی! حالا نام مستعار می‌سازیم:

```bash
alias foo='cd /usr; ls; cd -'
```

ساختار دستور alias:

```
alias نام='دستورات'
```

بعد از تعریف:

```bash
foo
```

```
bin  games  include  lib  local  sbin  share  src
/home/me
```

برای دیدن نام مستعار:

```bash
type foo
```

```
foo is aliased to 'cd /usr; ls; cd -'
```

برای حذف نام مستعار:

```bash
unalias foo
```

برای دیدن همه‌ی نام‌های مستعار تعریف‌شده:

```bash
alias
```

```
alias l.='ls -d .* --color=tty'
alias ll='ls -l --color=tty'
alias ls='ls --color=tty'
```

> **نکته:** نام‌های مستعاری که در خط فرمان تعریف می‌کنید با پایان نشست شل از بین می‌روند. در فصل ۱۱ یاد می‌گیریم چطور آن‌ها را دائمی کنیم.

---

## خلاصه

حالا که یاد گرفتیم چطور مستندات دستورها را پیدا کنیم، برو و مستندات همه‌ی دستوراتی که تا اینجا دیده‌ایم را مطالعه کن. گزینه‌های اضافی را ببین و امتحان کن!

---

## مطالعه‌ی بیشتر

- مرجع bash: [Bash Reference Manual](http://www.gnu.org/software/bash/manual/bashref.html)
- سؤالات متداول bash: [Bash FAQ](http://mywiki.wooledge.org/BashFAQ)
- مستندات پروژه‌ی گنو: [GNU Manuals](http://www.gnu.org/manual/manual.html)

---

*فصل بعدی: [فصل ۶ — تغییر مسیر](ch06-redirection.md)*
