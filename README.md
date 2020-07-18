# Best practices for writing Dockerfiles

## Introduction
ฺเอกสารนี้ครอบคลุมแนวทางปฏิบัติและวิธีการที่ดีที่สุดที่แนะนำสำหรับการสร้าง image ที่มีประสิทธิภาพ...\
อ่านต่อที่ [Introduction.md](../blob/Introduction.md)

## General guidelines and recommendations
- Create ephemeral containers 
- Understand build context
- Pipe Dockerfile through stdin 
- Exclude with .dockerignore 
- Use multi-stage builds
- Don’t install unnecessary packages 
- Decouple applications
- Minimize the number of layers
- Sort multi-line arguments
- Leverage build cache
-----------------------------
## Dockerfile instructions
- FROM 
- LABEL
- RUN 
- CMD
- EXPOSE
- ENV
- ADD or COPY
- ENTRYPOINT
- VOLUME 
- USER
- WORKDIR 
- ONBUILD
