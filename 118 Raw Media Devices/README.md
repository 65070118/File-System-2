# Raw Media Devices

## Raw Media Devices ใน Linux คืออะไร

Raw Media Devices ใน Linux คือ อุปกรณ์จัดเก็บข้อมูลที่สามารถเข้าถึงข้อมูลได้โดยตรงที่ระดับบล็อก โดยไม่ต้องผ่านระบบไฟล์ และ ตอนทำงานข้อมูลจะถูกเข้าถึงโดยไม่ผ่านระบบ Caches และ Buffers ของระบบปฏิบัติการ (OS)

ตัวอย่างของอุปกรณ์จัดเก็บข้อมูล:

- Hard Disk Drive (HDD)
- Solid State Drive (SSD)

## /dev/sda

`/dev/sda` เป็นอุปกรณ์บล็อก(Block Device) ของอุปกรณ์จัดเก็บข้อมูลใน Folder Root ของระบบ ตัวอย่างก็ HardDisk ที่ๆคอมของเราใช้กันอยู่นั่นแหละ

## Fdisk

`fdisk` เป็นโปรแกรมที่ใช้สำหรับการทำอะไรต่างๆ บนพาร์ติชันดิสก์ แต่จำเป็นต้องมีสิทธิ์ root เท่านั้นถึงจะใช้คำสั่งนี้ได้

## ตัวอย่างคำสั่ง `fdisk`

- `fdisk -l` แสดง List พาร์ทิชัน
- `fdisk -d` ลบ Partition
- `fdisk -n` สร้าง Partition ใหม่
- `fdisk -v` แสดงข้อมูล Version
- `fdisk -m` แสดงเมนูคำสั่งต่างๆ

### ตัวอย่างการใช้คำสั่ง fdisk -m เพื่อแสดงเมนูคำสั่งต่างๆ

```
$ fdisk /dev/sda
คำเตือน: โหมดที่เข้ากันได้กับ DOS ถูกยกเลิกแล้ว แนะนำให้เปลี่ยนโหมดนี้ (คำสั่ง 'c') และเปลี่ยนหน่วยการแสดงผลเป็น
sectors (คำสั่ง 'u').
คำสั่ง (m เพื่อแสดงความช่วยเหลือ): m
** การทำงานของคำสั่ง **
a   เปลี่ยนสถานะบูต
b   แก้ไข bsd disklabel
c   เปลี่ยนสถานะความเข้ากันได้กับ DOS
d   ลบพาร์ติชัน
l   รายการประเภทพาร์ติชันที่รู้จัก
m   แสดงเมนูนี้
n   เพิ่มพาร์ติชันใหม่
o   สร้างตารางพาร์ติชัน DOS ใหม่
p   พิมพ์ตารางพาร์ติชัน
q   ออกโดยไม่บันทึกการเปลี่ยนแปลง
s   สร้าง Sun disklabel ใหม่ที่ว่าง
t   เปลี่ยนรหัสระบบของพาร์ติชัน
u   เปลี่ยนหน่วยการแสดงผล / การป้อนข้อมูล
v   ตรวจสอบตารางพาร์ติชัน
w   เขียนตารางลงในดิสก์และออก
x   ฟังก์ชันเพิ่มเติม (ผู้เชี่ยวชาญเท่านั้น)
คำสั่ง (m เพื่อแสดงความช่วยเหลือ):
```

### ตัวอย่างการใช้คำสั่ง fdisk -l เพื่อแสดงข้อมูล disk partition ของ Linux

```
$ fdisk -l
Disk /dev/sda: 74.5 GiB, 80000000000 bytes, 156250000 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
ประเภทของ Disklabel: dos
รหัสตัวแยก Disk: 0x6e0c3dab

อุปกรณ์ Boot Start End Sectors Size Id Type
/dev/sda1 2048 152344575 152342528 72.7G 83 Linux
/dev/sda2 * 152344576 152735743 391168 191M 83 Linux
/dev/sda3 152735744 156248063 3512320 1.7G 82 Linux swap / Solaris
```

## Mkfs (Make File Systems)

`mkfs` เป็นคำสั่งที่ใช้สำหรับสร้างระบบไฟล์บน Linux

## รูปแบบของคำสั่ง `mkfs`

- `mkfs [ -V ] [ -t fstype ] [ fs-options ] filesys [ blocks ]`

เราสามารถดูระบบไฟล์ที่สามารถสร้างได้โดยการพิมพ์คำสั่ง `mkfs` แล้วกดแท็บสองครั้ง (ไม่ต้องมีเว้นวรรคหลังคำสั่ง `mkfs`)

![ตัวอย่างคำสั่ง Mkfs](https://static1.howtogeekimages.com/wordpress/wp-content/uploads/2019/10/1-5.png?q=50&fit=crop&w=750&dpr=1.5)

## ตัวอย่างคำสั่งสร้างระบบไฟล์ ext4 ที่ /dev/sda1

- `mkfs -t ext4 /dev/sda1`

# References

- Raw device - Wikipedia: https://en.wikipedia.org/wiki/Raw_device
- Raw Disk - linfo.org: https://www.linfo.org/raw_disk.html
- dev-sda - Baeldung: https://www.baeldung.com/linux/dev-sda
- fdisk(8) - Linux manual page: https://man7.org/linux/man-pages/man8/fdisk.8.html
- FDISK - TechTarget: https://www.techtarget.com/whatis/definition/FDISK
- fdisk Linux Command - Saixiii: https://saixiii.com/fdisk-linux-command/
- How to Use the mkfs Command on Linux - How-To Geek: https://www.howtogeek.com/443342/how-to-use-the-mkfs-command-on-linux/
- mkfs command in Linux with Examples - GeeksforGeeks: https://www.geeksforgeeks.org/mkfs-command-in-linux-with-examples/
- Linux Command: mkfs - Hongkiat: https://www.hongkiat.com/blog/linux-command-mkfs/
