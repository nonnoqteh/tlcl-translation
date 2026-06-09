---
layout: default
---
# فصل ۳۳ — کنترل جریان: حلقه با for
### Chapter 33 – Flow Control: Looping with for

---

## حلقه‌ی `for` — نسخه‌ی سنتی شل

```bash
for variable in list; do
    commands
done
```

### مثال‌های ساده

```bash
# روی یک لیست کلمه
for i in A B C D; do
    echo "$i"
done
```

```
A
B
C
D
```

```bash
# روی فایل‌ها
for file in *.txt; do
    echo "Processing: $file"
done
```

```bash
# با brace expansion
for i in {1..5}; do
    echo "Number: $i"
done
```

```bash
# با output دستور
for user in $(cut -d: -f1 /etc/passwd); do
    echo "$user"
done
```

---

## حلقه‌ی `for` — نسخه‌ی C-style

```bash
for (( expression1; expression2; expression3 )); do
    commands
done
```

```bash
for (( i=0; i<5; i++ )); do
    echo "i = $i"
done
```

```
i = 0
i = 1
i = 2
i = 3
i = 4
```

### مثال — جدول ضرب

```bash
#!/bin/bash

for (( i=1; i<=5; i++ )); do
    for (( j=1; j<=5; j++ )); do
        printf "%4d" $((i * j))
    done
    echo ""
done
```

```
   1   2   3   4   5
   2   4   6   8  10
   3   6   9  12  15
   4   8  12  16  20
   5  10  15  20  25
```

---

## `for` برای پردازش فایل‌ها

```bash
#!/bin/bash

# تغییر نام تمام فایل‌های .txt به .bak
for file in *.txt; do
    if [[ -f "$file" ]]; then
        mv "$file" "${file%.txt}.bak"
        echo "Renamed: $file → ${file%.txt}.bak"
    fi
done
```

---

## `for` با آرایه

```bash
colors=("red" "green" "blue")

for color in "${colors[@]}"; do
    echo "Color: $color"
done
```

---

## مقایسه‌ی `for`، `while`، `until`

| دستور | بهترین کاربرد |
|-------|--------------|
| `for` | تکرار روی لیست یا محدوده‌ی مشخص |
| `while` | تکرار تا زمانی که شرطی برقرار است |
| `until` | تکرار تا زمانی که شرطی برقرار نشود |

---

## مثال کاربردی — پشتیبان‌گیری

```bash
#!/bin/bash

BACKUP_DIR="/backup"
SOURCE_DIRS=("/home" "/etc" "/var/www")

for dir in "${SOURCE_DIRS[@]}"; do
    if [[ -d "$dir" ]]; then
        name=$(basename "$dir")
        tar czf "$BACKUP_DIR/${name}_$(date +%Y%m%d).tar.gz" "$dir"
        echo "Backed up: $dir"
    else
        echo "Warning: $dir not found"
    fi
done
```

---

*فصل بعدی: [فصل ۳۴ — رشته‌ها و اعداد](ch34-strings-and-numbers.md)*
