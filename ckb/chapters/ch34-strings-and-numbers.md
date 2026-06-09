---
layout: default
---
# بابەتی ٣٤ — ستریمگەکان و ژمارەکان
### Chapter 34 – Strings and Numbers

---

## فراوانکردنی پارامێتەر (Parameter Expansion)

### کارەکانی بنچینەیی

```bash
name="Hello World"

echo ${#name}          # درێژی ستریمگ → 11
echo ${name:6}         # لە شوێنی 6 → World
echo ${name:6:5}       # لە شوێنی 6 بە درێژی 5 → World
echo ${name: -5}       # 5 کاراکتەری دوایی → World
```

### بەهای بنچینەیی

```bash
echo ${var:-"default"}      # ئەگەر var خاڵی بوو، "default" چاپبکە
echo ${var:="default"}      # ئەگەر var خاڵی بوو، "default" پێبدە
echo ${var:?"error msg"}    # ئەگەر var خاڵی بوو، هەڵەناو چاپبکە و دەربچە
echo ${var:+"other"}        # ئەگەر var پێناسەکراوبوو، "other" بگەڕێنەوە
```

### سڕینەوەی پێشگر و پاشگر

```bash
filename="archive.tar.gz"

echo ${filename#*.}        # سڕینەوەی کووتاترین پێشگر → tar.gz
echo ${filename##*.}       # سڕینەوەی درێژترین پێشگر → gz
echo ${filename%.*}        # سڕینەوەی کووتاترین پاشگر → archive.tar
echo ${filename%%.*}       # سڕینەوەی درێژترین پاشگر → archive
```

نموونەی کاربەکار — گۆڕینی پاشگر:

```bash
for file in *.jpg; do
    cp "$file" "${file%.jpg}.png"
done
```

### جێگرینی ستریمگ

```bash
str="Hello World World"

echo ${str/World/Linux}     # جێگرینی یەکەم → Hello Linux World
echo ${str//World/Linux}    # جێگرینی هەموو → Hello Linux Linux
echo ${str/#Hello/Hi}       # جێگرینی لە سەرەتا → Hi World World
echo ${str/%World/Linux}    # جێگرینی لە کۆتایی → Hello World Linux
```

### گۆڕینی پیتەکان

```bash
str="Hello World"

echo ${str,,}    # هەموو بچووک → hello world
echo ${str^^}    # هەموو گەورە → HELLO WORLD
```

---

## کارەکانی ژمارەیی

### `$(( ))` — ژمارەکاری

```bash
echo $((5 + 3))       # 8
echo $((10 - 4))      # 6
echo $((3 * 4))       # 12
echo $((10 / 3))      # 3  (تەواو — بەبێ ئەشاری)
echo $((10 % 3))      # 1  (ماوە)
echo $((2 ** 8))      # 256 (توانا)
```

### گۆڕاوەکانی ژمارەیی

```bash
x=5
y=3

echo $((x + y))       # 8
x=$((x + 1))          # زیادکردن
(( x++ ))             # هاوتاکە
(( x += 5 ))          # زیادکردن
```

### ژمارەکاری پێشکەوتوو بە `bc`

بۆ ژمارەی ئەشاری:

```bash
echo "scale=2; 10 / 3" | bc     # 3.33
echo "scale=4; sqrt(2)" | bc    # 1.4142
```

---

## گۆڕینی بنچینەی ژمارەیی

```bash
echo $((16#ff))     # هێگزادێسیمال → 255
echo $((8#77))      # ئۆکتال → 63
echo $((2#1010))    # باینەری → 10

printf "%x\n" 255    # ff
printf "%o\n" 255    # 377
```

---

## نموونەی تەواو — ماشینی ژمارەکار

```bash
#!/bin/bash

read -p "Enter first number: " a
read -p "Enter operator (+, -, *, /): " op
read -p "Enter second number: " b

case "$op" in
    +) result=$((a + b)) ;;
    -) result=$((a - b)) ;;
    \*) result=$((a * b)) ;;
    /)
        if [[ $b -eq 0 ]]; then
            echo "Error: division by zero" >&2
            exit 1
        fi
        result=$((a / b))
        ;;
    *)
        echo "Unknown operator: $op" >&2
        exit 1
        ;;
esac

echo "$a $op $b = $result"
```

---

*بابەتی داهاتوو: [بابەتی ٣٥ — ئەرەیەکان](ch35-arrays)*
