# Pipe Dockerfile through `stdin`

Docker มีความสามารถในการที่จะ build image โดยการ pipe `Dockerfile` ผ่านทางคำสั่ง `stdin` ด้วย build context แบบ local หรือ remote การ pipe `Dockerfile` ผ่านทางคำสั่ง `stdin` สามารถเป็นประโยชน์ในการ build เพื่อใช้งานครั้งเดียว (one-off builds) โดยไม่การต้องเขียน Dockerfile ลงใน disk หรือในสถานการณ์ที่ build image แล้วไม่ต้องการเก็บ `Dockerfile` ไว้

------
------
##### ตัวอย่างนี้มาจาก [เอกสารนี้](http://tldp.org/LDP/abs/html/here-docs.html) เพื่อความสะดวก แต่สามารถใช้วิธีการอื่นใดก็ได้เพื่อให้ `Dockerfile` บน `stdin` สามารถใช้ได้

ดังตัวอย่างข้างล่าง คำสั่ง 2 แบบนี้ มีค่าเท่ากัน

```
echo -e 'FROM busybox\nRUN echo "hello world"' | docker build -
```
```
docker build -<<EOF
FROM busybox
RUN echo "hello world"
EOF
```

คุณสามารถแทนที่ตัวอย่างนี้ได้ด้วยวิธีการที่คุณต้องการ หรือวิธีการที่เหมาะสมกับงานที่ทำ (use-case)

------
------

### BUILD AN IMAGE USING A DOCKERFILE FROM STDIN, WITHOUT SENDING BUILD CONTEXT

