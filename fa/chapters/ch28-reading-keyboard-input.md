# فصل ۲۸ — خواندن ورودی از صفحه‌کلید
### Chapter 28 – Reading Keyboard Input

---

دستورهای این فصل:

- `read` — خواندن ورودی از stdin

---

## دستور `read`

`read` یک خط از ورودی استاندارد می‌خواند و در متغیر ذخیره می‌کند:

```bash
read variable
```

```bash
#!/bin/bash

echo -n "Enter a value: "
read value
echo "You entered: $value"
```

```
Enter a value: hello
You entered: hello
```

---

## گزینه‌های `read`

### `-p` — نمایش پیغام

```bash
read -p "Enter your name: " name
echo "Hello, $name!"
```

### `-s` — حالت مخفی (مناسب برای رمز عبور)

```bash
read -s -p "Password: " password
echo ""
echo "Password received."
```

### `-t` — محدودیت زمانی (ثانیه)

```bash
if read -t 5 -p "Enter something (5 sec): " input; then
    echo "You entered: $input"
else
    echo "\nTime's up!"
fi
```

### `-n` — تعداد کاراکتر

```bash
read -n 1 -p "Press any key to continue..."
echo ""
```

### `-r` — جلوگیری از تفسیر بک‌اسلش

```bash
read -r line
```

> توصیه می‌شود همیشه `-r` استفاده کنید.

### چند متغیر

```bash
read -p "Enter first and last name: " first last
echo "First: $first"
echo "Last: $last"
```

اگر کلمه‌های بیشتری وارد شود، مازاد به آخرین متغیر می‌رود.

---

## خواندن از فایل

```bash
while read line; do
    echo "$line"
done < file.txt
```

---

## ارزیابی ورودی

```bash
#!/bin/bash

read -p "Enter a number: " num

if [[ "$num" =~ ^[0-9]+$ ]]; then
    echo "$num is a valid number"
else
    echo "Invalid input!"
fi
```

---

## منوی تعاملی — مثال کامل

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

if [[ "$selection" =~ ^[0-3]$ ]]; then
    if [[ "$selection" == 0 ]]; then
        echo "Program terminated."
        exit
    fi
    if [[ "$selection" == 1 ]]; then
        echo "Hostname: $(hostname)"
        echo "Uptime: $(uptime)"
    fi
    if [[ "$selection" == 2 ]]; then
        df -h
    fi
    if [[ "$selection" == 3 ]]; then
        du -sh $HOME
    fi
else
    echo "Invalid entry."
fi
```

---

## متغیر `IFS` — جداکننده‌ی ورودی

`IFS` (Internal Field Separator) مشخص می‌کند `read` چطور ورودی را تقسیم کند:

```bash
# خواندن با جداکننده‌ی ":"
IFS=":" read -r user pass uid gid info home shell <<< "root:x:0:0:root:/root:/bin/bash"
echo "User: $user, Shell: $shell"
```

---

*فصل بعدی: [فصل ۲۹ — کنترل جریان: حلقه با while و until](ch29-flow-control-looping-with-while-until.md)*
