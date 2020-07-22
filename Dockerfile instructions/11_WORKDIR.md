# WORKDIR

ไฟล์อ้างอิง Docker สำหรับคำแนะนำของ WORKDIR : <https://docs.docker.com/engine/reference/builder/#workdir/>

เพื่อความชัดเจนและความน่าเชื่อถือคุณควรใช้เส้นทางที่แน่นอนสำหรับ `WORKDIR` ของคุณ นอกจากนี้คุณควรใช้ `WORKDIR` แทนคำแนะนำที่เพิ่มขึ้นเช่น `RUN cd … && do-something` ซึ่งยากต่อการอ่านแก้ไขและบำรุงรักษา
