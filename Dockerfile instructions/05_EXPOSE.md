# EXPOSE

อ้างอิงจาก [Dockerfile สำหรับการใช้ EXPOSE](https://docs.docker.com/engine/reference/builder/#expose)

คำแนะนำสำหรับการใช้ `EXPOSE` ชี้ให้เห็นถึงพอร์ตที่คอนเทนเนอร์ฟังสำหรับการเชื่อมต่อ ดังนั้นคุณควรใช้พอร์ตทั่วไปบนแอปพลิเคชั่น ตัวอย่างเช่น อิมเมจ ที่มี Apache web server จะใช้ `EXPOSE 80` ในขณะที่อิมเมจที่มี MongoDB จะใช้ `EXPOSE 27017` เป็นต้น

สำหรับการเข้าถึงจากภายนอก ผู้ใช้สามารถดำเนินการ `docker run` ได้ด้วยการตั้งค่าสถานะให้ระบุพอร์ตเป็นพอร์ตที่เลือกได้ สำหรับการเชื่อมโยงคอนเทนเนอร์ Docker จะเตรียมสภาพแวดล้อมของตัวแปรสำหรับเส้นทางจากคอนเทนเนอร์ฝั่งรับกลับไปที่ต้นทาง (เช่น `MYSQL_PORT_3306_TCP`)