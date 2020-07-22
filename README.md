# Best practices for writing Dockerfiles

## Introduction
ฺเอกสารนี้ครอบคลุมแนวทางปฏิบัติและวิธีการที่ดีที่สุดที่แนะนำสำหรับการสร้าง image ที่มีประสิทธิภาพ...\
อ่านต่อที่ [Introduction.md](../blob/Introduction.md)

## General guidelines and recommendations
- [Create ephemeral containers](General%20guidelines%20and%20recommendations/01_Create%20ephemeral%20containers.md)
- [Understand build context](General%20guidelines%20and%20recommendations/02_Understand%20build%20context.md)
- [Pipe Dockerfile through stdin](General%20guidelines%20and%20recommendations/03_Pipe%20Dockerfile%20through%20stdin.md)
- [Exclude with .dockerignore](General%20guidelines%20and%20recommendations/04_Exclude%20with%20.dockerignore.md)
- [Use multi-stage builds](General%20guidelines%20and%20recommendations/05_Use%20multi-stage%20builds.md)
- [Don’t install unnecessary packages](General%20guidelines%20and%20recommendations/06_Don’t%20install%20unnecessary%20packages.md)
- [Decouple applications](General%20guidelines%20and%20recommendations/07_Decouple%20applications.md)
- [Minimize the number of layers](General%20guidelines%20and%20recommendations/08_Minimize%20the%20number%20of%20layers.md)
- [Sort multi-line arguments](General%20guidelines%20and%20recommendations/09_Sort%20multi-line%20arguments.md)
- [Leverage build cache](General%20guidelines%20and%20recommendations/10_Leverage%20build%20cache.md)
-----------------------------
## Dockerfile instructions
- [FROM](Dockerfile%20instructions/01_FROM.md)
- [LABEL](Dockerfile%20instructions/02_LABEL.md)
- [RUN](Dockerfile%20instructions/03_RUN.md)
- [CMD](Dockerfile%20instructions/04_CMD.md)
- [EXPOSE](Dockerfile%20instructions/05_EXPOSE.md)
- [ENV](Dockerfile%20instructions/06_ENV.md)
- [ADD or COPY](Dockerfile%20instructions/07_ADD%20or%20COPY.md)
- [ENTRYPOINT](Dockerfile%20instructions/08_ENTRYPOINT.md)
- [VOLUME](Dockerfile%20instructions/09_VOLUME.md)
- [USER](Dockerfile%20instructions/10_USER.md)
- [WORKDIR](Dockerfile%20instructions/11_WORKDIR.md)
- [ONBUILD](Dockerfile%20instructions/12_ONBUILD.md)

*Translate from: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#exclude-with-dockerignore*
