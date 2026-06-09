# فصل ۳۱ — کنترل جریان: شاخه‌بندی با case
### Chapter 31 – Flow Control: Branching with case

---

## دستور `case`

وقتی باید مقدار یک متغیر را با چند گزینه مقایسه کنیم، `case` از `if/elif` خواناتر است:

```bash
case word in
    pattern1)
        commands ;;
    pattern2)
        commands ;;
    *)
        commands ;;    # حالت پیش‌فرض (default)
esac
```

---

## مثال ساده

```bash
#!/bin/bash

read -p "Enter a character: " char

case "$char" in
    a|e|i|o|u)
        echo "Vowel"
        ;;
    [b-df-hj-np-tv-z])
        echo "Consonant"
        ;;
    [0-9])
        echo "Digit"
        ;;
    *)
        echo "Other character"
        ;;
esac
```

---

## الگوها در `case`

| الگو | معنا |
|------|------|
| `a` | دقیقاً حرف `a` |
| `[abc]` | یکی از `a`، `b`، `c` |
| `[a-z]` | هر حرف کوچک |
| `a|b` | `a` یا `b` |
| `*.txt` | هر رشته‌ای که به `.txt` ختم می‌شود |
| `?` | هر یک کاراکتر |
| `*` | هر چیزی (پیش‌فرض) |

---

## منوی تعاملی با `case`

```bash
#!/bin/bash

echo "
Please Select:

    1. Display System Information
    2. Display Disk Space
    3. Display Home Space Utilization
    0. Quit
"

read -p "Enter selection [0-3] > " selection

case "$selection" in
    0)
        echo "Program terminated."
        exit
        ;;
    1)
        echo "Hostname: $(hostname)"
        uptime
        ;;
    2)
        df -h
        ;;
    3)
        du -sh "$HOME"
        ;;
    *)
        echo "Invalid entry" >&2
        exit 1
        ;;
esac
```

---

## `case` برای پردازش آرگومان‌های خط فرمان

```bash
#!/bin/bash

usage () {
    echo "Usage: $0 [-h] [-v] [-f file]"
}

while [[ -n "$1" ]]; do
    case "$1" in
        -h | --help)
            usage
            exit
            ;;
        -v | --verbose)
            verbose=1
            ;;
        -f | --file)
            shift
            filename="$1"
            ;;
        *)
            echo "Unknown option: $1" >&2
            usage >&2
            exit 1
            ;;
    esac
    shift
done
```

---

## `;;&` — بررسی الگوهای بیشتر (Bash 4+)

معمولاً `case` بعد از اولین الگوی تطبیق‌یافته متوقف می‌شود. با `;;&` می‌توان ادامه داد:

```bash
case "$char" in
    [[:upper:]])
        echo "Uppercase"
        ;;&
    [[:alpha:]])
        echo "Alphabetic"
        ;;&
    [[:alnum:]])
        echo "Alphanumeric"
        ;;
esac
```

---

*فصل بعدی: [فصل ۳۲ — پارامترهای موضعی](ch32-positional-parameters.md)*
