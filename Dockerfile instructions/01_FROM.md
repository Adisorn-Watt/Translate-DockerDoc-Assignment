# Dockerfile instructions

คำแนะนำต่อไปนี้ออกแบบมาเพื่อช่วยให้เราสร้าง Dockerfile ที่มีประสิทธิภาพ สามารถพัฒนา และบำรุงรักษาได้

```
Dockerfile
```

# FROM

ข้อมูลเพิ่มเติม : <https://docs.docker.com/engine/reference/builder/#from>

- ควรใช้ official image ที่เป็นปัจจุบันที่สุด
- ควรใช้ Alpine image เนื่องจากมีการควบคุมที่แน่นหนา และมีขนาดเล็ก (ปัจจุบันต่ำกว่า 5 MB)
  ในขณะที่ยังคงเป็น full Linux distribution
