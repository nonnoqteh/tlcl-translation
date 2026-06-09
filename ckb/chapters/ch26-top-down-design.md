---
layout: default
---
# بابەتی ٢٦ — دیزاینی لە سەرەوە بۆ خوارەوە
### Chapter 26 – Top-Down Design

---

## دیزاینی لە سەرەوە بۆ خوارەوە چییە؟

**دیزاینی لە سەرەوە بۆ خوارەوە** (top-down design) ڕێگایەکە کە تێیدا کێشەی گەورە دابەش دەکەین بۆ ئەرکە بچووکەکان. لە ئیسکریپتنووسیدا ئەم کارە بە **فەنکشن** (functions) ئەنجامدەدرێت.

---

## فەنکشنەکان

### پێناسەکردنی فەنکشن

```bash
function_name () {
    commands
    return
}
```

یان بە کلیلەوشەی `function`:

```bash
function function_name {
    commands
    return
}
```

### یاساکانی گرنگ

- فەنکشن دەبێت **پێش فراخواندن** پێناسە بکرێت
- `return` ئارەزوومەندانەیە — ئەگەر نەبوو فەنکشن دوای دوایین فەرمان دەگەڕێتەوە

---

## نموونە — ئیسکریپت بە فەنکشن

```bash
#!/bin/bash

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

## گۆڕاوی ناوخۆ (Local Variables)

گۆڕاوەکانی ناو فەنکشن بەبێ `local` گشتی (global) دەبن:

```bash
foo () {
    local x=5        # ناوخۆ — تەنها لە ناو ئەم فەنکشنە
    echo $x
}
```

بەبێ `local`:

```bash
foo () {
    x=5              # گشتی — هەرشوێنێک دەستگەیشتنی پێیەتی
    echo $x
}
```

---

## Stub — پیاده‌سازی کاتی

لە دیزاینی لە سەرەوە بۆ خوارەوە، سەرەتا فەنکشنەکان بە بدەنەی خاڵی دەنووسین تا ساختاری ئیسکریپت ڕوون بێت:

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

پاشان یەک بە یەک پیاده‌سازیان دەکەین.

---

## کورتە

| تێڕوانین | ڕوونکردنەوە |
|---------|------------|
| دیزاینی لە سەرەوە بۆ خوارەوە | دابەشکردنی کێشە بۆ ئەرکە بچووکەکان |
| فەنکشن | بلۆکێکی کۆد بە ناوی دیاری |
| `local` | گۆڕاوی ناوخۆ لە فەنکشن |
| stub | پیاده‌سازی کاتی بۆ تاقیکردنەوەی ساختار |

---

*بابەتی داهاتوو: [بابەتی ٢٧ — کۆنترۆڵی جەریان: لقبەندی بە if](ch27-flow-control-branching-with-if)*
