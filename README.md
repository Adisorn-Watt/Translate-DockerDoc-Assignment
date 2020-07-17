# Best practices for writing Dockerfiles

## Introduction
ฺBla Bla Bla Bla Bla Bla Bla Bla Bla Bla Bla Bla
Bla Bla Bla Bla Bla Bla Bla Bla Bla Bla Bla Bla

## General guidelines and recommendations
- Create ephemeral containers 
- Understand build context
- Pipe Dockerfile through stdin 
-----------------------------
- Exclude with .dockerignore 
- Use multi-stage builds
- Don’t install unnecessary packages 
- Decouple applications
- Minimize the number of layers
-----------------------------
- Sort multi-line arguments
- Leverage build cache
- Dockerfile instructions
- FROM 
- LABEL
-----------------------------
- RUN 
- CMD
- EXPOSE
- ENV
- ADD or COPY
-----------------------------
- ENTRYPOINT
- VOLUME 
- USER
- WORKDIR 
- ONBUILD