## Understand build context
เมื่อคุณใช้คำสั่ง `docker build` ไดเร็กทอรีที่กำลังทำงานอยู่จะถูกเรียกว่า *build context* โดยทั่วไปแล้วจะถือว่า Dockerfile อยู่ตรงนี้ แต่คุณสามารถระบุเจาะจงไปยังไดเร็กทอรีอื่นๆ ได้ด้วยการแฟล็กไฟล์ (file flag หรือ `-f`) ไม่ว่าตำแหน่งของ `Dockerfile` นั้นจะอยู่ที่ใด ส่วนของ recursive contents ทั้งหมดของไฟล์และไดเร็กทอรีที่มีอยู่ในไดเร็กทอรีปัจจุบัน จะถูกส่งไปยัง Docker daemon เพื่อเป็น build context

------
------
#### Build context example
สร้างไดเร็กทอรีสำหรับ build context และ `cd` (เปลี่ยนไดเร็กทอรี) เข้าไปในไดเร็กทอรีนั้นๆ จากนั้นพิมพ์คำว่า "hello" เข้าไปในไฟล์ประเภท text ชื่อไฟล์ว่า `hello` และสร้างไฟล์ Dockerfile ที่รันคำสั่ง `cat` อยู่ภายในไฟล์ แล้ว build image จากภายใน build context (`.`):

```
mkdir myproject && cd myproject
echo "hello" > hello
echo -e "FROM busybox\nCOPY /hello /\nRUN cat /hello" > Dockerfile
docker build -t helloapp:v1 . 
```
ย้ายไฟล์ `Dockerfile` และไฟล์ `hello` ไปยังไดเร็กทอรีที่อยู่แยกออกไป และทำการสร้าง image เวอร์ชั่นที่ 2 (โดยปราศจากการพึ่งพาแคชจากการ build รอบล่าสุด) ใช้ `-f` ในการชี้ไปยัง Dockerfile และเจาะจงไดเร็กทอรีที่ build context อยู่

```
mkdir -p dockerfiles context
mv Dockerfile dockerfiles && mv hello context
docker build --no-cache -t helloapp:v2 -f dockerfiles/Dockerfile context
```

------
------

การรวมไฟล์ที่ไม่จำเป็นต่อการ build image อย่างไม่ได้ตั้งใจนั้น ส่งผลให้ build context รวมทั้ง image มีขนาดที่ใหญ่ขึ้น ซึ่งเป็นเหตุให้เกิดการใช้เวลาที่มากขึ้นในการ build image, การ pull และ push, รวมทั้งเพิ่มขนาดรันไทม์ของ container หากต้องการที่จะเห็นว่า build context ของคุณใหญ่ขนาดไหน ให้มองหาข้อความประมาณนี้เวลาที่ทำการ build `Dockerfile`:

```
Sending build context to Docker daemon  187.8MB
```