# File-System-3

Computer Organization and Operating System Assignment (Chapter: File System, Sec3)

# บทคัดย่อ

# Files and Directories, FHS

# Raw Media Devices

### Raw Media Devices ใน Linux คืออะไร

Raw Media Devices ใน Linux คือ อุปกรณ์จัดเก็บข้อมูลที่สามารถเข้าถึงข้อมูลได้โดยตรงที่ระดับบล็อก โดยไม่ต้องผ่านระบบไฟล์ และ ตอนทำงานข้อมูลจะถูกเข้าถึงโดยไม่ผ่านระบบ Caches และ Buffers ของระบบปฏิบัติการ (OS)

ตัวอย่างของอุปกรณ์จัดเก็บข้อมูล:

- Hard Disk Drive (HDD)
- Solid State Drive (SSD)

### /dev/sda

`/dev/sda` เป็นอุปกรณ์บล็อก(Block Device) ของอุปกรณ์จัดเก็บข้อมูลใน Folder Root ของระบบ ตัวอย่างก็ HardDisk ที่ๆคอมของเราใช้กันอยู่นั่นแหละ

### Fdisk

`fdisk` เป็นโปรแกรมที่ใช้สำหรับการทำอะไรต่างๆ บนพาร์ติชันดิสก์ แต่จำเป็นต้องมีสิทธิ์ root เท่านั้นถึงจะใช้คำสั่งนี้ได้

#### ตัวอย่างคำสั่ง `fdisk`:

- `fdisk -l`: แสดง List พาร์ทิชัน
- `fdisk -d`: ลบ Partition
- `fdisk -n`: สร้าง Partition ใหม่
- `fdisk -v`: แสดงข้อมูล Version
- `fdisk -m`: แสดงเมนูคำสั่งต่างๆ

### Mkfs (Make File Systems)

`mkfs` เป็นคำสั่งที่ใช้สำหรับสร้างระบบไฟล์บน Linux

#### รูปแบบของคำสั่ง `mkfs`:

- `mkfs [ -V ] [ -t fstype ] [ fs-options ] filesys [ blocks ]`

เราสามารถดูระบบไฟล์ที่สามารถสร้างได้โดยการพิมพ์คำสั่ง `mkfs` แล้วกดแท็บสองครั้ง (ไม่ต้องมีเว้นวรรคหลังคำสั่ง `mkfs`)

![ตัวอย่างคำสั่ง Mkfs](https://static1.howtogeekimages.com/wordpress/wp-content/uploads/2019/10/1-5.png?q=50&fit=crop&w=750&dpr=1.5)

#### ตัวอย่างคำสั่งสร้างระบบไฟล์ ext4 ที่ /dev/sda1:

- `mkfs -t ext4 /dev/sda1`

# Physical Volume Administration

# Volume Group Administration

# Logical Volume Administration

# File System Type

# Archiver, Backup/Restore Tools

# References

Raw Media Divices

```
Raw device - Wikipedia: https://en.wikipedia.org/wiki/Raw_device
Raw Disk - linfo.org: https://www.linfo.org/raw_disk.html
dev-sda - Baeldung: https://www.baeldung.com/linux/dev-sda
fdisk(8) - Linux manual page: https://man7.org/linux/man-pages/man8/fdisk.8.html
FDISK - TechTarget: https://www.techtarget.com/whatis/definition/FDISK
How to Use the mkfs Command on Linux - How-To Geek: https://www.howtogeek.com/443342/how-to-use-the-mkfs-command-on-linux/
mkfs command in Linux with Examples - GeeksforGeeks: https://www.geeksforgeeks.org/mkfs-command-in-linux-with-examples/
Linux Command: mkfs - Hongkiat: https://www.hongkiat.com/blog/linux-command-mkfs/
```

# สมาชิก

| รูป | รหัส     | ชื่อ                  | ส่วนที่รับผิดชอบ               |
| --- | -------- | --------------------- | ------------------------------ |
|     | 65070117 | นัชชา เนินกร่าง       | File System Type               |
|     | 65070118 | นันทพงศ์ วิเศษมงคลชัย | Raw Media Devices              |
|     | 65070129 | ปรเมศวร์ โพธิ์หมุด    | Volume Group Administration    |
|     | 65070135 | ปัณณวิชญ์ ปานช้าง     | Logical Volume Administration  |
|     | 65070136 | ปานชีวา สุ่มมาตย์     | Archiver, Backup/Restore Tools |
|     | 65070160 | พีรเดช เสือแก้วน้อย   | Files and Directories, FHS     |
|     | 65070177 | ภูมิ บุตรศรีชา        | Physical Volume Administration |

F
