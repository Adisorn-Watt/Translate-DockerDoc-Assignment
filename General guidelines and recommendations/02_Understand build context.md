# Understand build context
เมื่อคุณใช้คำสั่ง `docker build` directory ที่กำลังทำงานอยู่ปัจจุบันก็คือ *build context* โดยทั่วไปแล้วจะถือว่า Dockerfile อยู่ตรงนี้เป็นค่าเริ่มต้น แต่คุณสามารถระบุเจาะจงได้ว่าอยู่ที่ directory อื่นๆ ได้ด้วยการแฟล็กไฟล์ (file flag หรือ `-f`) ไม่ว่าตำแหน่งของ `Dockerfile` นั้นจะอยู่ที่ใด ไฟล์และ directory ที่มีอยู่ใน directory ปัจจุบัน จะถูกส่งไปยัง Docker daemon เพื่อเป็น build context

------
------
##### ตัวอย่าง Build context 
สร้าง directory สำหรับ build context และ `cd` (เปลี่ยน directory) เข้าไปใน directory นั้นๆ จากนั้นพิมพ์คำว่า "hello" เข้าไปในไฟล์ประเภท text ชื่อไฟล์ว่า `hello` และสร้าง Dockerfile ที่รันคำสั่ง `cat` อยู่ภายในไฟล์ แล้ว build image จาก build context นี้ (`.`):

```
mkdir myproject && cd myproject
echo "hello" > hello
echo -e "FROM busybox\nCOPY /hello /\nRUN cat /hello" > Dockerfile
docker build -t helloapp:v1 . 
```
ย้าย `Dockerfile` และไฟล์ `hello` ไปยัง directory ที่อยู่แยกออกไป และทำการสร้าง image เวอร์ชั่นใหม่ (โดยปราศจากการพึ่งพาแคชจากการ build รอบล่าสุด) ใช้ `-f` ในการชี้ไปยัง Dockerfile และระบุเจาะจง directory ของ build context 

```
mkdir -p dockerfiles context
mv Dockerfile dockerfiles && mv hello context
docker build --no-cache -t helloapp:v2 -f dockerfiles/Dockerfile context
```

------
------

การรวมไฟล์ที่ไม่จำเป็นต่อการ build image อย่างไม่ได้ตั้งใจนั้น ส่งผลให้ build context รวมทั้ง image ที่สร้างมีขนาดที่ใหญ่ขึ้น ซึ่งทำให้ใช้เวลาที่มากขึ้นในการ build image, การ pull และ push, รวมทั้งเพิ่มขนาดรันไทม์ของ container หากต้องการที่จะเห็นว่า build context ของคุณใหญ่ขนาดไหน ให้มองหาข้อความประมาณนี้เวลาที่ทำการ build จาก `Dockerfile`:

```
Sending build context to Docker daemon  187.8MB
```