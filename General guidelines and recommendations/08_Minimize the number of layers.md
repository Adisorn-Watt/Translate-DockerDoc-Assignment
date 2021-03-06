## Minimize the number of layers

#### ลดจำนวนเลเยอร์ให้ได้น้อยที่สุด

ใน Docker เวอร์ชันเก่า มันเป็นเรื่องสำคัญที่คุณต้องลดจำนวนเลเยอร์ใน image ของคุณให้ได้น้อยที่สุด เพื่อให้แน่ใจว่าพวกมันจะทำงานได้ดี

ลดข้อจำกัดโดยการเพิ่ม features ดังต่อไปนี้:

- มีเพียงแค่คำสั่ง `RUN`,`COPY`,`ADD` เท่านั้นที่ใช้สร้างเลยอร์ คำสั่งอื่นๆจะใช้สร้าง image กลางชั่วคราว และห้ามมีการเพิ่มขนาดของการ build
- หากเป็นไปได้ ให้ใช้ **[multi-stage builds](https://docs.docker.com/develop/develop-images/multistage-build/)** และคัดลอกเฉพาะสิ่งที่คุณต้องการลงใน image สุดท้าย การทำเช่นนี้จะทำให้คุณรวมเครื่องมือต่างๆและตรวจสอบข้อผิดพลาดของข้อมูลในระหว่างการ build stages ของคุณโดยไม่เป็นการเพิ่มขนาดของ image สุดท้าย
