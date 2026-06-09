---
layout: default
---
# بابەتی ٣٥ — ئەرەیەکان
### Chapter 35 – Arrays

---

## ئەرەی چییە؟

**ئەرەی** (array) گۆڕاوێکە کە دەتوانێت چەند بەها پارێزێت. لە bash دوو جۆر ئەرەیمان هەیە:
- **ئەرەیی رێکخراو** (indexed array) — بە ئیندێکسی ژمارەیی
- **ئەرەیی بەکارهێنان** (associative array) — بە کلیلی ستریمگ (Bash 4+)

---

## ئەرەیی رێکخراو

### دروستکردنی ئەرەی

```bash
# شێوازی ١
a[1]=foo
a[2]=bar

# شێوازی ٢
a=("foo" "bar" "baz")

# شێوازی ٣
declare -a a=("foo" "bar" "baz")
```

### دەستگەیشتن بە ئەندامەکان

```bash
a=("alpha" "beta" "gamma")

echo ${a[0]}     # alpha
echo ${a[1]}     # beta
echo ${a[2]}     # gamma
echo ${a[-1]}    # دوایی → gamma
```

> **گرنگ:** ئیندێکسەکان لە `0` دەستدەپێکن.

### هەموو ئەندامەکان

```bash
echo ${a[@]}     # هەموو ئەندامەکان
echo ${#a[@]}    # ژمارەی ئەندامەکان
echo ${!a[@]}    # ئیندێکسەکان
```

---

## کارەکان سەر ئەرەی

### زیادکردنی ئەندام

```bash
a=("foo" "bar")
a+=("baz")           # زیادکردن بۆ کۆتایی
a[5]="extra"         # ئیندێکسی دلخواز
```

### سڕینەوەی ئەندام

```bash
unset a[1]           # سڕینەوەی ئەندام
unset a              # سڕینەوەی تەواوی ئەرەی
```

### بڕینی ئەرەی (Slice)

```bash
a=("a" "b" "c" "d" "e")

echo ${a[@]:1}       # لە ئیندێکسی ١ → b c d e
echo ${a[@]:1:3}     # لە ١ بە درێژی ٣ → b c d
```

---

## سووڕ سەر ئەرەی

```bash
colors=("red" "green" "blue" "yellow")

for color in "${colors[@]}"; do
    echo "Color: $color"
done
```

```bash
# بە ئیندێکس
for i in "${!colors[@]}"; do
    echo "Index $i: ${colors[$i]}"
done
```

---

## ئەرەیی بەکارهێنان (Bash 4+)

```bash
declare -A user_info

user_info["name"]="Alice"
user_info["age"]="30"
user_info["city"]="Hewler"

echo "Name: ${user_info["name"]}"

# هەموو کلیلەکان
echo "Keys: ${!user_info[@]}"

# سووڕ
for key in "${!user_info[@]}"; do
    echo "$key = ${user_info[$key]}"
done
```

---

## کورتە

| کار | شێواز |
|-----|-------|
| دروستکردن | `a=("x" "y" "z")` |
| دەستگەیشتن | `${a[0]}` |
| هەموو | `${a[@]}` |
| ژمارە | `${#a[@]}` |
| ئیندێکسەکان | `${!a[@]}` |
| زیادکردن | `a+=("new")` |
| سڕینەوە | `unset a[i]` |

---

*بابەتی داهاتوو: [بابەتی ٣٦ — تایبەتمەندییە تایبەتەکان](ch36-exotica)*
