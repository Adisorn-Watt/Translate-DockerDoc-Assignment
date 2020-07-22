# Pipe Dockerfile through `stdin`

Docker มีความสามารถในการที่จะ build image โดยการ pipe `Dockerfile` ผ่านทาง `stdin` ด้วย *build context แบบ local หรือ remote* การ pipe `Dockerfile` ผ่านทาง `stdin` เป็นประโยชน์ในการ build เพื่อใช้งานครั้งเดียว (one-off builds) โดยไม่ต้องเขียน Dockerfile ลงใน disk หรือในสถานการณ์ที่ build image แล้วไม่ต้องการเก็บ `Dockerfile` ไว้

------
------
##### ตัวอย่างนี้มาจาก [เอกสารนี้](http://tldp.org/LDP/abs/html/here-docs.html) เพื่อความสะดวก แต่สามารถใช้วิธีการอื่นใดก็ได้เพื่อให้ `Dockerfile` บน `stdin` สามารถใช้ได้

ดังตัวอย่างข้างล่าง คำสั่ง 2 แบบนี้ มีค่าเท่ากัน:

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


### Build image โดยการใช้ Dockerfile ผ่าน stdin โดยไม่ทำการส่ง build context

รูปแบบนี้เป็นการ build image โดยการใช้ `Dockerfile` ผ่านทาง `stdin` โดยไม่ทำการส่งไฟล์ใดๆ เพิ่มเติมเพื่อใช้เป็น build context เครื่องหมายยัติภังค์ (Hyphen หรือ `-`) รับตำแหน่งของ `PATH` และสั่งให้ Docker อ่าน build context (ซึ่งประกอบด้วย `Dockerfile` เท่านั้น) จาก `stdin` แทนที่จะอ่านจาก directory

```
docker build [OPTIONS] -
```

ตัวอย่างต่อไปนี้ ได้ทำการ build image โดยการใช้ `Dockerfile` ที่ผ่านทาง `stdin` โดยไม่มีไฟล์อื่นใดถูกส่งมาเป็น build context ให้กับ docker daemon นี้ 

```
docker build -t myimage:latest -<<EOF
FROM busybox
RUN echo "hello world"
EOF
```

การมองข้าม build context มีประโยชน์ในสถานการณ์ที่ `Dockerfile` ของคุณ ไม่ต้องการที่จะคัดลอกไฟล์ลงไปใน image, และช่วยเพิ่มความเร็วในการ build เนื่องจากไม่มีไฟล์ใดเลยถูกส่งไปยัง daemon

ถ้าหากคุณต้องการที่จะเพิ่มความเร็วในการ build ให้มากขึ้น โดยการละเว้นไม่ส่ง`บาง`ไฟล์จาก build context ให้ทำการอ่าน [exclude with .dockerignore](#exclude-with-dockerignore)

------
------
##### **หมายเหตุ**: การพยายาม build Dockerfile โดยการใช้คำสั่ง `COPY` หรือ `ADD` จะไม่สำเร็จหากใช้คำสั่งตามรูปแบบด้านบน ตัวอย่างต่อไปข้างล่างจะแสดงให้เห็นถึงเหตุการณ์นี้
```
# create a directory to work in
mkdir example
cd example

# create an example file
touch somefile.txt

docker build -t myimage:latest -<<EOF
FROM busybox
COPY somefile.txt .
RUN cat /somefile.txt
EOF

# observe that the build fails
...
Step 2/3 : COPY somefile.txt .
COPY failed: stat /var/lib/docker/tmp/docker-builder249218248/somefile.txt: no such file or directory
```
------
------

### Build จาก build context บน local โดยการใช้ Dockerfile จาก stdin

รูปแบบนี้เป็นการ build image โดยใช้ไฟล์จากบนเครื่อง local แต่ใช้ `Dockerfile` จาก `stdin` ใช้ `-f` (หรือ `--file`) เพื่อระบุ `Dockerfile` ที่จะใช้โดยใช้ยัติภังค์ (hyphen หรือ `-`) เป็นชื่อไฟล์เพื่อสั่งให้ Docker อ่าน Dockerfile จาก `stdin`:

```
docker build [OPTIONS] -f- PATH
```

ตัวอย่างข้างล่างใช้ directory ปัจจุบัน (`.`) เป็น building context และ build image โดยการใช้ `Dockerfile` ที่ส่งผ่านทาง `stdin` โดยการใช้ [Here Documents](http://tldp.org/LDP/abs/html/here-docs.html).

```
# create a directory to work in
mkdir example
cd example

# create an example file
touch somefile.txt

# build an image using the current directory as context, and a Dockerfile passed through stdin
docker build -t myimage:latest -f- . <<EOF
FROM busybox
COPY somefile.txt .
RUN cat /somefile.txt
EOF
```

### Build จาก build context บน remote โดยการใช้ Dockerfile จาก stdin

รูปแบบนี้เป็นการ build image โดยใช้ไฟล์ที่มาจาก `git` repository และใช้ `Dockerfile` จาก `stdin` ใช้ `-f` (หรือ `--file`) เพื่อระบุ `Dockerfile` ที่จะใช้โดยใช้ยัติภังค์ (hyphen หรือ `-`) เป็นชื่อไฟล์เพื่อสั่งให้ Docker อ่าน Dockerfile จาก `stdin`:

```
docker build [OPTIONS] -f- PATH
```

การทำในรูปแบบนี้เป็นประโยชน์ในสถานการณ์ที่คุณต้องการที่จะ build image จาก repository ที่ไม่มี `Dockerfile` หรือในกรณีที่คุณต้องการที่จะ build image ด้วย `Dockerfile` ที่สร้างขึ้นมาเอง หรือผ่านการปรับแต่งมา

ตัวอย่างข้างล่างเป็นการ build image โดยใช้ `Dockerfile` จาก `stdin` และเพิ่มไฟล์ `hello.c` จาก ["hello-world" Git repository on GitHub](https://github.com/docker-library/hello-world)

```
docker build -t myimage:latest -f- https://github.com/docker-library/hello-world.git <<EOF
FROM busybox
COPY hello.c .
EOF
```

------
------
##### Under the hood

เวลาที่ทำการ build image โดยใช้ remote Git repository เป็น build context. Docker จะทำการ `git clone` repository นั้นๆ ลงบนเครื่อง local และส่งไฟล์เหล่านั้นไปเป็น build context ให้กับ daemon. จึงทำให้ต้องลง `git` บนเครื่อง local ที่ทำการรัน `docker build`
------
------
