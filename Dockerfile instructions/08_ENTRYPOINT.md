# ENTRYPOINT

ไฟล์อ้างอิง Docker สำหรับคำแนะนำของ ENTRYPOINT : <https://docs.docker.com/engine/reference/builder/#entrypoint/>

การใช้ที่ดีที่สุดสำหรับ ENTRYPOINT คือการใช้ตั้งค่าคำสั่งหลักของอิมเมจนั้นๆทำให้ตัวอิมเมจนั้นสามารถทำวานราวกับว่าพิมพ์คำสั่งนั้นๆ(และหลังจากนั้นจะใช้ CMD เป็นตัวพื้นฐาน)

มาเริ่มกันจากตัวอย่างของอิมเมจสำหรับเครื่องมือของชุดคำสั่ง
s3cmd:

```
ENTRYPOINT ["s3cmd"]
CMD ["--help"]
```

ตอนนี้อิมเมจสามารถทำงานแบบนี้เพื่อแสดงคำสั่ง help

```
$ docker run s3cmd
```

หรือใช้พารามิเตอร์ให้ถูกต้องเพื่อออกคำสั่ง

```
$ docker run s3cmd ls s3://mybucket
```

สิ่งนี้เป็นประโยชน์มากเพราะชื่อของอิมเมจสามารถเพิ่มเป็นเท่าตัวเหมือนกับการอ้างอิงไปยังไปนารี่ดังที่แสดงในคำสั่งด้านบน

ตัวคำแนะนำ ENTRYPOINT ยังเป็นการใช้ผสมผสานกับสคริปช่วยเหลือเพื่ออนุญาตใช้มันทำงานไปในทางเดียวกันเพื่อที่จะออกคำสั่งข้างต้นแม้ว่าขณะเริ่มเครื่องมืออาจจะต้องการมากกว่าหนึ่งขั้นตอนก็ตาม

สำหรับตัวอย่างตัว Postgres Official Image: <https://hub.docker.com/_/postgres/> ใช้สคริปต่อไปนี้เป็น ENTRYPOINT:

```
#!/bin/bash
set -e

if [ "$1" = 'postgres' ]; then
    chown -R postgres "$PGDATA"

    if [ -z "$(ls -A "$PGDATA")" ]; then
        gosu postgres initdb
    fi

    exec gosu postgres "$@"
fi

exec "$@"
```

#### ปรับแต่งแอ๊พให้เป็น PID1

สคริปนี้ใช้ exec Bash command เพราะฉะนั้นแอ๊พที่ทำงานจริงๆจะเป็นคอนแทนเน่อร์ PID 1ซึ้งจะอนุญาติให้แอ๊พรับสัญญาณจาก Unix ที่ส่งไปนังคอนเทนเน่อร์
สามารถศึกษาเพิ่มเติมได้ที่ :<https://docs.docker.com/engine/reference/builder/#entrypoint/>

สคริปช่วยเหลือจะถูกคัดลอกไปยังคอนเทนเน่อร์และทำงานผ่าน ENTRYPOINT เมื่อคอนเทนเน่อร์เริ่ม

```
COPY ./docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["postgres"]
```

สคริปนี้จะทำให้ผู้ใช้สามารถติดต่อกับ Postgers ได้ในหลายๆทาง

สามารถทำการเริ่ม Postgres ได้ง่ายๆ

```
$ docker run postgres
```

หรือมันสามารถใช้เพื่อเปิดการทำงานของ Postgres และส่งค่าต่างๆไปยังเซิฟเวอร์ได้

```
$ docker run postgres postgres --help
```

สุดท้ายนี้มันยังสามารถใช้ในการเริ่มเครื่องมือที่แตกต่างโดยสิ้นเชิงเช่น Bash

```
$ docker run --rm -it postgres bash
```
