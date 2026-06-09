# فصل ۳۴ — رشته‌ها و اعداد
### Chapter 34 – Strings and Numbers

---

## گسترش پارامتر (Parameter Expansion)

### عملیات پایه

```bash
name="Hello World"

echo ${#name}          # طول رشته → 11
echo ${name:6}         # از موقعیت 6 → World
echo ${name:6:5}       # از موقعیت 6 به اندازه 5 → World
echo ${name: -5}       # 5 کاراکتر آخر → World
```

### مقدار پیش‌فرض

```bash
echo ${var:-"default"}      # اگر var خالی است، "default" چاپ کن
echo ${var:="default"}      # اگر var خالی است، آن را "default" تعیین کن
echo ${var:?"error msg"}    # اگر var خالی است، پیغام خطا چاپ کن و خارج شو
echo ${var:+"other"}        # اگر var تعریف شده، "other" برگردان
```

### حذف پیشوند و پسوند

```bash
filename="archive.tar.gz"

echo ${filename#*.}        # حذف کوتاه‌ترین پیشوند → tar.gz
echo ${filename##*.}       # حذف بلندترین پیشوند → gz
echo ${filename%.*}        # حذف کوتاه‌ترین پسوند → archive.tar
echo ${filename%%.*}       # حذف بلندترین پسوند → archive
```

مثال کاربردی — تغییر پسوند:

```bash
for file in *.jpg; do
    cp "$file" "${file%.jpg}.png"
done
```

### جایگزینی رشته

```bash
str="Hello World World"

echo ${str/World/Linux}     # جایگزینی اول → Hello Linux World
echo ${str//World/Linux}    # جایگزینی همه → Hello Linux Linux
echo ${str/#Hello/Hi}       # جایگزینی در ابتدا → Hi World World
echo ${str/%World/Linux}    # جایگزینی در انتها → Hello World Linux
```

### تبدیل حروف

```bash
str="Hello World"

echo ${str,,}    # همه بچووک → hello world
echo ${str^^}    # همه گچووک → HELLO WORLD
echo ${str,}     # اول بچووک → hello World
echo ${str^}     # اول گچووک → Hello World
```

---

## عملیات حسابی

### `$(( ))` — محاسبه

```bash
echo $((5 + 3))       # 8
echo $((10 - 4))      # 6
echo $((3 * 4))       # 12
echo $((10 / 3))      # 3  (صحیح — بدون اعشار)
echo $((10 % 3))      # 1  (باقی‌مانده)
echo $((2 ** 8))      # 256 (توان)
```

### متغیرهای عددی

```bash
x=5
y=3

echo $((x + y))       # 8
echo $((x * y))       # 15

x=$((x + 1))          # افزایش
(( x++ ))             # معادل
(( x += 5 ))          # افزایش
```

### محاسبات پیشرفته با `bc`

برای اعداد اعشاری:

```bash
echo "scale=2; 10 / 3" | bc     # 3.33
echo "scale=4; sqrt(2)" | bc    # 1.4142
```

---

## تبدیل پایه‌ی عددی

```bash
echo $((16#ff))     # هگزادسیمال → 255
echo $((8#77))      # اکتال → 63
echo $((2#1010))    # باینری → 10

# نمایش در مبنای دیگر
printf "%x\n" 255    # ff
printf "%o\n" 255    # 377
printf "%b\n" 255    # (باینری در برخی سیستم‌ها)
```

---

## مقایسه‌ی رشته‌ها

```bash
str1="apple"
str2="banana"

if [[ "$str1" < "$str2" ]]; then
    echo "$str1 comes before $str2"
fi

if [[ "$str1" == "$str2" ]]; then
    echo "equal"
fi

if [[ -z "$str1" ]]; then
    echo "empty string"
fi
```

---

## مثال کامل — ماشین‌حساب

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

*فصل بعدی: [فصل ۳۵ — آرایه‌ها](ch35-arrays.md)*
