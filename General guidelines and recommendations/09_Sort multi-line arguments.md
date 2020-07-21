# Sort multi-line arguments

- เมื่อใดก็ตามที่เป็นไปได้ ให้ลดความยุ่งยากที่จะตามมาในภายหลัง โดยการเรียงลำดับ arguments ตามตัวอักษรและตัวเลข
- การทำแบบนี้จะช่วยลดการทำ package ซ้ำ และทำให้ง่่ายต่อการอัพเดท อ่านง่าย และตรวจสอบ PR ได้ง่ายขึ้น
- การเคาะspace ใช้เครื่องหมาย backslash ( \ ) ก็ช่วยได้เช่นกัน

#### ตัวอย่าง buildpack-deps image:

```
RUN apt-get update && apt-get install -y \
  bzr \
  cvs \
  git \
  mercurial \
  subversion
```
