# Files and Directories, FHS
ระบบปฏิบัติการของ Linux นั้นมีการจัดการกับโครงสร้างของไฟล์และไดเร็กทอรีโดยใช้มาตรฐาน FHS ซึ่งเป็นโครงสร้างแบบลำดับชั้น โดยแต่ละไฟล์นั้นสามารถกำหนดสิทธิ์ที่ต่างกันเพื่อควบคุมการเข้าถึง การอ่าน และการเขียน

## ประเภทของไฟล์ใน Linux
1. Regular files
  เป็นไฟล์ทั่วไปที่พบบ่อยที่สุด เช่น ไฟล์รูปภาพ, โปรแกรมไฟล์
  ตัวอย่าง
  ```
  $ touch linuxcareer.com
  $ ls -ld linuxcareer.com 
  -rw-rw-r-- 1 lubos lubos 0 Jan 10 12:52 linuxcareer.com
  ```
  จากที่ใช้ คำสั่ง ls แล้วจะเห็น เครื่องหมาย “-” อยู่ที่ตัวแรก เป็นการระบุถึง Regular files และถ้าต้องการลบไฟล์สามารถใช้คำสั่ง rm	
  ```
  $ rm linuxcareer.com 
  $
  ```
  
2. Directories
  เป็นไฟล์ที่พบได้บ่อยใน Linux ใช้เก็บ Subdirectories สามารถใช้คำสั่ง  mkdir ในการสร้างไดเร็กทอรี
  ตัวอย่าง 
  ```
  $ mkdir FileTypes
  $ ls -ld FileTypes/
  drwxrwxr-x 2 lubos lubos 4096 Jan 10 13:14 FileTypes/
  ```
  จะเห็น ตัว “d” อยู่ด้านหน้าสุด เป็นการระบุว่าเป็นไดเร็กทอรีและ ถ้าต้องการลบไดเร็กทอรี ให้ใช้คําสั่ง rmdir
  ```
  $ rmdir FileTypes
  ```
  ถ้าใช้คำสั่ง rmdir กับ ไดเร็กทอรีที่มีไฟล์อยู่ข้างในจะเกิดข้อผิดพลาด
  ```
  rmdir: failed to remove `FileTypes/': Directory not empty
  ```
  ซึ่งเราสามารถแก้ไขได้โดยใช้คำสั่ง
  ```
  $ rm -r FileTypes/
  ```

3. Character device
4. Block Device
5. Local domain sockets
6. Named Pipes
7. Symbolic links
## Linux Directories 
## Linux File Contents 
## Filesystem Hierarchy Standard (FHS)
