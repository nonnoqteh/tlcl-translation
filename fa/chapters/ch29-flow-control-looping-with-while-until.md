---
layout: default
---
# فصل ۲۹ — کنترل جریان: حلقه با while و until
### Chapter 29 – Flow Control: Looping with while / until

---

## حلقه‌ی `while`

```bash
while commands; do
    commands
done
```

تا زمانی که شرط **درست** باشد، حلقه تکرار می‌شود.

### مثال — شمارش معکوس

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

## حلقه‌ی `until`

```bash
until commands; do
    commands
done
```

تا زمانی که شرط **نادرست** باشد، حلقه تکرار می‌شود — برعکس `while`.

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

### `break` — خروج از حلقه

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

### `continue` — رفتن به تکرار بعدی

```bash
for i in {1..10}; do
    if [[ $((i % 2)) -eq 0 ]]; then
        continue    # زوج‌ها را رد کن
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

## حلقه بر اساس کد خروجی

```bash
while ping -c 1 server &> /dev/null; do
    echo "Server is up"
    sleep 60
done
echo "Server is down!"
```

---

## منوی تعاملی با حلقه

```bash
#!/bin/bash

DELAY=3
COLUMNS=76

while true; do
    if read -s -n 1 -t "$DELAY" key; then
        case "$key" in
            q|Q) break ;;
        esac
    fi

    clear
    echo "System Monitor — Press q to quit"
    echo "================================"
    echo ""
    echo "Uptime:"
    uptime
    echo ""
    echo "Disk:"
    df -h /
    echo ""
    echo "Memory:"
    free -h
done
```

---

## خواندن فایل با `while`

```bash
#!/bin/bash

while IFS= read -r line; do
    echo "$line"
done < /etc/passwd
```

### پردازش خروجی دستور

```bash
while IFS= read -r line; do
    echo "Processing: $line"
done < <(ls -la)
```

---

## خلاصه

| دستور | کاربرد |
|-------|--------|
| `while` | تکرار تا زمانی که شرط درست است |
| `until` | تکرار تا زمانی که شرط نادرست است |
| `break` | خروج فوری از حلقه |
| `continue` | رفتن به تکرار بعدی |

---

*فصل بعدی: [فصل ۳۰ — رفع اشکال](ch30-troubleshooting)*
