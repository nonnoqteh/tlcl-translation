---
layout: default
---
# بابەتی ٣١ — کۆنترۆڵی جەریان: لقبەندی بە case
### Chapter 31 – Flow Control: Branching with case

---

## فەرمانی `case`

کاتێک دەبێت بەهای گۆڕاوێک بە چەند هەڵبژاردن بەراوردبکەین، `case` لە `if/elif` خوێندنەوەتره:

```bash
case word in
    pattern1)
        commands ;;
    pattern2)
        commands ;;
    *)
        commands ;;    # حاڵەتی بنچینەیی (default)
esac
```

---

## نموونەی سادە

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

## شێوازەکان لە `case`

| شێواز | مانا |
|-------|------|
| `a` | تەنها پیتی `a` |
| `[abc]` | یەکێک لە `a`، `b`، `c` |
| `[a-z]` | هەر پیتێکی بچووک |
| `a\|b` | `a` یان `b` |
| `*.txt` | هەر ستریمگێک کە بە `.txt` کۆتادەبێت |
| `?` | هەر یەک کاراکتەر |
| `*` | هەر شتێک (بنچینەیی) |

---

## مینوی کارتەوانی بە `case`

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

## `case` بۆ پرۆسێسکردنی ئارگیومەنتەکانی خێڵی فەرمان

```bash
#!/bin/bash

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
            exit 1
            ;;
    esac
    shift
done
```

---

*بابەتی داهاتوو: [بابەتی ٣٢ — پارامێتەرەکانی شوێنی](ch32-positional-parameters)*
