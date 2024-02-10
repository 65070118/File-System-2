# Physical Volume

คือ หน่วยเก็บข้อมูล Physical storage unit ของ LVM logical Volume  เป็น block device ที่เก็บข้อมูลทางกายภาพใดๆ เช่น Hard disk หรือ Disk Partition ที่อยู่ในชั้นล่างสุดของ Logical Volume Management(LVM)

![ตัวอย่าง](https://cdn.thegeekdiary.com/wp-content/uploads/2014/10/LVM-basic-structure.png)

หากเราต้องการใช้ Hard disk ทั้งก้อนในการสร้าง Physical Volume ภายใน Disk ต้องไม่มีตาราง Partition อยู่ หากมี Partition table อยู่ต้องทำการลบด้วยคำสั่ง `wipefs -a /dev/vdb1` เป็นคำสั่งหลัก ซึ่งการทำคำสั่งนี้จะทำการลบข้อมูลทั้งหมดใน Disk และสำหรับ DOS Disk Partition ให้ id ของ Partition ต้องเซ็ดเป็น 0x8e โดยการใช้ คำสั่ง `fdisk` หรือ `cfdisk` หรือเทียบเท่า 

-  Partition คือ การแบ่งพื้นที่ออกเป็นส่วนๆ

>[!NOTE]
> ## สิ่งที่น่าสนใจ
>
> ### ข้อจำกัดในการสร้าง Physical Volume
>
> - Physical Volume สามารถมีได้ 1 ถึง 32 ต่อ 1 Volume group
> - Physical Volume สามารถมีได้ 1 ถึง 128 ต่อ 1 Big Volume group
> - ขนาดของ Partition ถูกจำกัดไว้ที่ 2 ยกกำลัง n bytes ลำหรับ 20 <= n <= 30
> - ขนาดของ Physical block ถูกจำกัดไว้ที่ 512 หรือ 4096 bytes

# เค้าโครงของ Physical Volume

![ตัวอย่าง](https://access.redhat.com/webassets/avalon/d/Red_Hat_Enterprise_Linux-9-Configuring_and_managing_logical_volumes-en-US/images/8a27e9aae1f828bbdd43c54090d5ab15/physical-volume-layout.png)

### LVM Label : 

ช่วยในการระบุอุปกรณ์ว่าเป็น Physical Volume ของ LVM 

ตามค่าเริ่มต้น LVM Label จะอยู่ใน secter 512-bytes ที่ 2 หรือ สามารถเลือกวาง label นี้ ใน 4 secter แรกก็ได้ เพราะว่า LVM tools จะทำการแสกนหา LVM Label ใน 4 secter แรก และ Physical Volume Label จะขึ้นต้นด้วยสตริง LABELONE
  Physical Volume Label จะประกอบไปด้วย :

- Physical Volume UUID

- ขนาดของ block device เป็น bytes

- รายการตำแหน่งของพื้นที่ข้อมูลที่สิ้นสุดด้วยค่า NULL

- รายการตำแหน่งพื้นที่ข้อมูล metadataที่สิ้นสุดด้วยค่า NULL

### Metadata : 

เก็บข้อมูลการกำหนดค่าของ Volume group ที่มีอยู่ในระบบ โดยจะมีการสร้างสำเนาข้อมูล metadata เอาไว้ทุกๆ Metadata Area บน ทุกๆ Physical Volume ภายใน Volume group

### Usable space : 

พื้นที่ที่ใช้งานได้

# การสร้าง Physical Volume

เมื่อต้องการเพิ่มอุปกรณ์เก็บข้อมูล เข้าสู่ระบบ LVM เพื่อให้สามารถจัดการพื้นที่เก็บข้อมูลได้อย่างยืดหยุ่น เราจะต้องสร้าง Physical Value โดยใช้คำสั่งที่เกี่ยวข้องกับ LVM เพื่อให้ระบบ LVM รู้ว่าอุปกรณ์เหล่านั้นเป็นส่วนหนึ่งของ LVM.

ก่อนหน้านั้นเราสามารถกำหนด ใช้ Hard disk ได้ทั้งก้อน คือ `/dev/vdb` หรือ ทำเป็น Partition คือ `/dev/vdb1 /dev/vdb2`

## ข้อกำหนดเบื้องต้น
ต้องทำการดาวน์โหลกแพคเกจ `lvm2` ก่อน ด้วยคำสั่ง `sudo apt-get install lvm2`

## ขั้นตอนการสร้าง

1. สร้าง Physical Volumes ที่มีชื่อว่า vdb ด้วยคำสั่ง `pvcreate` :

    - สร้างแบบ Hard Disk ทั้งก้อน : 
    
        ```
        # pvcreate /dev/vdb
          Physical volume "/dev/vdb" successfully created.
        ```

    - สร้างแบบ Partition :

        ```
        # pvcreate /dev/vdb1 /dev/vdb2 /dev/vdb3
        Physical volume "/dev/vdb1" successfully created.
        Physical volume "/dev/vdb2" successfully created.
        Physical volume "/dev/vdb3" successfully created.
        ```
2. สามารถดู Physical Volumes ที่ถูกสร้างขึ้นด้วยคำสั่ง ต่อไปนี้ :

    - `pvdisplay` เป็นคำสั่งที่จะ Output ข้อมูล           แบบหลายบรรทัดโดยละเอียดซึ่งจะแสดงข้อมูลคุณสมบัติสำหรับ Physical Volume แต่ละตัว เช่น ขนาด, extends, ชื่อ Volume group และตัวเลือกอื่นๆ :

        ```
        # pvdisplay
        --- NEW Physical volume ---
        PV Name               /dev/vdb1
        VG Name
        PV Size               1.00 GiB
        [..]
        --- NEW Physical volume ---
        PV Name               /dev/vdb2
        VG Name
        PV Size               1.00 GiB
        [..]
        --- NEW Physical volume ---
        PV Name               /dev/vdb3
        VG Name
        PV Size               1.00 GiB
        [..]
        ```

    - `pvs` เป็นคำสั่งที่จะให้ข้อมูล Physical Volume ในรูปแบบที่กำหนดค่าได้ ซึ่งจะแสดงแต่ละ Physical Volume ต่อ 1 แถว : 

        ```
        # pvs
          PV         VG  Fmt    Attr    PSize      PFree
        /dev/vdb1        lvm2           1020.00m   0
        /dev/vdb2        lvm2           1020.00m   0
        /dev/vdb3        lvm2           1020.00m   0
        ```
    - `pvscan` เป็นคำสั่งที่จะแสกน อุปกรณ์ LVM block ที่รองรับทั้งหมด ใน Physical Volume โดยสามารถกำหนดตัวกรองในไฟล์ `lvm.conf` เพื่อที่คำสั่งนี้จะหลีกเลี่ยงการแสกน Physical Volume ที่เฉพาะได้ :

        ```
        # pvscan
          PV  /dev/vdb1                      lvm2 [1.00 GiB]
          PV  /dev/vdb2                      lvm2 [1.00 GiB]
          PV  /dev/vdb3                      lvm2 [1.00 GiB]
        ```
# การลบ Physical Volume

ถ้าหากว่าอุปกรณ์ไม่จำเป็นต้องใช้อีกต่อไป เราสามารถลบ label LVM ออก ได้ด้วยคำสั่ง `pvremove` ที่ทำให้ข้อมูล metadata LVM เป็น 0 บน Physical Volume ว่าง

## ขั้นตอนการลบ

1. ลบ Physical Volume ด้วยคำสั่ง `pvremove`:

        ```
        # pvremove /dev/vdb3
        Labels on physical volume "/dev/vdb3" successfully wiped.
        ```

2. ตรวจสอบว่าลบออกไปแล้วหรือยัง และดู Physical Volume ที่มีอยู่ด้วยคำสั่ง `pvs` :

        ```
        # pvs
          PV         VG   Fmt    Attr    PSize      PFree
          /dev/vdb1  	    lvm2           1020.00m   0
          /dev/vdb2  	    lvm2           1020.00m   0
        ```


## References

- https://www.ibm.com/docs/es/aix/7.1?topic=subsystem-physical-volumes

- https://www.redhat.com/sysadmin/create-physical-volume

- https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/logical_volume_manager_administration/index
