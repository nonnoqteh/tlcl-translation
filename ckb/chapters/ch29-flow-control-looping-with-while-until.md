---
layout: default
---
# بابەتی ٢٩ — کۆنترۆڵی جەریان: سووڕ بە while و until
### Chapter 29 – Flow Control: Looping with while / until

---

## سووڕی `while`

```bash
while commands; do
    commands
done
```

تا کاتێک مەرج **ڕاستە**، سووڕ دووبارەدەبێتەوە.

### نموونە — ژماردنی پاشگەڕاو

```bash
#!/bin/bash

count=1

while [[ "$count" -le 5 ]]; do
    echo "$count"
    count=$((count + 1))
done

echo "Finished!"
```

```
1
2
3
4
5
Finished!
```

---

## سووڕی `until`

```bash
until commands; do
    commands
done
```

تا کاتێک مەرج **هەڵەیە**، سووڕ دووبارەدەبێتەوە — پێچەوانەی `while`.

```bash
#!/bin/bash

count=1

until [[ "$count" -gt 5 ]]; do
    echo "$count"
    count=$((count + 1))
done
```

---

## `break` و `continue`

### `break` — دەرچوون لە سووڕ

```bash
while true; do
    read -p "Enter number (0 to quit): " n
    if [[ "$n" -eq 0 ]]; then
        break
    fi
    echo "You entered: $n"
done
echo "Loop ended."
```

### `continue` — چوون بۆ دووبارەکردنەوەی داهاتوو

```bash
for i in {1..10}; do
    if [[ $((i % 2)) -eq 0 ]]; then
        continue    # ژووبارەکان تێبپەڕێنە
    fi
    echo "$i"
done
```

```
1
3
5
7
9
```

---

## سووڕ بەپێی کۆدی دەرچوون

```bash
while ping -c 1 server &> /dev/null; do
    echo "Server is up"
    sleep 60
done
echo "Server is down!"
```

---

## خوێندنەوەی فایل بە `while`

```bash
#!/bin/bash

while IFS= read -r line; do
    echo "$line"
done < /etc/passwd
```

### پرۆسێسکردنی ئاوتپوتی فەرمان

```bash
while IFS= read -r line; do
    echo "Processing: $line"
done < <(ls -la)
```

---

## کورتە

| فەرمان | بەکارهێنان |
|--------|-----------|
| `while` | دووبارەکردنەوە تا مەرج ڕاستە |
| `until` | دووبارەکردنەوە تا مەرج ڕاست دەبێت |
| `break` | دەرچوونی ئارایی لە سووڕ |
| `continue` | چوون بۆ دووبارەکردنەوەی داهاتوو |

---

*بابەتی داهاتوو: [بابەتی ٣٠ — چارەسەرکردنی کێشەکان](ch30-troubleshooting)*
