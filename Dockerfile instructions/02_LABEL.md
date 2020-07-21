# Label

ข้อมูลเพิ่มเติม : <https://docs.docker.com/config/labels-custom-metadata/>

- เราสามารถเพิ่ม Label ให้กับ image ได้ เพื่อช่วยในการจัดการต่างๆในโปรเจค บันทึกข้อมูล license ต่างๆ
  การทำ automation ต่างๆ รวมถึงเหตุผลอื่นๆ
- ในแต่ละ Label ให้เพิ่มบรรทัดที่ขึ้นต้นด้วย

```
LABEL
```

และต่อด้วยค่า key-value pairs อย่างน้อย 1 คู่ (หรือมากกว่า 1 คู่ก็ได้)

#### ตัวอย่างต่อไปนี้ เป็นรูปแบบต่างๆที่สามารถใช้ได้ รวมถึงมีการเขียนอธิบายไว้ใน comments แล้ว

\*\* สตริงที่มีช่องว่างจะต้องอยู่ในเครื่องหมายอัญประกาศ (")

```
# กำหนดแต่ละ Label (เขียนแยกกัน)
LABEL com.example.version="0.0.1-beta"
LABEL vendor1="ACME Incorporated"
LABEL vendor2=ZENITH\ Incorporated
LABEL com.example.release-date="2015-02-12"
LABEL com.example.version.is-production=""
```

- ในหนึ่ง image สามารถมี Label ได้มากกว่า 1 Label
- ก่อนหน้า Docker 1.10 ควรรวม labels ทั้งหมดไว้ในคำสั่ง

```
LABEL
```

```
# กำหนด Label หลายอันในบรรทัดเดียว
LABEL com.example.version="0.0.1-beta" com.example.release-date="2015-02-12"
```

ซึ่งโค้ดด้านบนสามารถเขียนอีกแบบได้ดังนี้

```
# กำหนด Label หลายอันในครั้งเดียว โดยการใช้เครื่องหมาย Backslash
LABEL vendor=ACME\ Incorporated \
      com.example.is-beta= \
      com.example.is-production="" \
      com.example.version="0.0.1-beta" \
      com.example.release-date="2015-02-12"
```

- สามารถดูข้อมูลเพิ่มเติมได้ที่ <https://docs.docker.com/config/labels-custom-metadata/>
  ใช้สำหรับเป็น guidelines เกี่ยวกับ label keys และ values
- สำหรับข้อมูลเกี่ยวกับ querying labels ที่เกี่ยวข้องกับ filtering ดูได้ใน <https://docs.docker.com/config/labels-custom-metadata/#manage-labels-on-objects>
- สำหรับข้อมูล LABEL ใน Dockerfile reference เพิ่มเติมอื่นๆที่ <https://docs.docker.com/engine/reference/builder/#label>
