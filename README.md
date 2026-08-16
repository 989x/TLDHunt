# TLDHunt - Domain Availability Checker

TLDHunt เป็นเครื่องมือบรรทัดคำสั่งสำหรับเช็คว่าชื่อโดเมนว่างหรือไม่ในหลาย TLD พร้อมกัน ใส่คำ keyword กับลิสต์ TLD เข้าไป มันจะบอกว่าโดเมนไหนว่างบ้าง

> ทดสอบบน Kali Linux และ macOS (ต้องใช้ Homebrew Bash 4+)

## Dependencies

- curl — ใช้เช็คผ่าน RDAP และดึงลิสต์ TLD จาก IANA
- whois — ใช้เป็น fallback สำหรับ TLD ที่ไม่รองรับ RDAP
- Bash 4 ขึ้นไป — macOS มากับ Bash 3.2 ซึ่งใช้ไม่ได้ (ไม่มีคำสั่ง readarray)

Linux:
```bash
sudo apt install whois curl -y
```

macOS:
```bash
brew install whois bash
```

## Run
รันสคริปต์ด้วย Homebrew Bash แทนตัวระบบ:
```bash
/opt/homebrew/bin/bash tldhunt.sh -k <keyword> -E popular.txt
```

## How It Works

เช็คแต่ละโดเมนผ่าน RDAP ก่อนเป็นอันดับแรก ซึ่งเป็นโปรโตคอลมาตรฐานที่ตอบกลับด้วย HTTP status code ชัดเจน (404 = ว่าง, 200 = ถูกจดแล้ว) แม่นยำกว่าการอ่านข้อความดิบจาก whois มาก

ถ้า TLD ไหนไม่รองรับ RDAP สคริปต์จะ fallback ไปสแกนข้อความจาก whois แทน โดยหาคำเช่น "Name Server" หรือ "status: active" ผลลัพธ์กลุ่มนี้จะติดป้าย (whois, unverified) เพราะรูปแบบข้อความ whois แต่ละ registry ไม่เหมือนกัน อาจไม่แม่นเสมอไป ควรเช็คซ้ำด้วยตัวเองอีกที

ไฟล์ลิสต์ TLD เป็น plain text หนึ่งนามสกุลต่อบรรทัด:
```
.com
.net
.io
.dev
```

ค่า default คือ popular.txt ซึ่งเป็นลิสต์ TLD ที่ใช้บ่อย ใช้ -E <file> เพื่อชี้ไปที่ลิสต์อื่นได้ หรือรัน --update-tld ก่อนถ้าอยากได้ลิสต์เต็มจาก IANA

## Screenshot
![TLDHunt](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiH2w600_IzO7BX6TmRECWzHu3aXlxsMVVBsvCk5cZ56x6v341edcGB3ByhhFiojjpkenLxShLVu5mpUeO9PO05Rv37fjylD2f5rpHodI8-6YelfVKXuvOcjbvlIgVteTtNpnaHYAm_xz9n7Q86ln6U9SAgUV6y65Dfg6UAdc-bb-vyHmuHvp63-Qlujlwx/s949/tldhunt.png "TLDHunt")
