# File-System-3

Computer Organization and Operating System Assignment (Chapter: File System, Sec3)

- [Files and Directories, FHS](https://github.com/65070118/File-System-3?tab=readme-ov-file#files-and-directories-fhs)
- [Raw Media Devices](https://github.com/65070118/File-System-3?tab=readme-ov-file#raw-media-devices)
- [Physical Volume Administration](https://github.com/65070118/File-System-3?tab=readme-ov-file#physical-volume-administration)
- [Volume Group Administration](https://github.com/65070118/File-System-3?tab=readme-ov-file#volume-group-administration)
- [Logical Volume Administration](https://github.com/65070118/File-System-3?tab=readme-ov-file#logical-volume-administration)
- [File System Type](https://github.com/65070118/File-System-3?tab=readme-ov-file#file-system-type)
- [Archiver, Backup/Restore Tools](https://github.com/65070118/File-System-3?tab=readme-ov-file#archiver-backuprestore-tools)
- [Refferences](https://github.com/65070118/File-System-3?tab=readme-ov-file#references)
- [สมาชิก](https://github.com/65070118/File-System-3?tab=readme-ov-file#%E0%B8%AA%E0%B8%A1%E0%B8%B2%E0%B8%8A%E0%B8%B4%E0%B8%81)
# บทคัดย่อ

# Files and Directories, FHS

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

## Mkfs (Make File Systems)

`mkfs` เป็นคำสั่งที่ใช้สำหรับสร้างระบบไฟล์บน Linux

## รูปแบบของคำสั่ง `mkfs`

- `mkfs [ -V ] [ -t fstype ] [ fs-options ] filesys [ blocks ]`

เราสามารถดูระบบไฟล์ที่สามารถสร้างได้โดยการพิมพ์คำสั่ง `mkfs` แล้วกดแท็บสองครั้ง (ไม่ต้องมีเว้นวรรคหลังคำสั่ง `mkfs`)

![ตัวอย่างคำสั่ง Mkfs](https://static1.howtogeekimages.com/wordpress/wp-content/uploads/2019/10/1-5.png?q=50&fit=crop&w=750&dpr=1.5)

## ตัวอย่างคำสั่งสร้างระบบไฟล์ ext4 ที่ /dev/sda1

- `mkfs -t ext4 /dev/sda1`

# Physical Volume Administration

# Volume Group Administration

## Volume group คืออะไร?

Volume group คือ กลุ่มของ physical volumes โดยจะทำการสร้างพูลของพื้นที่บนดิสก์ที่สามารถจัดสรร logical volumes ได้ โดยในแต่ละ volume group จะมีพื้นที่ดิสก์ที่พร้อมใช้งานสำหรับการแบ่ง โดยจะถูกแบ่งออกเป็นส่วนๆ ที่มีขนาดคงที่เรียกว่า extents 

- extents คือ หน่วยพื้นที่ที่เล็กที่สุดที่สามารถจัดสรรได้ ภายใน physical volume โดยสามารถเรียกได้อีกแบบว่า physical extents

logical volume ที่ได้ทำการแบ่งเป็น logical extens จะมีขนาดเท่ากันกับ physical extents ดังนั้นขนาดของ extents จึงเท่ากันทั้งหมดสำหรับ logical volumes ใน volume group โดย volume group จะทำการ maps logical extents กับ physical extents

## การสร้าง Volume Groups

ในการสร้าง volume group ที่ชื่อ `myvg` โดยใช้ physical volumes  `/dev/vdb1` และ `/dev/vdb2` เป็นค่าเริ่มต้น เมื่อใช้ physical volumes ในการสร้าง volume group จะทำการแบ่งพื้นที่บนดิสก์เป็น extents ขนาด 4MB โดยขนาดของ extents นี้คือ ขนาดขั้นต่ำที่สามารถเพิ่มหรือลดขนาด logical volumes ได้ 

โดยขนาดของ extents สามารถแก้ไขได้โดยการเพิ่ม `-s` ไปที่คำสั่ง `vgcreate` โดยจำนวนของ extents ไม่มีผลกระทบต่อการทำงานของ I/O ของ physical volumes และเราสามารถจำกัดจำนวนของ physical หรือ logical volumes โดยทำการเพิ่ม `-p` และ `-l` ไปที่คำสั่ง `vgcreate`

### การเตรียมการเบื้องต้น
- ได้ทำการติดตั้ง package `lvm2` แล้ว
- ได้ทำการสร้าง physical volumes จำนวนตั้งแต่หนึ่งตัวขึ้นไปไว้แล้ว

### ขั้นตอนในการสร้าง
&nbsp;1. สร้าง volume group ที่มีชื่อว่า `myvg` สามารถสร้างได้ดังนี้

- ไม่มีการกำหนดเงื่อนไขใดๆ :
```
# vgcreate myvg /dev/vdb1 /dev/vdb2
 Volume group "myvg" successfully created.
```

- มีการกำหนดขนาดของ extents โดยทำการเพิ่ม `-s` :
```
# vgcreate -s 2 /dev/myvg /dev/vdb1 /dev/vdb2
Volume group "myvg" successfully created.
```

- มีการจำกัดจำนวนของ physical หรือ logical volumes ที่ volume group สามารถมีได้ โดยทำการเพิ่ม `-p` และ `-l` :
```
# vgcreate -l 1 /dev/myvg /dev/vdb1 /dev/vdb2
Volume group "myvg" successfully created.
```

&nbsp;2. เราสามารถดู volume group ที่ถูกสร้างขึ้นมาได้โดยวิธีดังนี้

- ใช้คำสั่ง `vgs` โดยคำสั่งนี้จะทำการแสดงข้อมูลที่สามารถกำหนดค่าได้แบบหนึ่งบรรทัดต่อ volume group :
```
# vgs
  VG    #PV #LV #SN  Attr  VSize   VFree
 myvg   2    0   0   wz-n  159.99g 159.99g
```

- ใช้คำสั่ง `vgdisplay` โดยคำสั่งนี้จะทำการแสดงคุณสมบัติของ volume group เช่น ขนาด extents จำนวนของ physical volumes และตัวเลือกอื่น ๆ ที่ไม่สามารถแ้ไขค่าได้ ตัวอย่างต่อไปนี้คือการแสดงเอาต์พุตของคำสั่ง `vgdisplay` สำหรับ volume group `myvg` หากต้องการแสดง volume group ที่มีอยู่ทั้งหมด ไม่ต้องระบุชื่อของ volume group :
```
# vgdisplay myvg
  --- Volume group ---
  VG Name               myvg
  System ID
  Format                lvm2
  Metadata Areas        4
  Metadata Sequence No  6
  VG Access             read/write
[..]
```

- ใช้คำสั่ง `vgscan`  โดยคำสั่งนี้จะทำการสแกนหา volume group ในอุปกรณ์นี้ :
```
# vgscan
  Found volume group "myvg" using metadata type lvm2
```

&nbsp;3. ทางเลือก: เพิ่มความจุของ volume group โดยทำการเพิ่ม physical volume ตั้งแต่หนึ่งตัวขึ้นไป :
```
# vgextend myvg /dev/vdb3
Physical volume "/dev/vdb3" successfully created.
Volume group "myvg" successfully extended
```

&nbsp;4. เปลี่ยนชื่อของ volume group ที่มีอยู่:
```
# vgrename myvg myvg1
Volume group "myvg" successfully renamed to "myvg1"
```

## การรวม Volume Groups
ในการรวมสอง volume group เข้าด้วยกัน จะใช้คำสั่ง `vgmerge` โดยเราสามารถรวม "source" volume ที่ยังไม่ถูกใช้งาน กับ "destination" volume ที่ยังไม่ถูกใช้งานหรือกำลังใช้งานอยู่ได้ ถ้า physical extents ของ volumes ทั้งสองตัวมีขนาดเท่ากัน จะสรุปได้ว่าทั้ง physical และ logical volumes มีขนาดพอดีกับ destination volume ที่จำกัดเอาไว้

### การเตรียมการเบื้องต้น
- ทำการรวม volume group database ที่ยังไม่ถูกใช้งาน กับ volume group `myvg` ที่ยังไม่ถูกใช้งานหรือกำลังใช้งานอยู่ได้ โดยให้ข้อมูลรันไทม์แบบละเอียด
```
# vgmerge -v myvg databases
```

## การลบ Physical Volumes ออกจาก Volume Group
ในการลบ physical volumes ที่ไม่ได้ถูกใช้ออกจาก volume group จะใช้คำสั่ง `vgreduce` โดยคำสั่ง `vgreduce` จะทำการลดความจุของ volume group โดยทำการลบ physical volumes ที่ไม่ได้ถูกใช้งานตั้งแต่หนึ่งตัวขึ้นไป โดยการใช้คำสั่งนี้จะทำให้ physical volumes ที่ไม่ได้ถูกใช้งานใน volume group นี้ถูกลบออกไป เพือนำไปใช้ใน volume groups อื่น หรือ ถูกลบออกจากระบบไปเลย

### การเตรียมการเบื้องต้น
&nbsp;1. ถ้า physical volume ยังถูกใช้งานอยู่ ให้ทำการย้ายข้อมูลไปยัง physical volume อื่นทีอยู่ใน volume group เดียวกัน :
```
# pvmove /dev/vdb3
  /dev/vdb3: Moved: 2.0%
 ...
  /dev/vdb3: Moved: 79.2%
 ...
  /dev/vdb3: Moved: 100.0%
```

&nbsp;2. ถ้าไม่มี extents ว่างบน physical volume อื่นๆ ใน volume group ที่มีอยู่ ทำขั้นตอนดังนี้ :

&nbsp;&nbsp;&nbsp;i. สร้าง physical volume ใหม่ จาก `/dev/vdb4` :
```
# pvcreate /dev/vdb4
  Physical volume "/dev/vdb4" successfully created
```

&nbsp;&nbsp;&nbsp;ii. เพิ่ม physical volume ใหม่ไปยัง `myvg` volume group :
```
# vgextend myvg /dev/vdb4
  Volume group "myvg" successfully extended
```

&nbsp;&nbsp;&nbsp;iii. เพิ่ม physical volume ใหม่ไปยัง `myvg` volume group :
```
# pvmove /dev/vdb3 /dev/vdb4
  /dev/vdb3: Moved: 33.33%
  /dev/vdb3: Moved: 100.00%
```

&nbsp;3. ลบ physical volume  `dev/vdb3` จาก volume group :
```
# vgreduce myvg /dev/vdb3
Removed "/dev/vdb3" from volume group "myvg"
```

### การตรวจสอบ
- ตรวจสอบว่า `dev/vdb3` physical volume ถูกลบออกจาก `myvg` volume group แล้วหรือยัง :
```
# pvs
  PV           VG    Fmt   Attr   PSize        PFree      Used
  /dev/vdb1 myvg  lvm2   a--    1020.00m    0          1020.00m
  /dev/vdb2 myvg  lvm2   a--    1020.00m    0          1020.00m
  /dev/vdb3   	    lvm2   a--    1020.00m   1008.00m    12.00m
```

## การแบ่ง Volume Group
ถ้า physical volume ที่ยังมีพื่นที่ที่ยังไม่ถูกใช้งานเหลือ สามารถสร้าง volume group ใหม่ได้โดยไม่ต้องเพิ่มดิสก์ใหม่

ในการตั้งค่าเริ่มต้น, volume group ที่ชื่อว่า `myvg` ประกอบไปด้วย `dev/vdb1`, `/dev/vb2` และ `dev/vdb3` หลังจากเตรียมการเสร็จ จะได้  volume group ที่ชื่อว่า `myvg` ประกอบไปด้วย `dev/vdb1`และ `/dev/vb2` และ volume group ที่ชื่อว่า `yourvg` จะประกอบไปด้วย `dev/vdb3`

### ข้อกำหนดเบื้องต้น
- มีพื้นที่เพียงพอภายใน volume group โดยใช้คำสั่ง `vgscan` เพื่อตรวจสอบว่ามีพื้นที่เหลือเท่าไหร่ใน volume group
- จะขึ้นอยู่กับความจุที่ยังไม่ได้ถูกใช้งานใน physical volume ที่มีอยู่ ให้ย้าย physical extents ที่ใช้ทั้งหมดไปยัง physical volume อื่น โดยใช้คำสั่ง `pvmove` 

### การเตรียมการเบื้องต้น
&nbsp;1. แบ่ง volume group ที่ชื่อว่า `myvg` ที่มีอยู่ ไปยัง volume group ใหม่ที่ชื่อว่า `yourvg` :
```
# vgsplit myvg yourvg /dev/vdb3
  Volume group "yourvg" successfully split from "myvg"
```

> [!NOTE]
> ถ้ามี logical volume ถูกสร้างไว้แล้วใช้งานอยู่ใน volume group ที่มีอยู่ ให้ใช้คำสั่งตามนี้เพื่อหยุดการทำงานของ logical volume :
```
# lvchange -a n /dev/myvg/mylv
```

&nbsp;2. ดู attributes ของ volume groups สองตัว:
```
# vgs
  VG     #PV #LV #SN Attr   VSize  VFree
  myvg     2   1   0 wz--n- 34.30G 10.80G
  yourvg   1   0   0 wz--n- 17.15G 17.15G
```

### การตรวจสอบ
- ตรวจสอบว่า volume group ที่สร้างขึ้นมาใหม่ ประกอบด้วย  physical volume ที่ชื่อว่า `dev/vdb3`:
```
# pvs
  PV           VG      Fmt   Attr   PSize        PFree      Used
  /dev/vdb1 myvg   lvm2   a--    1020.00m    0          1020.00m
  /dev/vdb2 myvg   lvm2   a--    1020.00m    0          1020.00m
  /dev/vdb3 yourvg lvm2   a--    1020.00m   1008.00m    12.00m
```

## ย้าย volume group ไปยังระบบอื่น
่เราสามารถย้าย volume group ทั้งตัวไปยังระบบอื่นได้ด้วยคำสั่งดังนี้ :

`vgexport`

&nbsp;&nbsp;&nbsp;ใช้คำสั่งนี้บนระบบที่มีอยู่ เพื่อทำให้ volume group ที่ไม่ได้ใช้งาน ไม่สามารถเข้าถึงระบบได้ เมื่อ volume group ไม่สามารถเข้าถึงในระบบได้ เราสามารถถอด physical volumes ของมันออกได้

`vgimport`
&nbsp;&nbsp;&nbsp;ใช้คำสั่งนี้บนระบบทอื่น เพื่อสร้าง volume group ที่ไม่ได้ใช้งาน ในระบบเก่า สามารถเข้าถึงได้ในระบบใหม่

### ข้อกำหนดเบื้องต้น
- ต้องไม่มีผู้ใช้เข้าถึงไฟล์บน volume group ที่ใช้งานอยู่ใน volume group ที่คุณกำลังจะย้าย

### การเตรียมการเบื้องต้น

&nbsp;1. ทำการถอนการติดตั้ง logical volume ที่ชื่อว่า `mylv` :
```
# umount /dev/mnt/mylv
```

&nbsp;2. ทำการหยุดการใช้งาน logical volume ทั้งหมด ใน volume group :
```
# vgchange -an myvg
vgchange -- volume group "myvg" successfully deactivated
```

&nbsp;3. เอา volume group ออกไปจากระบบ:
```
# vgexport myvg
vgexport -- volume group "myvg" successfully exported
```

&nbsp;4. ดู volume group ที่ถูกนำออก:
```
# pvscan
  PV /dev/sda1    is in exported VG myvg [17.15 GB / 7.15 GB free]
  PV /dev/sdc1    is in exported VG myvg [17.15 GB / 15.15 GB free]
  PV /dev/sdd1   is in exported VG myvg [17.15 GB / 15.15 GB free]
  ...
```

&nbsp;5. ปิดระบบและถอดดิสก์ของ volume group และเชื่อมมันเข้ากับระบบใหม่:

&nbsp;6. เสียบดิสก์เข้าไปในระบบใหม่และ import volume group เพื่อให้สามราถใช้งานได้ในระบบใหม่ได้:
```
# vgimport myvg
```

> [!NOTE]
> เราสามารถเพิ่ม `--force` เข้าไปในคำสั่ง `vgimport` เพื่อทำการ import volume group ที่ physical volumes หายไป ต่อมาให้รันคำสั่ง `vgreduce --removemissing`

&nbsp;7. เปิดใช้งาน volume group:
```
# vgchange -ay myvg
```

&nbsp;8. ติดตั้ง file system เพื่อให้ใช้งานได้:
```
# mkdir -p /mnt/myvg/users
# mount /dev/myvg/users /mnt/myvg/users
```

## ลบ volume group 
เราสามารถลบ volume group ที่มีอยู่แล้ว ด้วยคำสั่ง `vgremove`

### ข้อกำหนดเบื้องต้น
- volume group ต้องไม่มี logical volumes เลย

### การเตรียมการเบื้องต้น
&nbsp;1. หากมี volume group อยู่ในสภาพแวดล้อมแบบคลัสเตอร์ ให้หยุดการล็อคพื้นที่ของ volume group บนโหนดอื่นทั้งหมด โดยใช้คำสั่งต่อไปนี้บนโหนดทั้งหมด ยกเว้นโหนดที่คุณกำลังดำเนินการลบ:
```
# vgchange --lockstop vg-name
```

&nbsp;2. ลบ volume group :
```
# vgremove vg-name
  Volume group "vg-name" successfully removed
```


# Logical Volume Administration

# File System Type

# Archiver, Backup/Restore Tools

# References
### Files and Directories, FHS
### Raw Media Divices

- Raw device - Wikipedia: https://en.wikipedia.org/wiki/Raw_device
- Raw Disk - linfo.org: https://www.linfo.org/raw_disk.html
- dev-sda - Baeldung: https://www.baeldung.com/linux/dev-sda
- fdisk(8) - Linux manual page: https://man7.org/linux/man-pages/man8/fdisk.8.html
- FDISK - TechTarget: https://www.techtarget.com/whatis/definition/FDISK
- How to Use the mkfs Command on Linux - How-To Geek: https://www.howtogeek.com/443342/how-to-use-the-mkfs-command-on-linux/
- mkfs command in Linux with Examples - GeeksforGeeks: https://www.geeksforgeeks.org/mkfs-command-in-linux-with-examples/
- Linux Command: mkfs - Hongkiat: https://www.hongkiat.com/blog/linux-command-mkfs/

### Physical Volume Administration
### Volume Group Administration
- Managing LVM volume groups: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9/html/configuring_and_managing_logical_volumes/managing-lvm-volume-groups_configuring-and-managing-logical-volumes#creating-lvm-volume-group_managing-lvm-volume-groups
### Logical Volume Administration
### File System Type
### Archiver, Backup/Restore Tools

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

