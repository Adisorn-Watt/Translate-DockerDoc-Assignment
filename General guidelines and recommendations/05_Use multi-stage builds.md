## Use multi-stage builds 
#### การใช้ multi-stage builds

**[Multi-stage builds](https://docs.docker.com/develop/develop-images/multistage-build/)** สามารถช่วยลดขนาดของ image สุดท้ายได้อย่างมาก โดยการกระทำนี้จะไม่พยายามลดจำนวนของเลเยอร์และไฟล์กลาง เนื่องจากimage ที่ถูกสร้างใน stage สุดท้ายของการสร้างนั้นคุณสามารถลดขนาดเยอร์ต่างๆของ image ให้ได้มากที่สุดโดยการใช้ **[leveraging build cache](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#leverage-build-cache)** กล่าวคือคุณไม่จำเป็นที่จะต้องทำการสร้าง iamge ใหม่ทุกครั้งแต่การใช้ Multi-stage builds จะเป็นการคัดลอก image ที่เคยถูกสร้างไว้แล้วมาใช้กับ stage ใหม่แทน

ยกตัวอย่างเช่น หากการ build ของคุณบรรจุเลเยอร์หลายๆชั้น คุณสามารถออกคำสั่งพวกมันจากการเปลี่ยนแปลงเล็กน้อย (เพื่อรับรองว่าการสร้าง cache สามารถนำมาใช้ซ้ำได้) เป็นการเปลี่ยนแปลงที่บ่อยมากขึ้น ดังต่อไปนี้:
* ติดตั้งเครื่องมือต่างๆที่คุณจำเป็นต้องใช้ในการ build ใน application ของคุณ
* ติดตั้งหรืออัพเดท library ที่จำเป็น
* ทำการ generate application ของคุณ

Dockerfile สำหรับ Go appliacation จะมีลักษณะดังนี้

```
FROM golang:1.11-alpine AS build

# ติดตั้งเครื่องมือที่จำเป็นสำหรับ project
# Run `docker build --no-cache .` เพื่ออัพเดท dependencies
RUN apk add --no-cache git
RUN go get github.com/golang/dep/cmd/dep

# List project dependencies กับ Gopkg.toml และ Gopkg.lock
# เลเยอร์เหล่านี้เป็นเพียงแค่การ re-built เมื่อ Gopkg files อัพเดทเรียบร้อยแล้ว
COPY Gopkg.lock Gopkg.toml /go/src/project/
WORKDIR /go/src/project/
# ติดตั้ง library ที่จำเป็น
RUN dep ensure -vendor-only

# คัดลอก project ทั้งหมดและทำการ build
# เลเยอร์นี้คือการ rebuilt เมื่อไฟล์มีการเปลี่ยนแปลงใน project directory
COPY . /go/src/project/
RUN go build -o /bin/project

# นี่คือผลลัพธ์ใน single layer image
FROM scratch
COPY --from=build /bin/project /bin/project
ENTRYPOINT ["/bin/project"]
CMD ["--help"]
```