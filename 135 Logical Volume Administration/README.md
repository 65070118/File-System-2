# Logical Volume Administration



ระบบจัดการพื้นที่เก็บข้อมูลบน Linux การแบ่งมาจาก Volume Group ช่วยให้จัดสรรพื้นที่ได้อย่างยึดยุ่นซึ่งมีความสามารถรองรับการทำ RAID Mirroring และ Snapshot

### ข้อดีของการใช้ Logical Volume (LV)
- 1. ความยืดหยุ่นในการจัดเก็บข้อมูล:

LV สามารถรวมฮาร์ดดิสก์และพาร์ติชันหลาย ๆ อันเข้าเป็น Logical Volume เดียว ทำให้ระบบแฟ้มครอบคลุมพื้นที่จัดเก็บข้อมูลได้หลายดิสก์


- 2. การย้ายข้อมูลแบบออนไลน์:

สามารถย้ายข้อมูลระหว่าง Logical Volume ต่าง ๆ ขณะที่ระบบยังทำงานอยู่ เพื่อเปลี่ยนไปใช้ระบบจัดเก็บข้อมูลใหม่ที่เร็วกว่า ทนทานกว่า
สามารถจัดเรียงข้อมูลบนดิสก์ใหม่ได้ขณะที่ดิสก์ยังใช้งานอยู่ ตัวอย่างเช่น ย้ายข้อมูลออกจากดิสก์แบบ Hot-swappable ก่อนถอดดิสก์ออก
- 3. Convenient device naming:

Logical Volume สามารถจัดกลุ่มตามผู้ใช้กำหนด และตั้งชื่อตามความสะดวก
- 4. Disk Striping:

สร้าง Logical Volume ที่แบ่งข้อมูลออกเป็นแถบ (stripe) แล้วกระจายไปยังหลายดิสก์ ช่วยเพิ่มประสิทธิภาพการอ่าน/เขียนข้อมูลได้อย่างมาก
- 5. Mirroring Volume:

สร้างสำเนาข้อมูล (mirror) บน Logical Volume เพิ่มความทนทานของข้อมูล
- 6. Volume Snapshots:

สร้างสำเนาของ Logical Volume เพื่อใช้เป็นข้อมูลสำรอง หรือทดสอบการเปลี่ยนแปลงข้อมูลโดยไม่กระทบข้อมูลจริง

- 7. Resizeable storage pools
สามารถขยายขนาดหรือลดขนาด Logical Volume ได้ง่ายด้วยคำสั่งซอฟต์แวร์ ไม่จำเป็นต้องฟอร์แมตหรือแบ่งพาร์ติชันใหม่บนฮาร์ดดิสก์

### LVM Component

- 1. Physical Volumes
หน่วยพื้นฐานของ LV คือ Physical Volume (PV) ซึ่งสามารถอุปกรณ์เก็บข้อมูล Hdd sdd นำมาแบ่งเป็น label ชื่ออุปกรณ์หรือชื่อพาติชั่น

- 2. Volume Groups
คือ Physical Volume (PV) อันหรือมากกว่าหนึ่งจะถูกแบ่งเป็น volume Groups ให้ LVM จัดการ

- 3.  LVM Logical Volumes
LVM ทำการแบ่ง Volume Groups(VG) ออกแบ่งเชิงตรรถะเพื่อให้งานต้องการจัดการไฟล์

### ประเภทของการ LVM
-   Linear Volume: รวบรวมพื้นที่ว่างจากหลาย Physical Volume มาสร้างเป็น Logical Volume ขนาดใหญ่
-   Striped Volume: แบ่ง Logical Volume ออกเป็นส่วนเล็กๆ แล้วกระจายข้อมูลไปเขียนยัง Physical Volume ต่างๆ เพิ่มประสิทธิภาพในการอ่านเขียนข้อมูลขนาดใหญ่
-   RAID Volume: ใช้เทคโนโลยี RAID ในการสร้าง Logical Volume เพิ่มความทนทานของข้อมูล
-   Thinly-Provisioned Logical Volume (Thin Volume): สร้าง Logical Volume ที่มีขนาดใหญ่กว่าพื้นที่จัดเก็บข้อมูลจริง ใช้พื้นที่อย่างคุ้มค่า
-   Snapshot Volume: สร้างสำเนาเสมือนของ Logical Volume ณ เวลารนั้นโดยไม่กระทบการใช้งาน ช่วยในการสำรองข้อมูลหรือทดสอบระบบ
-   Thinly-Provisioned Snapshot Volume: ประเภทพิเศษของ Snapshot Volume ที่ใช้เทคนิค Thin Provisioning ช่วยประหยัดพื้นที่จัดเก็บข้อมูล

### การแสดง Logical Volumes
```
lvscan
```
![lvsan](/135%20Logical%20Volume%20Administration/pic/lvscan.png)

```
lvdisplay
```
![lvdisplay](/135%20Logical%20Volume%20Administration/pic/lvdisplay.png)


### การสร้าง Logical Volumes
	ในการสร้าง lv นั้นต้องสร้างผ่าน vg ด้วยคำสั่ง
```
lvcreate --name $name  --size xxg $nameVG

```
![lvcreate](/135%20Logical%20Volume%20Administration/pic/lvcreate.png)

### การ Setup Logical Volumes
```
mkfs.ntfs -QIL $name /dev/$nameVG/$namelvm
```
![lvmkfs](/135%20Logical%20Volume%20Administration/pic/mkfs.ntfs.png)

ในการ setup เราต้องสร้างระบบไฟล์ขึ้นมาในตัวอย่างนี้แบบระบบไฟล์ ntfs

```
    mount /dev/VG/LVM /dir
```

![lvmount](/135%20Logical%20Volume%20Administration/pic/mount.png)

สร้าง directory และใช้การ mount เพื่อให้ lvm เชื่อมกับ directory นั้น

```
    df -h .
```
![lvmount2](/135%20Logical%20Volume%20Administration/pic/showresulfmount.png)

แสดงรายละเอียด File System






### การเพิ่มขนาด Logical Volumes
```
lvextend -L +10G my-logical-volume
```
![lvextend](/135%20Logical%20Volume%20Administration/pic/extend.png)

### การลดขนาด Logical Volumes
```
lvreduce -L -10G my-volume
```
![lvextend](/135%20Logical%20Volume%20Administration/pic/lvreduce.png)


### การ Resize Logical Volumes


ทำการ umount แล้วทำการ resizefs เพื่อให้ไฟล์แสดงผลถูกต้องกับ logical volume

```
nrfsresize /dev/nameVG/nameLV
```
![lvextend](/135%20Logical%20Volume%20Administration/pic/resize.png)

### LVM Mirror

LVM Mirror เป็นเทคนิคการสำรองข้อมูล Logical Volume (LV) โดยทำสำเนาข้อมูลไปยัง Physical Volume (PV) หลายตัว ช่วยให้มั่นใจว่าข้อมูลจะยังคงอยู่แม้ว่า PV ตัวใดตัวหนึ่งจะเสียหาย

ข้อดี:

-   เพิ่มความทนทานของข้อมูล:  ข้อมูลจะยังคงอยู่แม้ว่า PV ตัวใดตัวหนึ่งจะเสียหาย
-   เพิ่มประสิทธิภาพการอ่านข้อมูล:  ข้อมูลสามารถอ่านจาก PV ทั้งหมดพร้อมกัน
-   ง่ายต่อการจัดการ: LVM จัดการการสำรองข้อมูลโดยอัตโนมัติ

ข้อเสีย:

-   พื้นที่จัดเก็บข้อมูล:  พื้นที่จัดเก็บข้อมูลจะถูกใช้สองเท่า
-   ต้นทุน: PV เพิ่มเติมมีค่าใช้จ่ายเพิ่มเติม

#### ตัวอย่างการ LVM Mirror
ในตัวอย่างนี้จะใช้ sdb2 sbc สร้าง VG ขึ้นมาชื่อว่า vgm
![lvextend](/135%20Logical%20Volume%20Administration/pic/createvgformirror.png)
```
lvcreate –size 500m -n testmir -m 1 vgm
```
![lvextend2](/135%20Logical%20Volume%20Administration/pic/createmirror.png)

ใช้คำสั่งเพื่อสร้าง lvcreate –size 500m -n testmir -m 1 vgm

-  -size 300m: กำหนดขนาด LV ใหม่เป็น 500MB

-  -n lvmir: การตั้งชื่อ LV ใหม่ว่า "testmir"

-  -m 1: ระบุอัลกอริทึมการกระจายข้อมูลแบบ striping

-  vgm: ระบุ Volume Group (VG) ที่จะสร้าง LV ใหม่



### LVM Snapshot

```
lvcreate -L100M -s -n snaptest2 dev/vgm/mos_vg2
```
![lvextend](/135%20Logical%20Volume%20Administration/pic/lvcreatesnapshot.png)

-  -L100M: กำหนดขนาด Snapshot LV ใหม่เป็น 100MB

-  -s: สร้าง Snapshot LV โดยใช้พื้นที่ว่างที่มีอยู่บน Logical Volume ต้นทาง

-  -n snapLV: ตั้งชื่อ Snapshot LV ใหม่ว่า "snaptest2"

-  dev/vgm/mos_vg2: ระบุ Logical Volume ต้นทาง


### Remove Logical Volumes
```
lvremove vgname/lvname
```


### Rename Logical Volumes

```
lvrename vgname/oldnamelv vgname/newnamelv
```


## Reference

- [Linuxredhat](https://access.redhat.com/documentation/th-th/red_hat_enterprise_linux/6/html/logical_volume_manager_administration/index)
- [youtubebulemonkey](https://www.youtube.com/watch?v=JlWiNnuMm_4)
- [linux-training](https://linux-training.be/linuxsto.pdf)
- [medium](https://medium.com/@songyotemungmai/%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B9%83%E0%B8%8A%E0%B9%89%E0%B8%87%E0%B8%B2%E0%B8%99-lvm-%E0%B9%80%E0%B8%9E%E0%B8%B7%E0%B9%88%E0%B8%AD%E0%B8%88%E0%B8%B1%E0%B8%94%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B8%9E%E0%B8%B7%E0%B9%89%E0%B8%99%E0%B8%97%E0%B8%B5%E0%B9%88%E0%B8%AE%E0%B8%B2%E0%B8%A3%E0%B9%8C%E0%B8%94%E0%B8%94%E0%B8%B4%E0%B8%AA%E0%B8%81%E0%B9%8C-6c127b24ef87)
