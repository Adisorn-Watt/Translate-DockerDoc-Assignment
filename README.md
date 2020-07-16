*Best practices for writing Dockerfiles

**Introduction
**General guidelines and recommendations
- Create ephemeral containers (น้อย)
- Understand build context
- Pipe Dockerfile through stdin (เยอะ)

- Exclude with .dockerignore (น้อย)
- Use multi-stage builds
- Don’t install unnecessary packages (น้อย)
- Decouple applications
- Minimize the number of layers

- Sort multi-line arguments
- Leverage build cache
* Dockerfile instructions
- FROM (น้อย)
- LABEL

- RUN (เยอะ)
- CMD
- EXPOSE
- ENV
- ADD or COPY

- ENTRYPOINT
- VOLUME (น้อย)
- USER
- WORKDIR (น้อย)
- ONBUILD