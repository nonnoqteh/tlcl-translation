---
layout: default
---
# بابەتی ٢٤ — نووسینی یەکەم ئیسکریپتت
### Chapter 24 – Writing Your First Script

---

فەرمانەکانی ئەم بابەتە:

- `bash` — جێبەجێکردنی ئیسکریپتی شێل
- `chmod` — گۆڕینی مۆڵەتی فایل

---

## ئیسکریپتی شێل چییە؟

**ئیسکریپتی شێل** (shell script) فایلێکی دەقییە کە زنجیرەیەک لە فەرمانەکانی شێل تێدایە. بەجێی ئەوەی هەر جار فەرمانەکان یەک بە یەک بنووسین، لە فایلدا پاریزدەیین و یەک جار جێبەجێیان دەکەین.

---

## چۆن ئیسکریپت بنووسین؟

### ١. دروستکردنی فایل

```bash
nano hello_world
```

ناوەرۆکی فایل:

```bash
#!/bin/bash
# This is our first script.
echo "Hello World!"
```

### خێڵی یەکەم — Shebang

```bash
#!/bin/bash
```

ئەم خێڵە بە سیستەمی کارپێکردن دەڵێت ئەم فایلە بە `/bin/bash` جێبەجێ بکە. **هەمیشە دەبێت یەکەم خێڵی ئیسکریپت بێت.**

### کامێنت

```bash
# ئەمە کامێنتێکە
```

هەموو شتێک دوای `#` تا کۆتایی خێڵ، پشتگوێ دەخرێتەوە. کامێنتنووسین خۆی خۆی عادەتێکی باشە.

---

## ٢. ئیسکریپت جێبەجێکراو بکە

```bash
chmod 755 hello_world
```

یان:

```bash
chmod +x hello_world
```

---

## ٣. جێبەجێکردنی ئیسکریپت

```bash
./hello_world
```

```
Hello World!
```

> **بۆچی `./`؟** شێل بۆ جێبەجێکردن لە ناو `PATH` دەگەڕێت. چونکە پووشپەری ئێستا زۆری کات لە `PATH` نییە، دەبێت بە `./` ڕێگای تەواوەکە دیاری بکەیت.

---

## ٤. دانانی ئیسکریپت لە ناو `PATH`

بۆ دەستگەیشتن لە هەر شوێنێک:

```bash
mkdir -p ~/bin
cp hello_world ~/bin/
```

ئەگەر `~/bin` لە `PATH` نییە:

```bash
export PATH="$HOME/bin:$PATH"
```

ئەم خێڵە بزیانە بۆ `~/.bashrc` تا هەمیشەیی بێت:

```bash
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

ئێستا لە هەر شوێنێک دەتوانی جێبەجێی بکەیت:

```bash
hello_world
```

---

## ئیسکریپتێکی بەکارهاتووتر

```bash
#!/bin/bash

# script_info — نیشاندانی زانیاری سیستەم

echo "System Information Report"
echo "========================="
echo ""
echo "Hostname: $(hostname)"
echo "Uptime: $(uptime)"
echo "Disk Usage:"
df -h
echo ""
echo "Memory Usage:"
free -h
```

```bash
chmod 755 script_info
./script_info
```

---

## کورتە

| مەرحەلە | فەرمان |
|---------|--------|
| دروستکردنی فایل | `nano script_name` |
| دەستپێکردن بە shebang | `#!/bin/bash` |
| جێبەجێکراو کردن | `chmod 755 script_name` |
| جێبەجێکردن | `./script_name` |

---

*بابەتی داهاتوو: [بابەتی ٢٥ — دەستپێکردنی پڕۆژەیەک](ch25-starting-a-project)*
