# FileSystem-3

Computer Organization and Operating System Assignment (Chapter: File System, Sec3)

- [Files and Directories, FHS](https://github.com/65070118/File-System-3/tree/main/160%20Files%20and%20Directories%2C%20FHS)
- [Raw Media Devices](https://github.com/65070118/File-System-3/tree/main/118%20Raw%20Media%20Devices)
- [Physical Volume Administration](https://github.com/65070118/File-System-3/tree/main/177%20Physical%20Volume%20Administration)
- [Volume Group Administration](https://github.com/65070118/File-System-3/tree/main/129%20Volume%20Group%20Administration)
- [Logical Volume Administration](https://github.com/65070118/File-System-3/tree/main/135%20Logical%20Volume%20Administration)
- [File System Type](https://github.com/65070118/File-System-3/tree/main/117%20File%20System%20Type)
- [Archiver, Backup/Restore Tools](https://github.com/65070118/File-System-3/tree/main/136%20Archiver%2C%20Backup_Restore%20Tools)
- [สมาชิก](https://github.com/65070118/File-System-3?tab=readme-ov-file#%E0%B8%AA%E0%B8%A1%E0%B8%B2%E0%B8%8A%E0%B8%B4%E0%B8%81)


# Overview
**ระบบไฟล์ (File System)** คือ ระบบปฏิบัติการที่ทำหน้าที่บริหารจัดการข้อมูลที่ถูกเก็บไว้ในฮาร์ดดิสก์หรืออุปกรณ์จัดเก็บข้อมูล ช่วยให้เราสามารถเข้าถึงข้อมูล, ควบคุมการจัดเก็บข้อมูล  อย่างการอ่าน, เขียน, และจัดการไฟล์ต่าง ๆ ได้อย่างมีประสิทธิภาพ หากไม่มีระบบไฟล์ ข้อมูลที่เก็บไว้จะเป็นก้อนเดียวกัน ทำให้ไม่สามารถแยกแยะได้ว่าเป็นข้อมูลอะไร ตำแหน่งของข้อมูลที่ต้องการจะอยู่ที่ไหน โดยระบบไฟล์มีหลายประเภท ซึ่งแต่ละระบบปฏิบัติการจะมีระบบไฟล์ที่แตกต่างกัน 

ระบบไฟล์ใน Linux ประกอบด้วยสามเลเยอร์หลัก อย่าง Logical File System ทำหน้าที่เป็นส่วนของติดต่อกันระหว่างแอปพลิแคชันของผู้ใช้และระบบไฟล์ ช่วยอำนวยความสะดวกในใช้งานต่างๆ เช่น การเปิดปิดไฟล์ การอ่านไฟล์ ส่วน Virtual File System ให้การเข้าถึงข้อมูลแก่แอปพลิเคชันผู้ใช้ โดยใช้ API เป็นช่องทางที่ให้ผู้ใช้สามารถเข้าถึงและจัดการกับไฟล์ได้ ส่วนสุดท้าย Physical File System ทำหน้าที่ในการจัดการเกี่ยวกับบล็อกหน่วความจำบน disk 

ระบบไฟล์ทั้งสามเลเยอร์นี้ทำให้ข้อมูลที่เก็บไว้เป็นระเบียบและสามารถจัดการได้อย่างมีประสิทธิภาพ นอกจากนี้ยังเครื่องมือสำหรับทำการสำรองข้อมูลเพื่อรักษาความปลอดภัยในการเข้าถึงข้อมูลอีกด้วย

# Subcomponents

### Files and Directories, FHS
ระบบปฏิบัติการของ Linux นั้นมีการจัดการกับโครงสร้างของไฟล์และไดเร็กทอรีโดยใช้มาตรฐาน FHS ซึ่งเป็นโครงสร้างแบบลำดับชั้น โดยแต่ละไฟล์นั้นสามารถกำหนดสิทธิ์ที่ต่างกันเพื่อควบคุมการเข้าถึง การอ่าน และการเขียน

### Raw Media Devices
อุปกรณ์จัดเก็บข้อมูลที่สามารถเข้าถึงข้อมูลได้โดยตรงที่ระดับบล็อก โดยไม่ต้องผ่านระบบไฟล์ และ ตอนทำงานข้อมูลจะถูกเข้าถึงโดยไม่ผ่านระบบ Caches และ Buffers ของระบบปฏิบัติการ (OS)

### Physical Volume Administration
คือ หน่วยเก็บข้อมูล Physical storage unit ของ LVM logical Volume เป็น block device ที่เก็บข้อมูลทางกายภาพใดๆ เช่น Hard disk หรือ Disk Partition ที่อยู่ในชั้นล่างสุดของ Logical Volume Management(LVM)

### Volume group Administration
คือ กลุ่มของ physical volumes โดยจะทำการสร้างพูลของพื้นที่บนดิสก์ที่สามารถจัดสรร logical volumes ได้ โดยในแต่ละ volume group จะมีพื้นที่ดิสก์ที่พร้อมใช้งานสำหรับการแบ่ง โดยจะถูกแบ่งออกเป็นส่วนๆ ที่มีขนาดคงที่เรียกว่า extents หน่วยพื้นที่ที่เล็กที่สุดที่สามารถจัดสรรได้ ภายใน physical volume

### Logical Volume Administration
คือ ระบบจัดการพื้นที่เก็บข้อมูลบน Linux การแบ่งมาจาก Volume Group ช่วยให้จัดสรรพื้นที่ได้อย่างยึดยุ่นซึ่งมีความสามารถรองรับการทำ RAID Mirroring และ Snapshot

### File System Type
ระบบไฟล์ คือ โครงสร้างหรือรูปแบบที่ใช้ในการจัดการและจัดเก็บข้อมูลบนอุปกรณ์เก็บข้อมูล โดยแต่ละระบบไฟล์มีลักษณะและคุณสมบัติที่แตกต่างกัน ที่คอยช่วยให้ข้อมูลถูกจัดเก็บและจัดการได้อย่างมีระเบียบ และรวดเร็วในการเข้าถึง

### Archiver, Backup/Restore Tools
เป็นเครื่องมือที่ใช้ในการจัดเก็บข้อมูลและสำรองข้อมูลเพื่อรักษาความปลอดภัยในการเข้าถึงข้อมูลที่สำคัญ


# สมาชิก

| รูป | รหัสนักศึกษา     | ชื่อ                  | ส่วนที่รับผิดชอบ               |
| --- | -------- | --------------------- | ------------------------------ |
|   <img height="150" src="img/Nam.png" width="150"/>  | 65070117 | นัชชา เนินกร่าง       | File System Type               |
|   <img height="150" src="img/Tum (1).png" width="150"/>  | 65070118 | นันทพงศ์ วิเศษมงคลชัย | Raw Media Devices              |
|   <img height="150" src="img/best.png"/>  | 65070129 | ปรเมศวร์ โพธิ์หมุด    | Volume Group Administration    |
|   <img height="150" src="img/Moss.png" width="150"/>  | 65070135 | ปัณณวิชญ์ ปานช้าง     | Logical Volume Administration |
|   <img height="150" src="img/Folk.png" width="150"/>  | 65070136 | ปานชีวา สุ่มมาตย์     | Archiver, Backup/Restore Tools |
|   <img height="150" src="img/del3.png" width="150"/>  | 65070160 | พีรเดช เสือแก้วน้อย   | Files and Directories, FHS     |
|   <img height="150" src="img/Phum.png" width="150"/>  | 65070177 | ภูมิ บุตรศรีชา        | Physical Volume Administration  |
