# Physical Volume

คือ ที่เก็บข้อมูลทางกายภาพใดๆ เช่น Hard disk หรือ Disk Partition ที่อยู่ในชั้นล่างสุดของ Logical Volume Management(LVM)

![ตัวอย่าง](https://cdn.thegeekdiary.com/wp-content/uploads/2014/10/LVM-basic-structure.png)

หากเราต้องการใช้ Hard disk ทั้งก้อนในการสร้าง Physical Volume ภายใน Disk ต้องไม่มีตาราง Partition อยู่ หากมี Partition table อยู่ต้องทำการลบด้วยคำสั่ง `wipefs -a /dev/vdb1` เป็นคำสั่งหลัก ซึ่งการทำคำสั่งนี้จะทำการลบข้อมูลทั้งหมดใน Disk และสำหรับ DOS Disk Partition ให้ id ของ Partition ต้องเซ็ดเป็น 0x8e โดยการใช้ คำสั่ง `fdisk` หรือ `cfdisk` หรือเทียบเท่า 

>[!NOTE]
> Partition คือ การแบ่งพื้นที่ออกเป็นส่วนๆ
>

# การสร้าง Physical Volume

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
    - `pvscan` เป็นคำสั่งที่จะแสกน อุปกรณ์ LVM block ที่รองรับทั้งหมด ใน Physical Volume โดยสามารถกำหนดตัวกรองในไฟล์ `lvm.conf` เพื่อที่คำสั่งนี้จะหลีกเลี่ยงการแสกน Physical Volume ที่เฉพาะได้

        ```
        # pvscan
          PV  /dev/vdb1                      lvm2 [1.00 GiB]
          PV  /dev/vdb2                      lvm2 [1.00 GiB]
          PV  /dev/vdb3                      lvm2 [1.00 GiB]
        ```

## References
https://access.redhat.com/documentation/th-th/red_hat_enterprise_linux/8/html/configuring_and_managing_logical_volumes/managing-lvm-physical-volumes_configuring-and-managing-logical-volumes

