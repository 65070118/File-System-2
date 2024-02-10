# Archiver, Backup/Restore Tools

เป็นเครื่องมือที่ใช้ในการจัดเก็บข้อมูลและสำรองข้อมูลเพื่อรักษาความปลอดภัยในการเข้าถึงข้อมูลที่สำคัญ โดยมีลักษณะการใช้งานที่แตกต่างกันออกไป ซึ่งแบ่งออกเป็น 2 หัวข้อหลักๆ คือ

- **Archiver and Compression** เป็นเครื่องมือที่ใช้ในการลดขนาดของไฟล์หรือบีบอัดไฟล์เพื่อช่วยในการจัดเก็บข้อมูลที่มีปริมาณมากให้ประหยัดพื้นที่ในการจัดเก็บข้อมูลมากขึ้น
- **Backup/Restore Tools** เป็นเครื่องมือที่ใช้สำหรับการสำรองข้อมูลโดยมีการสร้างสำเนาข้อมูล (Backup) เพื่อป้องกันข้อมูลจากการสูญหายหรือเสียหาย และยังสามารถกู้คืนข้อมูล (Restore) ในกรณีที่ข้อมูลสูญหายหรือถูกทำลาย

## Archiver and Compression
เมื่อรวมเนื้อหาหลายรายการไว้ในไฟล์เดียว จะเรียกว่า `archiving` ซึ่งไฟล์ขนาดกะทัดรัดนี้สามารถถ่ายโอนไปยังเครื่องอื่นหรือจัดเก็บไว้ในที่เก็บข้อมูลได้ ซึ่งการใช้งานแบบนี้มีความสำคัญมากในระบบปฏิบัติการ Linux เนื่องจากเมื่อติดตั้งซอฟต์แวร์ใด ๆ ก็ตาม ซอฟต์แวร์นั้นจะถูกดาวน์โหลดมาเป็น archive file ก่อน
![type](https://linuxsimply.com/wp-content/uploads/2022/11/archive-in-linux-0-e1669657153386-1024x645.png)

**Archiver VS Compression**

- **Archiver** คือ โปรแกรมจัดเก็บไฟล์โดยรวมหลายไฟล์ไว้ในไฟล์เก็บถาวรเพียงไฟล์เดียว เช่น tar, cpio
- **Compression** คือ เครื่องมือบีบอัดและขยายข้อมูล เช่น gzip, bzip2

เครื่องมือเหล่านี้มักใช้ตามลำดับโดยสร้าง archive file ก่อนแล้วจึงบีบอัดไฟล์ แต่ก็ยังมีเครื่องมือที่ทำทั้งสองอย่างด้วย เช่น rar, zip

| Archiver | Compression |
|-------------------|-------------------|
| archive file คือชุดของไฟล์และไดเร็กทอรีทั้งหมดที่เก็บไว้ในไฟล์เดียว      | compressed file คือชุดของไฟล์และไดเร็กทอรีที่จัดเก็บไว้ในไฟล์เดียว โดยจัดเก็บในลักษณะที่ใช้พื้นที่ดิสก์น้อยกว่าไฟล์และไดเร็กทอรีทั้งหมดรวมกัน      |
| archive file ไม่ได้ถูกบีบอัด คือมันใช้พื้นที่ดิสก์เท่ากันกับไฟล์และไดเร็กทอรีทั้งหมดรวมกัน      | หากกังวลเรื่องเนื้อที่ดิสก์ ให้บีบอัดไฟล์ที่ไม่ค่อยได้ใช้ หรือจัดเก็บไฟล์ทั้งหมดใน archive file แล้วค่อยบีบอัดไฟล์      |

![type](https://linuxsimply.com/wp-content/uploads/2022/11/archive-in-linux-0_1-e1669657325603-1024x691.png)

- ## tar
คำว่า `tar` ใน Linux ย่อมาจาก `tape archive` หมายถึงการสร้างไฟล์เก็บถาวรและแยกไฟล์เก็บถาวร คำสั่ง `tar` ใน Linux เป็นหนึ่งในคำสั่งสำคัญที่ให้ฟังก์ชันการเก็บถาวรในระบบปฏิบัติการ Linux เราสามารถใช้คำสั่ง `tar` ของ Linux เพื่อสร้างไฟล์เก็บถาวรที่บีบอัดหรือไม่บีบอัด และดูแลรักษาและแก้ไขไฟล์เหล่านั้น

```
tar [options] [archive-file] [file or directory to be archived]
```
**การสร้าง tar archive ที่ไม่บีบอัดโดยใช้ -cvf**

คำสั่งนี้สร้างไฟล์ `tar` ชื่อ `file.tar` ซึ่งเป็นไฟล์เก็บถาวรของไฟล์ `.c` ทั้งหมดในไดเร็กทอรีปัจจุบัน
```
tar cvf file.tar *.c
```
- '-c': สร้างไฟล์ archive ใหม่
- '-v': แสดงผลลัพธ์แบบ verbose โดยแสดงความคืบหน้าของกระบวนการ archiving
- '-f': ระบุชื่อไฟล์ของเอกสาร

**Output**
```
os2.c
os3.c
os4.c
```
  
**แตกไฟล์ออกจากไฟล์ archive โดยใช้ -xvf**

คำสั่งนี้ใช้แตกไฟล์ออกจากไฟล์ archive
```
tar xvf file.tar
```
- '-x': แตกไฟล์ archive
- '-v': แสดงผลลัพธ์แบบ verbose ตอนกระบวนการแยกไฟล์
- '-f': ระบุชื่อไฟล์ของเอกสาร

**Output**
```
os2.c
os3.c
os4.c
```

**การบีบอัด gzip บน tar archive โดยใช้ -z**

คำสั่งนี้สร้างไฟล์ `tar` ชื่อ `file.tar.gz` ซึ่งเป็นไฟล์เก็บถาวรของไฟล์ `.c`
```
tar cvzf file.tar.gz *.c
```
**แยก gzip tar Archive (*.tar.gz) โดยใช้ -xvzf**

คำสั่งนี้จะแตกไฟล์จากไฟล์ `tar` ที่เก็บใน `file.tar.gz`
```
tar xvzf file.tar.gz
```
**Command**
| Option   | Description                                       |
|----------|---------------------------------------------------|
| -A       | Append tar files to existing archives.           |
| -c       | Create a new archive file.                        |
| -d       | Compare archive with specified filesystem.       |
| -j       | Bzip the archive.                                 |
| -r       | Append files to existing archives.                |
| -t       | List contents of existing archives.               |
| -u       | Update archive.                                   |
| -f       | Specifies the filename of the archive to be created or extracted.               |
| -v       | Displays verbose information, providing detailed output during the archiving or extraction process.                                 |
| -x       | Extract file from existing archive.               |
| -z       | Gzip the archive.                                 |
| --delete | Delete files from existing archive.               |

### สรุป
บทความนี้เน้นเรื่องการลดขนาดของไฟล์ในระบบปฏิบัติการ Linux โดยการใช้คำสั่ง Tape Archive (Tar) ซึ่งเป็นเครื่องมือที่สามารถรวมไฟล์และบีบอัดไฟล์เหล่านั้นให้มีขนาดเล็กลง และหวังว่าคู่มือนี้จะช่วยให้ผู้ใช้ Linux ทำงานอย่างมีประสิทธิภาพและง่ายต่อการจัดการมากขึ้น

- ## zip
**โครงสร้างสำหรับการสร้างไฟล์ zip**
```
zip [file_name.zip] [file_name]
```
**คำสั่ง unzip ใน Zip**

`unzip` เป็นคำสั่งที่ใช้ในระบบ Unix เพื่อแสดงรายการทดสอบหรือแตกไฟล์จากไฟล์ `zip` โดยทั่วไปแล้วพฤติกรรมเริ่มต้นของ `unzip` คือการแตกไฟล์ทั้งหมดจากไฟล์ `zip` ที่ระบุลงในไดเร็กทอรีปัจจุบัน นั่นหมายความว่าถ้าไฟล์ zip มีโครงสร้างโฟลเดอร์ซ้อนกัน unzip จะสร้างโฟลเดอร์ที่มีชื่อเหมือนกับชื่อของไฟล์ zip และแยกไฟล์ไปยังโฟลเดอร์นั้นๆ โดยจะไม่ลบหรือแทนที่ไฟล์ที่มีชื่อเหมือนกันที่มีอยู่แล้วในไดเร็กทอรีปัจจุบัน

ตัวอย่างเช่น เรามีไฟล์ zip “name = jayesh_gfg.zip” และเรามีไฟล์ข้อความสามไฟล์อยู่ข้างใน “name = a.txt, b.txt และ c.txt” ต้องการ unzip
```
unzip jayesh_gfg.zip
```
**Output**

![type](https://media.geeksforgeeks.org/wp-content/uploads/20230424165015/56.webp)

**คำสั่ง -u ใน Zip**

ตัวอย่างเช่น เรามีไฟล์ zip “name= myfile.zip” และเราต้องเพิ่มไฟล์ใหม่ “name = hello9.c” เข้าไป
```
zip -u myfile.zip hello9.c
```
**Output**

![type](https://media.geeksforgeeks.org/wp-content/uploads/20230424172444/58.webp)

**คำสั่ง -m ใน Zip**

ตัวอย่างเช่น เรามีไฟล์ zip “name= myfile.zip” และเราต้องย้ายไฟล์ “name = hello1.c, hello2.c, hello3.c, hello4.c, hello5.c, hello6.c, hello8.c, hello9 .c ” ในไดเร็กทอรีปัจจุบันเป็นไฟล์ zip
```
zip -m myfile.zip *.c
```
**Output**

![type](https://media.geeksforgeeks.org/wp-content/uploads/20230424173509/59.webp)

**คำสั่ง -r ใน Zip**

ตัวอย่างเช่น เรามีไฟล์ zip “name= myfile.zip” และเราต้องย้ายไฟล์ “name = hello1.c, hello2.c, hello3.c, hello4.c, hello5.c, hello6.c, hello7.c, hello8 .c ” มีอยู่ในไดเร็กทอรี “name= jkj_gfg” ไปยังไฟล์ zip เดิม
```
zip -r myfile.zip jkj_gfg/
```
ใช้คำสั่ง `vi myfile.zip` เพื่อตรวจสอบ

**Output**

![type](https://media.geeksforgeeks.org/wp-content/uploads/20230424174443/60.webp)

**Command**
| Option | Description                                           |
|--------|-------------------------------------------------------|
| -d     | Remove files from the archive.                        |
| -u     | Update files in the archive.                          |
| -m     | Move files into the archive.                          |
| -r     | Recursively zip a directory.                          |
| -x     | Exclude files from the zip.                           |
| -v     | Verbose mode.                                         |


### สรุป
คำสั่ง Zip ใน Linux ใช้เพื่อบีบอัดไฟล์และบรรจุลงในไฟล์เก็บถาวร .zip ไฟล์เดียว ซึ่งช่วยในการประหยัดพื้นที่ดิสก์และทำให้ง่ายต่อการจัดการข้อมูลขนาดใหญ่ เราได้พูดถึงตัวเลือกต่างๆ ที่ใช้ในคำสั่ง zip เช่น -u, -m, -r  ซึ่งเป็นเครื่องมือที่แนะนำสำหรับผู้ใช้ Linux ในการจัดการไฟล์ให้มีประสิทธิภาพ

- ## gzip
```
gzip [Options] [filenames]
```
**การบีบอัดไฟล์โดยใช้คำสั่ง gzip ใน Linux**

คำสั่งนี้จะเป็นการบีบอัดไฟล์ชื่อ `mydoc.txt`
```
gzip mydoc.txt
```
**จะขยายขนาดไฟล์ gzip ใน Linux ได้อย่างไร?**

คำสั่ง `gzip` พื้นฐานสำหรับการขยายขนาดไฟล์มีดังนี้
```
gzip -d filename.gz
```
คำสั่งนี้จะแตกไฟล์ `gzip` ที่ระบุ โดยไฟล์ต้นฉบับที่ไม่ถูกบีบอัดยังคงอยู่

**เก็บไฟล์ต้นฉบับโดยใช้คำสั่ง gzip ใน Linux**

ค่าเริ่มต้น `gzip` จะลบไฟล์ต้นฉบับหลังการบีบอัด ถ้าต้องการเก็บไฟล์ต้นฉบับไว้ ให้ใช้ -k
```
gzip -k example.txt
```
คำสั่งนี้จะบีบอัด `example.txt` และเก็บไฟล์ต้นฉบับไว้ครบถ้วน

**Command**
| Option | Description                                                                                               |
|--------|-----------------------------------------------------------------------------------------------------------|
| -f     | Forcefully compress a file even if a compressed version with the same name already exists.                |
| -k     | Compress a file and keep the original file, resulting in both the compressed and original files.         |
| -L     | Display the gzip license for the software.                                                                |
| -r     | Recursively compress all files in a folder and its subfolders.                                            |
| -v     | Display the name and percentage reduction for each file compressed or decompressed.                       |
| -d     | Decompress a file that was compressed using the gzip command.                                              |



### สรุป
ในบทความนี้ เราได้แสดงถึงความสำคัญและความทรงพลังของเครื่องมือ gzip ในระบบปฏิบัติการ Linux ซึ่งเป็นเครื่องมือที่มีประสิทธิภาพสูงในการบีบอัดและขยายขนาดไฟล์ เช่น `-k` เพื่อรักษาไฟล์ต้นฉบับ และ `-v` เพื่อให้ข้อมูลเพิ่มเติม รวมถึง `-f` ที่ช่วยในการบีบอัด และ `-r` เพื่อความสะดวกในการบีบอัดแบบเรียกซ้ำ ทำให้ gzip เป็นเครื่องมือที่ใช้งานได้ง่ายและมีประสิทธิภาพในระบบ Linux
