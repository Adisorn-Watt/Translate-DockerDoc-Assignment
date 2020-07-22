## ENV

อ้างอิง Dockerfile สำหรับคำสั่ง ENV **[ที่นี่](https://docs.docker.com/engine/reference/builder/#env)**

ในการสร้างซอฟต์แวร์ใหม่ที่ง่ายต่อการ run นั้น คุณสามารถใช้ `ENV` เพื่ออัพเดท `PATH` ของตัวแปร environment สำหรับซอฟต์แวร์ที่ติดตั้งที่ container ของคุณ ยกตัวอย่างเช่น `ENV PATH /usr/local/nginx/bin:$PATH` ทำให้แน่ใจว่า `CMD ["nginx"]` นั้นสามารถใช้งานได้จริง

คำสั่ง `ENV` ยังเป็นเป็นโยชน์สำหรับการจัดเตรียมตัวแปร environment หรือสภาพแวดล้อมที่จำเป็นต่อ services ที่คุณต้องการจะจัดเก็บ เช่น Postgres’s `PGDATA`

สุดท้ายนี้ `ENV` ยังสามารถใช้เพื่อตั้งค่า number version ที่ใช้กันทั่วไป เพื่อให้ง่ายต่อการบำรุงรักษา ซึ่งจะเห็นได้จากตัวอย่างดังต่อไปนี้

```
ENV PG_MAJOR 9.3
ENV PG_VERSION 9.3.4
RUN curl -SL http://example.com/postgres-$PG_VERSION.tar.xz | tar -xJC /usr/src/postgress && …
ENV PATH /usr/local/postgres-$PG_MAJOR/bin:$PATH
```

คล้ายกับการมีตัวแปรคงที่ในโปรแกรม (ซึ่งตรงข้ามกับค่าของการ hard-coding) วิธีการนี้จะช่วยให้คุณเปลี่ยนคำสั่ง `ENV` เพียงคำสั่งเดียวเพื่อตรวจสอบ version ของซอฟต์แวร์ใน container ของคุณได้อัตโนมัติอย่างน่าอัศจรรย์

`ENV` แต่ละ line จะสร้างเลเยอร์ระหว่างกลางขึ้นมาใหม่ ซึ่งจะเหมือนกับการใช้คำสั่ง `RUN` นั่นหมายความว่า ต่อให้คุณทำการยกเลิกการตั้งค่าตัวแปร environment ของเลเยอร์ในอนาคต มันก็ยังคงอยู่ในเลเยอร์นี้และค่าของมันก็ไม่ได้หายไปไหน คุณสามารถตรวจสอบได้โดยการสร้าง Dockerfile เหมือนกับตามตัวอย่างข้างล่าง จากนั้นก็ทำการ build

```
FROM alpine
ENV ADMIN_USER="mark"
RUN echo $ADMIN_USER > ./mark
RUN unset ADMIN_USER
```

```
$ docker run --rm test sh -c 'echo $ADMIN_USER'

mark
```

เพื่อป้องกันและยกเลิกการตั้งค่าตัวแปรนี้จริงๆให้ใช้คำสั่ง `RUN` พร้อมกับ shell commands เพื่อตั้งค่า, ใช้, และยกเลิกการตั้งค่าตัวแปรทั้งหมดในแต่ละเลเยอร์ คุณสามารถแยกคำสั่งได้โดยการใช้ `;` หรือ `&&` ถ้าคุณใช้วิธีที่2 และหนึ่งในคำสั่งนั้นล้มเหลว `docker build` ก็จะล้มเหลวด้วยเช่นกัน การใช้ `\` เป็นสิ่งที่แสดงความต่อเนื่อง หรือ line continuation character สำหรับ Linux Dockerfile ช่วยเพิ่มความสามารถในการอ่านก็เป็นเรื่องที่ดี คุณยังสามารถใส่คำสั่งทั้งหมดใน shell scripts และใช้คำสั่ง `RUN` เพื่อ run เพียงแค่ shell scripts นั้น

```
FROM alpine
RUN export ADMIN_USER="mark" \
    && echo $ADMIN_USER > ./mark \
    && unset ADMIN_USER
CMD sh
```

```
$ docker run --rm test sh -c 'echo $ADMIN_USER'
```
