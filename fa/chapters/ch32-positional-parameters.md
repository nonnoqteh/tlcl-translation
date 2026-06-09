---
layout: default
---
# فصل ۳۲ — پارامترهای موضعی
### Chapter 32 – Positional Parameters

---

## پارامترهای موضعی چیست؟

وقتی اسکریپتی را اجرا می‌کنیم، می‌توانیم آرگومان‌هایی به آن بدهیم. شل این آرگومان‌ها را در **پارامترهای موضعی** (positional parameters) ذخیره می‌کند:

```bash
./myscript arg1 arg2 arg3
```

| پارامتر | مقدار |
|---------|-------|
| `$0` | نام اسکریپت |
| `$1` | اولین آرگومان |
| `$2` | دومین آرگومان |
| `$3` | سومین آرگومان |
| `$@` | همه‌ی آرگومان‌ها (جداگانه) |
| `$*` | همه‌ی آرگومان‌ها (یکجا) |
| `$#` | تعداد آرگومان‌ها |

---

## مثال

```bash
#!/bin/bash

echo "Program name: $0"
echo "Number of arguments: $#"
echo "Argument 1: $1"
echo "Argument 2: $2"
echo "All arguments: $@"
```

```bash
./myscript hello world
```

```
Program name: ./myscript
Number of arguments: 2
Argument 1: hello
Argument 2: world
All arguments: hello world
```

---

## `shift` — جابجایی پارامترها

`shift` پارامترهای موضعی را یک جا به چپ منتقل می‌کند:

```bash
#!/bin/bash

while [[ -n "$1" ]]; do
    echo "Argument: $1"
    shift
done
```

```bash
./myscript a b c
```

```
Argument: a
Argument: b
Argument: c
```

---

## بررسی آرگومان‌ها

```bash
#!/bin/bash

if [[ $# -ne 2 ]]; then
    echo "Usage: $0 file1 file2" >&2
    exit 1
fi

echo "Comparing $1 and $2"
diff "$1" "$2"
```

---

## `$@` در مقابل `$*`

```bash
#!/bin/bash
print_args () {
    echo "Using \$@:"
    for arg in "$@"; do
        echo "  [$arg]"
    done

    echo "Using \$*:"
    for arg in "$*"; do
        echo "  [$arg]"
    done
}

print_args "hello world" "foo" "bar"
```

```
Using $@:
  [hello world]
  [foo]
  [bar]
Using $*:
  [hello world foo bar]
```

> **نتیجه:** تقریباً همیشه `"$@"` را استفاده کنید.

---

## مدیریت گزینه‌های خط فرمان

```bash
#!/bin/bash

usage () {
    echo "Usage: $(basename $0) [-h] [-v] filename"
    echo "  -h  Show help"
    echo "  -v  Verbose mode"
}

verbose=0

while getopts "hv" opt; do
    case "$opt" in
        h) usage; exit ;;
        v) verbose=1 ;;
        ?) usage >&2; exit 1 ;;
    esac
done

shift $((OPTIND - 1))    # رفتن به آرگومان‌های غیر-گزینه

filename="$1"

if [[ -z "$filename" ]]; then
    echo "Error: no filename given" >&2
    exit 1
fi

if [[ "$verbose" -eq 1 ]]; then
    echo "Processing: $filename"
fi
```

---

## `getopts` — پردازش گزینه

```bash
while getopts "hv:f:" opt; do
    case "$opt" in
        h) usage ;;
        v) level="$OPTARG" ;;    # OPTARG مقدار گزینه
        f) file="$OPTARG" ;;
    esac
done
```

- `:` بعد از حرف = آن گزینه نیاز به مقدار دارد
- `$OPTARG` مقدار گزینه

---

*فصل بعدی: [فصل ۳۳ — کنترل جریان: حلقه با for](ch33-flow-control-looping-with-for.md)*
