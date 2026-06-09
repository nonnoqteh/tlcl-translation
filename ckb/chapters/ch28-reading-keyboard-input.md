---
layout: default
---
# بابەتی ٢٨ — خوێندنەوەی تێخستنی کیبۆرد
### Chapter 28 – Reading Keyboard Input

---

فەرمانەکانی ئەم بابەتە:

- `read` — خوێندنەوەی تێخستن لە stdin

---

## فەرمانی `read`

`read` یەک خێڵ لە تێخستنی ستاندارد دەخوێنێتەوە و لە گۆڕاودا دەیپارێزێت:

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

## ئۆپشنەکانی `read`

### `-p` — نیشاندانی پەیام

```bash
read -p "Enter your name: " name
echo "Hello, $name!"
```

### `-s` — حاڵەتی نهێنی (بۆ وشەی نهێنی)

```bash
read -s -p "Password: " password
echo ""
echo "Password received."
```

### `-t` — سنووری کات (چرکە)

```bash
if read -t 5 -p "Enter something (5 sec): " input; then
    echo "You entered: $input"
else
    echo "\nTime's up!"
fi
```

### `-n` —ژمارەی کاراکتەر

```bash
read -n 1 -p "Press any key to continue..."
echo ""
```

### `-r` — ڕێگریکردن لە تەفسیری بەکسلاش

```bash
read -r line
```

> پێشنیار دەکرێت هەمیشە `-r` بەکاربهێنی.

### چەند گۆڕاو

```bash
read -p "Enter first and last name: " first last
echo "First: $first"
echo "Last: $last"
```

---

## خوێندنەوە لە فایل

```bash
while read line; do
    echo "$line"
done < file.txt
```

---

## سەنجینی تێخستن

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

## مینوی کارتەوانی — نموونەی تەواو

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

## گۆڕاوی `IFS` — جیاکەری تێخستن

`IFS` (Internal Field Separator) دیاری دەکات `read` چۆن تێخستن دابەش دەکات:

```bash
IFS=":" read -r user pass uid gid info home shell <<< "root:x:0:0:root:/root:/bin/bash"
echo "User: $user, Shell: $shell"
```

---

*بابەتی داهاتوو: [بابەتی ٢٩ — کۆنترۆڵی جەریان: سووڕ بە while و until](ch29-flow-control-looping-with-while-until)*
