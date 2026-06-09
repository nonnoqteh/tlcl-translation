---
layout: default
---
# بابەتی ٣٢ — پارامێتەرەکانی شوێنی
### Chapter 32 – Positional Parameters

---

## پارامێتەرەکانی شوێنی چین؟

کاتێک ئیسکریپتێک جێبەجێدەکەین، دەتوانین ئارگیومەنتیان پێبدەین. شێل ئەم ئارگیومەنتانە لە **پارامێتەرەکانی شوێنی** (positional parameters) دەپارێزێت:

```bash
./myscript arg1 arg2 arg3
```

| پارامێتەر | بەها |
|-----------|-----|
| `$0` | ناوی ئیسکریپت |
| `$1` | یەکەم ئارگیومەنت |
| `$2` | دووەم ئارگیومەنت |
| `$3` | سێیەم ئارگیومەنت |
| `$@` | هەموو ئارگیومەنتەکان (جیاجیا) |
| `$*` | هەموو ئارگیومەنتەکان (یەکجا) |
| `$#` | ژمارەی ئارگیومەنتەکان |

---

## نموونە

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

## `shift` — جووڵاندنی پارامێتەرەکان

`shift` پارامێتەرەکانی شوێنی یەک شوێن بۆ چەپ دەگوازێتەوە:

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

## پشکنینی ئارگیومەنتەکان

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

## `$@` لە بەرامبەر `$*`

```bash
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

> **ئەنجام:** تەقریبەن هەمیشە `"$@"` بەکاربهێنە.

---

## بەڕێوەبردنی هەڵبژاردنەکانی خێڵی فەرمان بە `getopts`

```bash
while getopts "hv:f:" opt; do
    case "$opt" in
        h) usage ;;
        v) level="$OPTARG" ;;
        f) file="$OPTARG" ;;
    esac
done
```

- `:` دوای پیت = ئەو هەڵبژاردنە پێویستی بەهایەکی هەیە
- `$OPTARG` بەهای هەڵبژاردن

---

*بابەتی داهاتوو: [بابەتی ٣٣ — کۆنترۆڵی جەریان: سووڕ بە for](ch33-flow-control-looping-with-for)*
