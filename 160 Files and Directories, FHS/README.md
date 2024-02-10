# Files and Directories, FHS
ระบบปฏิบัติการของ Linux นั้นมีการจัดการกับโครงสร้างของไฟล์และไดเร็กทอรีโดยใช้มาตรฐาน FHS ซึ่งเป็นโครงสร้างแบบลำดับชั้น โดยแต่ละไฟล์นั้นสามารถกำหนดสิทธิ์ที่ต่างกันเพื่อควบคุมการเข้าถึง การอ่าน และการเขียน

## ประเภทของไฟล์ใน Linux
#### 1. Regular files
  เป็นไฟล์ทั่วไปที่พบบ่อยที่สุด เช่น ไฟล์รูปภาพ, โปรแกรมไฟล์
  ```
  $ touch linuxcareer.com
  $ ls -ld linuxcareer.com 
  -rw-rw-r-- 1 lubos lubos 0 Jan 10 12:52 linuxcareer.com
  ```
  &nbsp;จากที่ใช้ คำสั่ง ls แล้วจะเห็น เครื่องหมาย “-” อยู่ที่ตัวแรก เป็นการระบุถึง Regular files และถ้าต้องการลบไฟล์สามารถใช้คำสั่ง rm	
  ```
  $ rm linuxcareer.com 
  $
  ```
  
#### 2. Directories
  เป็นไฟล์ที่พบได้บ่อยใน Linux ใช้เก็บ Subdirectories สามารถใช้คำสั่ง  mkdir ในการสร้างไดเร็กทอรี
  ```
  $ mkdir FileTypes
  $ ls -ld FileTypes/
  drwxrwxr-x 2 lubos lubos 4096 Jan 10 13:14 FileTypes/
  ```
  &nbsp;จะเห็น ตัว “d” อยู่ด้านหน้าสุด เป็นการระบุว่าเป็นไดเร็กทอรีและ ถ้าต้องการลบไดเร็กทอรี ให้ใช้คําสั่ง rmdir
  ```
  $ rmdir FileTypes
  ```
  &nbsp;ถ้าใช้คำสั่ง rmdir กับ ไดเร็กทอรีที่มีไฟล์อยู่ข้างในจะเกิดข้อผิดพลาด
  ```
  rmdir: failed to remove `FileTypes/': Directory not empty
  ```
  &nbsp;ซึ่งเราสามารถแก้ไขได้โดยใช้คำสั่ง
  ```
  $ rm -r FileTypes/
  ```

#### 3. Character device
  ทั้ง character device และ block device เป็นอุปกรณ์ที่ช่วยให้ ผู้ใช้และ โปรแกรมสามารถ ติดต่อสื่อสารกับอุปกรณ์ต่อพ่วงได้ เช่น vmware   
  ```
  $ ls -ld /dev/vmmon 
  crw------- 1 root root 10, 165 Jan  4 10:13 /dev/vmmon
  ```
  

#### 4. Block Device
  เป็นอุปกรณ์ คล้ายกับ character device โดยส่วนใหญ่จะควบคุมฮาร์ดแวร์เช่น ฮาร์ดไดรฟ์หรือ หน่วยความจำ
  ```
  $ ls -ld /dev/sda
  brw-rw---- 1 root disk 8, 0 Jan  4 10:12 /dev/sda
  ```


#### 5. Local domain sockets
  จะใช้เพื่อสื่อสารระหว่างกระบวนการภายในเครื่อง โดยทั่วไปมักถูกใช้โดยบริการและโปรแกรม เช่น X windows, syslog
  ```
  $ ls -ld /dev/log
  srw-rw-rw- 1 root root 0 Jan  4 10:13 /dev/log
  ```
  &nbsp;สามารถใช้คำสั่ง unlink หรือ rm ในการลบ sockets  

#### 6. Named Pipes
  Named Pipes หรือ FIFOs คล้ายๆ กับ Local domain sockets แต่ Named Pipes จะเป็นช่องทางที่ใช้ในการสื่อสารกันระหว่าง สองโปรแกรมในเครื่อง สามารถสร้างได้โดยคําสั่ง mknod และใช้คําสั่ง rm ในการลบออก

#### 7. Symbolic links
  ผู้ดูแลระบบ (administrator) สามารถกําหนดสัญลักษณ์ให้ไฟล์หรือไดเร็กทอรีเพื่อใช้เป็นตัวชี้ไปยังไฟล์ต้นฉบับ ซึ่ง Links จะมีอยู่สองประเภท:
  - hard links
  - soft links

ความแต่ต่างระหว่าง hard links และ soft links คือ ถ้าเป็น soft links จะใช้ชื่อไฟล์เป็นแหล่งอ้างอิงในการเข้าถึงไฟล์ แต่ถ้าเป็น hard links จะอ้างอิงไปที่ไฟล์ต้นฉบับโดยตรง และ hard link ยัง      ไม่สามารถข้ามระบบไฟล์และพาร์ติชันได้ และในการสร้าง soft links สามารถใช้คำสั่ง ln -s ได้
  ```
  $ echo file1 > file1
  $ ln -s file1 file2
  $ cat file2 
  file1
  $ ls -ld file2 
  lrwxrwxrwx 1 lubos lubos 5 Jan 10 14:42 file2 -> file1
  ```
  โดยสามารถใช้คําสั่ง unlink หรือ rm ในการลบ Link

### สรุปประเภทของไฟล์ใน Linux
| ตัวอักษร  | ความหมาย                 | 
|----------|---------------------------|
| `-`      | Regular files             |
| `d`      | Directories               |
| `l`      | Symbolic links            |
| `b`      | Block Device              |
| `p`      | Named Pipes               |
| `c`      | Character device          |
| `s`      | Local domain sockets      |

  
## Linux Directories 
  Command คืออะไร command ก็คือคำสั่งที่เราเขียนเพื่อให้ คอมพิวเตอร์ทำงานตามความต้องการของเรา ถ้าเป็นใน windows สามารถเขียนคำสั่งได้ใน Command Prompt แต่ถ้าเป็นใน Linux จะใช้ Terminal โดย   คำสั่งจะถูกส่งไปยัง shell ซึ่งจะทำการอ่านและดำเนินการ โดย shell จะทำหน้าที่เป็นตัวกลางระหว่างผู้ใช้กับคอมพิวเตอร์

  คำสั่ง shell จะมีอยู่สองประเภท
  - Built-in shell commands: เป็นคำสั่งที่เป็นส่วนหนึ่งของ shell 
  - External/Linux commands: เป็นคำสั่งจากภายนอกที่แยกออกมาต่างหากอาจจะเขียนด้วยภาษา C หรือโปรแกรมอื่นๆ

### Linux Directory Commands 
  1. pwd  มาจากคำว่า  (print working directory) เป็นคำสั่งที่ใช้แสดงไดเร็กทอรีที่ทำงานอยู่ใน ปัจจุบัน ซึ่งเมื่อเราเข้าสู่ระบบของ Linux เริ่มต้นจะอยู่ที่ home ไดเร็กทอรี

  รูปแบบการเขียนคำสั่ง
  ```
  pwd [-options] 
  ```
  ตัวอย่างการใช้งาน
  ```
  $ /bin/pwd  
  ```
  ผลลัพธ์
  
  ![Linux Directory Structure](https://static.javatpoint.com/linux/images/linux-pwd-command.png)


  
## Linux File Contents 
## Filesystem Hierarchy Standard (FHS)
