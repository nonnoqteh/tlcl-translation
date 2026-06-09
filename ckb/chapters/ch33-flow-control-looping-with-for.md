---
layout: default
---
# بابەتی ٣٣ — کۆنترۆڵی جەریان: سووڕ بە for
### Chapter 33 – Flow Control: Looping with for

---

## سووڕی `for` — شێوازی کلاسیکی شێل

```bash
for variable in list; do
    commands
done
```

### نموونەی سادە

```bash
# سەر لیستێکی وشە
for i in A B C D; do
    echo "$i"
done
```

```bash
# سەر فایلەکان
for file in *.txt; do
    echo "Processing: $file"
done
```

```bash
# بە brace expansion
for i in {1..5}; do
    echo "Number: $i"
done
```

```bash
# بە ئاوتپوتی فەرمان
for user in $(cut -d: -f1 /etc/passwd); do
    echo "$user"
done
```

---

## سووڕی `for` — شێوازی C

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

### نموونە — جەدوەلی لێکدان

```bash
#!/bin/bash

for (( i=1; i<=5; i++ )); do
    for (( j=1; j<=5; j++ )); do
        printf "%4d" $((i * j))
    done
    echo ""
done
```

---

## `for` بۆ پرۆسێسکردنی فایلەکان

```bash
#!/bin/bash

# گۆڕینی ناوی هەموو فایلەکانی .txt بۆ .bak
for file in *.txt; do
    if [[ -f "$file" ]]; then
        mv "$file" "${file%.txt}.bak"
        echo "Renamed: $file → ${file%.txt}.bak"
    fi
done
```

---

## `for` لەگەڵ ئەرەی

```bash
colors=("red" "green" "blue")

for color in "${colors[@]}"; do
    echo "Color: $color"
done
```

---

## بەراوردکردنی `for`، `while`، `until`

| فەرمان | باشترین بەکارهێنان |
|--------|------------------|
| `for` | دووبارەکردنەوە سەر لیست یان مەودای دیاری |
| `while` | دووبارەکردنەوە تا مەرجێک بەرپاست |
| `until` | دووبارەکردنەوە تا مەرجێک بەرپا نەبێت |

---

## نموونەی کاربەکار — پشتیوانی

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

*بابەتی داهاتوو: [بابەتی ٣٤ — ستریمگەکان و ژمارەکان](ch34-strings-and-numbers)*
