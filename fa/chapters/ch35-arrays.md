---
layout: default
---
# فصل ۳۵ — آرایه‌ها
### Chapter 35 – Arrays

---

## آرایه چیست؟

**آرایه** (array) متغیری است که می‌تواند چندین مقدار را نگه دارد. در bash دو نوع آرایه داریم:
- **آرایه‌ی مرتب** (indexed array) — با اندیس عددی
- **آرایه‌ی انجمنی** (associative array) — با کلید رشته‌ای (Bash 4+)

---

## آرایه‌ی مرتب

### ایجاد آرایه

```bash
# روش ۱ — مستقیم
a[1]=foo
a[2]=bar

# روش ۲ — یکجا
a=("foo" "bar" "baz")

# روش ۳ — declare
declare -a a=("foo" "bar" "baz")
```

### دسترسی به عناصر

```bash
a=("alpha" "beta" "gamma")

echo ${a[0]}     # alpha
echo ${a[1]}     # beta
echo ${a[2]}     # gamma
echo ${a[-1]}    # آخرین → gamma
```

> **مهم:** اندیس‌ها از `0` شروع می‌شوند.

### همه‌ی عناصر

```bash
echo ${a[@]}     # همه‌ی عناصر
echo ${a[*]}     # همه (یکجا)
echo ${#a[@]}    # تعداد عناصر
```

### اندیس‌ها

```bash
echo ${!a[@]}    # نمایش اندیس‌ها
```

---

## عملیات روی آرایه

### اضافه کردن عنصر

```bash
a=("foo" "bar")
a+=("baz")           # اضافه کردن به انتها
a[5]="extra"         # اندیس دلخواه
```

### حذف عنصر

```bash
unset a[1]           # حذف عنصر
unset a              # حذف کل آرایه
```

### برش آرایه (Slice)

```bash
a=("a" "b" "c" "d" "e")

echo ${a[@]:1}       # از اندیس ۱ → b c d e
echo ${a[@]:1:3}     # از ۱ به اندازه ۳ → b c d
```

### مرتب‌سازی

```bash
a=("banana" "apple" "cherry")
sorted=($(for i in "${a[@]}"; do echo "$i"; done | sort))
echo ${sorted[@]}    # apple banana cherry
```

---

## حلقه روی آرایه

```bash
colors=("red" "green" "blue" "yellow")

for color in "${colors[@]}"; do
    echo "Color: $color"
done
```

```bash
# با اندیس
for i in "${!colors[@]}"; do
    echo "Index $i: ${colors[$i]}"
done
```

---

## آرایه‌ی انجمنی (Bash 4+)

```bash
declare -A user_info

user_info["name"]="Alice"
user_info["age"]="30"
user_info["city"]="Tehran"

echo "Name: ${user_info["name"]}"
echo "Age: ${user_info["age"]}"

# همه‌ی کلیدها
echo "Keys: ${!user_info[@]}"

# حلقه
for key in "${!user_info[@]}"; do
    echo "$key = ${user_info[$key]}"
done
```

---

## مثال کاربردی — پردازش فایل‌ها

```bash
#!/bin/bash

# خواندن فایل‌ها در آرایه
files=()
while IFS= read -r -d '' file; do
    files+=("$file")
done < <(find . -name "*.txt" -print0)

echo "Found ${#files[@]} files:"
for f in "${files[@]}"; do
    echo "  $f"
done
```

---

## خلاصه

| عملیات | نحو |
|--------|-----|
| ایجاد | `a=("x" "y" "z")` |
| دسترسی | `${a[0]}` |
| همه | `${a[@]}` |
| تعداد | `${#a[@]}` |
| اندیس‌ها | `${!a[@]}` |
| اضافه | `a+=("new")` |
| حذف | `unset a[i]` |

---

*فصل بعدی: [فصل ۳۶ — موارد خاص](ch36-exotica.md)*
