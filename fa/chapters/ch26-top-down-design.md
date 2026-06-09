# فصل ۲۶ — طراحی از بالا به پایین
### Chapter 26 – Top-Down Design

---

## طراحی از بالا به پایین چیست؟

**طراحی از بالا به پایین** (top-down design) روشی است که در آن مسئله‌ی بزرگ را به وظایف کوچک‌تر تقسیم می‌کنیم. در اسکریپت‌نویسی این کار با **توابع** (functions) انجام می‌شود.

---

## توابع

### تعریف تابع

```bash
function_name () {
    commands
    return
}
```

یا با کلمه‌ی کلیدی `function`:

```bash
function function_name {
    commands
    return
}
```

### قوانین مهم

- تابع باید **قبل از فراخوانی** تعریف شود
- `return` اختیاری است — اگر نباشد تابع بعد از آخرین دستور برمی‌گردد

---

## مثال — اسکریپت با توابع

```bash
#!/bin/bash

# sys_info_page — گزارش سیستم

TITLE="System Information Report For $HOSTNAME"
CURRENT_TIME=$(date +"%x %r %Z")
TIMESTAMP="Generated $CURRENT_TIME, by $USER"

report_uptime () {
    echo "<h2>System Uptime</h2>"
    echo "<pre>$(uptime)</pre>"
    return
}

report_disk_space () {
    echo "<h2>Disk Space Utilization</h2>"
    echo "<pre>$(df -h)</pre>"
    return
}

report_home_space () {
    echo "<h2>Home Space Utilization</h2>"
    echo "<pre>$(du -sh /home/*)</pre>"
    return
}

cat << _EOF_
<html>
    <head>
        <title>$TITLE</title>
    </head>
    <body>
        <h1>$TITLE</h1>
        <p>$TIMESTAMP</p>
        $(report_uptime)
        $(report_disk_space)
        $(report_home_space)
    </body>
</html>
_EOF_
```

---

## توابع محلی (Local Variables)

متغیرهای داخل تابع بدون `local` سراسری (global) هستند:

```bash
foo () {
    local x=5        # محلی — فقط داخل این تابع
    echo $x
}
```

بدون `local`:

```bash
foo () {
    x=5              # سراسری — همه‌جا در دسترس
    echo $x
}
```

---

## Stub — پیاده‌سازی موقت

در طراحی از بالا به پایین، ابتدا توابع را با بدنه‌ی خالی می‌نویسیم تا ساختار اسکریپت مشخص شود:

```bash
report_uptime () {
    # TODO: implement
    return
}

report_disk_space () {
    # TODO: implement
    return
}
```

بعد یک‌به‌یک پیاده‌سازی می‌کنیم.

---

## خلاصه

| مفهوم | توضیح |
|-------|-------|
| طراحی از بالا به پایین | تقسیم مسئله به وظایف کوچک |
| تابع | بلوکی از کد با نام مشخص |
| `local` | متغیر محلی داخل تابع |
| stub | پیاده‌سازی موقت برای تست ساختار |

---

*فصل بعدی: [فصل ۲۷ — کنترل جریان: شاخه‌بندی با if](ch27-flow-control-branching-with-if.md)*
