
# File System Type
อย่างที่ทราบกัน **file system** มีหน้าที่จัดการข้อมูลบนอุปกรณ์ที่จัดเก็บข้อมูล เช่น  hard drives, SSDs และ USB drives  ต่อมาสิ่งที่สำคัญเลยก็คือ เราต้องทำความเข้าใจเกี่ยวกับระบบไฟล์ ว่าระบบไฟล์นั้นใช้ประเภทใดบ้างและมีลักษณะอย่างไร ซึ่งในส่วนนี้เราจะเรียนรู้เกี่ยวกับประเภทรูปแบบระบบไฟล์หลักๆ ที่สำคัญทั้งหมดบน Linux

![Type](https://static.javatpoint.com/linux/images/linux-file-system2.png)

อ้างอิงรูปภาพจาก https://www.javatpoint.com/linux-file-system



# ประเภทของระบบไฟล์ที่ใช้ใน Linux (File System Type) 

### 1.	Ext, Ext2, Ext3 and Ext4 file system

ระบบไฟล์ **Ext** ย่อมาจาก **Extended File System** ประเภทระบบไฟล์แรก รองรับ Partition สูงสุด 2GB แต่กลับมีปัญหาเกี่ยวกับการประทับเวลาในระบบไฟล์ ทำให้ไม่ได้ใช้งานต่อ

ต่อมาได้พัฒนามาเป็น **Ext2** ซึ่งยังคงโครงสร้างภายในไว้ ทำให้สามารถปรับปรุงการทำงานของระบบได้ เหมาะสำหรับ Flash drive, USB drive 

| File Systems | ขนาดไฟล์ size สูงสุด     | พื้นที่เก็บสูงสุด              |
| :-------- | :------- | :------------------------- |
| `Ext2` | 16GB - 2TB | 2TB - 32TB |


*ข้อจำกัด Ext2 เป็นระบบไฟล์ที่ยังไม่มีการทำ **Journaling feature***


>[!Note] 
>**Journaling** เป็นเทคนิคที่ใช้บันทึกการเปลี่ยนแปลงของข้อมูลในระบบไฟล์
>เมื่อระบบไฟล์พังขึ้นมา เช่น ไฟดับ ก็ยังสามารถกู้ข้อมูลกลับมาได้ เพราะว่ามีการบันทึกเอาไว้ว่าไฟล์ไหนถูกแก้ไขไปบ้างก่อนที่ระบบจะล่ม 

และต่อมาคือ **Ext3** ได้มีการทำ **Journaling** ช่วยป้องกันข้อมูลที่สูญหาย เมื่อระบบล่ม ส่วนขนาดไฟล์และขนาดพื้นที่รวมยังคงเท่า Ext2 เรียกได้ว่าเป็นเวอร์ชันอัปเกรดของ Ext2

**Ext4** เป็นระบบไฟล์ที่เร็วที่สุดในกลุ่มระบบไฟล์ Ext ทั้งหมด ด้านประสิทธิภาพดีขึ้น ยังสามารถเข้ากันได้ดีกับ SSD รองรับไฟล์ขและพื้นที่เก็บไฟล์ขนาดใหญ่ ยังสามารถใช้กับรุ่นก่อนหน้าอย่าง Ext2 และ Ext3 ได้

| File Systems | ขนาดไฟล์ size สูงสุด     | พื้นที่เก็บสูงสุด              |
| :-------- | :------- | :------------------------- |
| `Ext4` | 16GB - 16TB | 1 EB (exabyte) |


### 2. JFS (Journaled File System)
ได้รับการพัฒนาโดย **IBM** สามารถใช้แทน Ext4 ได้ เป็นระบบไฟล์ที่มี **Journaling** อีกด้วย คือ ป้องกันข้อมูลที่สูญหาย ในตอนที่เกิดการขัดข้องหรือระบบล่ม

### 3. ReiserFS
ระบบไฟล์นี้ถูกนำเสนอเป็นทางเลือกสามารถใช้แทนระบบไฟล์ Ext3 ได้ มีประสิทธิภาพที่ดีกว่า มีการจัดเก็บไฟล์แบบ B+Tree ช่วยลดเวลาในการค้นหาและอัปเดตไดเรกทอรี

### 4.	XFS file system
เป็นระบบไฟล์เริ่มต้นใน Red Hat Enterprise Linux 7 เหมาะสำหรับการจัดการไฟล์และพื้นที่จัดเก็บข้อมูลที่มีขนาดใหญ่ ซึ่งพัฒนาขึ้นสำหรับการประมวลผล I/O แบบขนาน และยังการทำ Journaling ช่วยในการปกป้องและกู้ข้อมูล

*ข้อจำกัด ไม่มีการ checksum ทำให้เสี่ยงที่บางไฟล์ข้อมูลจะเสียหาย*

>[!Note] 
>checksum คือการตรวจสอบความถูกต้องของข้อมูล 

### 5.Btrfs (B-tree FS)
ระบบไฟล์ที่ถูกออกแบบมาเพื่อแก้ไขข้อจำกัดของระบบไฟล์รุ่นเก่า ซึ่งจะมุ่งเน้นไปที่การจัดการกับข้อผิดพลาด การซ่อมแซม และการดูแลระบบ รวมไปถึงการจัดการที่เก็บข้อมูลขนาดใหญ่ 
-	มีการทำ checksum ตรวจสอบข้อมูล เพื่อหาข้อผิดพลาดที่เกิดขึ้น
-	snapshots  สำหรับการสำรองและกู้คืนระบบ

### 6. ZFS (Z File System)
ระบบไฟล์ที่รองรับอุปกรณ์เก็บข้อมูลได้หลายตัว สามารถจัดการข้อมูลขั้นสูง 
- การทำงานแบบ Copy-on-Write
- Snapshot ที่ช่วยในการสร้างสำเนาของข้อมูล โดยการบันทึกสถานะปัจจุบันของข้อมูล 
- built-in RAID ในตัว


# หลักการทำงาน File System
![Type](https://lh3.googleusercontent.com/UhXSGNFoV6RHiFAtUb7vIVoO0GovE4cqAp2H7aMBNVklxQrxSXG8toYWMJa1D-P7BOknqBIIORJbhR5wQGuuwNq917haZlT10g8qW56NC4l7FpXAuEaGvdhpWsSpJptukT12H0hFzLJnjGAC73_TjQWG1AWrx9dWF7sL_ebT7nWCydzNOL4p6aJuyA)

อ้างอิงรูปภาพจาก https://www.enablegeek.com/tutorial/work-file-system-in-linux/

ส่วนต่อมาเราจะพูดถึงหลักการทำงานของระบบไฟล์ Linux จากรูปข้างบน สิ่งที่ทำให้แอปพลิเคชันสามารถทำงานกับระบบไฟล์ได้ จะมี 2 ส่วนคือ **kernel** ทำให้ผู้ใช้สามารถติดต่อกับระบบไฟล์ได้ 

และในส่วนของ **Linux virtual file system** คือส่วนที่จัดการกับการจัดเก็บข้อมูลบนอุปกรณ์เก็บข้อมูล เช่น ฮาร์ดดิสก์ หรือ SSD และให้การเข้าถึงข้อมูลแก่แอปพลิเคชันผู้ใช้ โดยใช้ **API (Application Programming Interface)** เป็นช่องทางที่ให้ผู้ใช้และโปรแกรมสามารถเข้าถึงและจัดการกับไฟล์ไดเรกทอรีได้ 

นอกจากนี้ใน **virtual file system** ยังมีไดรเวอร์ระบบที่เป็นตัวกลางที่ช่วยให้สามารถเชื่อมต่อกับระบบไฟล์ที่แตกต่างกันได้ เช่น ext4, xfs, ntfs

# Commands for File System
### การสร้างระบบไฟล์ใหม่บน Partition
สำหรับการสร้างระบบไฟล์บน Partition หรืออุปกรณ์จัดเก็บข้อมูลในระบบ**จะใช้คำสั่ง `mkfs`**

**ตัวอย่างเช่น** สร้างระบบไฟล์ Ext4 

```
  sudo mkfs.ext4 /dev/sdX1
```
ในที่นี่ **/dev/sdX1**  คือ อุปกรณ์หรือPartitionของเรา

### การเชื่อมต่อระบบไฟล์อุปกรณ์ไปยัง Directory
หลังจากเราสร้างระบบไฟล์ Ext4 บนอุปกรณ์หรือ Partition แล้ว เราต้องทำการ mount (เชื่อมต่อ) ระบบไฟล์นั้นไปยังโฟลเดอร์ในระบบ เพื่อให้เราสามารถเข้าถึงและใช้งานไฟล์บนระบบไฟล์ Ext4 นั้นได้ **จะใช้คำสั่ง `mount`**

**ตัวอย่างเช่น**

```
  sudo mount /dev/sdX1 /mnt/ext4_partition
```
จากคำสั่งข้างบน เราทำการ mount ระบบไฟล์ Ext4 ไปยังโฟลเดอร์ /mnt/ext4_partition

### การตรวจสอบและซ่อมแซมระบบไฟล์
**`fsck`** เป็นคำสั่งสำหรับตรวจสอบและซ่อมแซมข้อผิดพลาดในระบบไฟล์

การตรวจสอบ fsck นี้สามารถทำได้โดยอัตโนมัติตั้งแต่เริ่ม boot เครื่อง

### ตรวจสอบสถานะ File system และ Partition 


**ตัวอย่างเช่น**

```
  fsck /dev/sdb
```
จากคำสั่งข้างบน ตรวจสอบระบบไฟล์บนอุปกรณ์ /dev/sdb

ถ้า**ไม่พบปัญหา** หรือไม่มีอะไรผิดพลาด จะแสดงผลลัพธ์แบบนี้

```
  $ fsck from util-linux 2.36.1
  e2fsck 1.46.2 (28-Feb-2021)
  /dev/sdb: clean, x/y files, z/z blocks
```

แต่ถ้า**พบปัญหา** หรือมีข้อผิดพลาด จะแสดงผลลัพธ์แบบนี้

```
  $ fsck /dev/sda
  fsck from util-linux 2.20.1
  fsck: fsck.ntfs: not found
  fsck: error 2 while executing fsck.ntfs for /dev/sda
```

มันจะส่งคืน exit code ออกมา สามารถดูคำอธิบาย exit code ได้ **โดยใช้ `man fsck`**

```
0 – No errors
1 – Filesystem errors corrected
2 – System should be rebooted
4 – Filesystem errors left uncorrected
8 – Operational error
16 – Usage or syntax error
32 – Fsck canceled by user request
128 – Shared-library error
```

### ตรวจสอบ file system ทั้งหมด
`option -A` ตรวจสอบและซ่อมแซมทระบบไฟล์ทั้งหมดใน /etc/fstab

```
  $ fsck -AR -y
  fsck from util-linux 2.20.1
  e2fsck 1.42 (29-Nov-2011)
  /dev/sda6: clean, 95/2240224 files, 3793506/4476416 blocks
  dosfsck 3.0.12, 29 Oct 2011, FAT32, LFN
  /dev/sda7: 8 files, 50/1463400 clusters
```


### ตรวจสอบเฉพาะประเภทระบบไฟล์ที่ต้องการ
`option -t` สำหรับระบุประเภทของระบบไฟล์ที่ต้องการตรวจสอบและซ่อมแซม

```
  $ fsck -AR -t ext4 /dev/sda1
  fsck from util-linux 2.36.1
  e2fsck 1.46.2 (28-Feb-2021)
  /dev/sda1: clean, 12345/56789 files, 98765/43210 blocks

```

### แก้ไขปัญหาหลังจากตรวจสอบแล้ว
`option -y ` ให้ fsck แก้ไขข้อผิดพลาดให้

```
  $ fsck -y /dev/sda6
  fsck from util-linux 2.20.1
  e2fsck 1.42 (29-Nov-2011)
  /dev/sda6 contains a file system with errors, check forced.
  Pass 1: Checking inodes, blocks, and sizes
  Inode 2060353 is a unknown file type with mode 0137642 but it looks 
  like it is really a directory.
  Fix? yes

  Pass 2: Checking directory structure
  Entry 'test' in / (2) has deleted/unused inode 49059.  Clear? yes

  Pass 3: Checking directory connectivity
  Pass 4: Checking reference counts
  Pass 5: Checking group summary information

  /dev/sda6: ***** FILE SYSTEM WAS MODIFIED *****
  /dev/sda6: 96/2240224 files (7.3% non-contiguous), 3793508/4476416 blocks

```

### Command Options 


```
-A  สำหรับตรวจสอบระบบไฟล์ทั้งหมดใน /etc/fstab
-C  แสดงแถบความคืบหน้า
-l  ล็อคอุปกรณ์ เพื่อไม่ให้มีการใช้งานระหว่างตรวจสอบ
-M  ป้องกันการตรวจสอบไฟล์ระบบ ที่ทำ mount
-N  ตรวจสอบและแสดงเกี่ยวกับปัญหาที่พบ แต่ไม่ทำการซ่อมแชม
-R  ไม่ให้ตรวจสอบ root File System มีประโยชน์เฉพาะกับ -A
-r  ระบุสถิติของแต่ละอุปกรณ์ที่ตรวจสอบ
-t  ระบุเฉพาะประเภทระบบไฟล์ Linux ที่จะตรวจสอบ
-V  แสดงเวอร์ชั่นของ fsck

```

>[!Note]
>## สิ่งที่น่าสนใจเพิ่มเติม
>นอกจากประเภทระบบไฟล์ที่กล่าวมาข้างบน Linux ยังรองรับการใช้งานระบบไฟล์ของระบบปฏิบัติการอื่น ๆ ได้อีกด้วย เช่น NTFS และ exFAT พัฒนาโดย Microsoft
>สามารถเก็บข้อมูลและรองรับกับฮาร์ดแวร์ที่มีขนาดใหญ่มากได้
>แต่มีข้อจำกัดที่เกี่ยวกับการรับรองสิทธิ์ Unix มาตรฐานและความปลอดภัย



# Reference
- https://www.easeus.com/computer-instruction/linux-file-system-type.html
- https://www.geeksforgeeks.org/linux-file-system/
- https://www.javatpoint.com/linux-file-system
- https://kridsana.cmtc.ac.th/load/Linux/unix02-Install.pdf
- https://www.acisonline.net/?p=8786
- https://www.tecmint.com/find-linux-filesystem-type/
- https://saixiii.com/fsck-linux-command/
- https://th.linux-console.net/?p=182
- https://blog.desdelinux.net/th/sistemas-archivos-cual-elegir-discos-particiones-linux/

