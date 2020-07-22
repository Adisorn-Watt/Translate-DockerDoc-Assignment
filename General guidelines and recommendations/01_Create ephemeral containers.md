## Create ephemeral containers
Image ที่กำหนดค่าโดย `Dockerfile` ของคุณ ควรที่จะทำให้เกิดการสร้าง container ที่มีความไม่จีรัง (ephemeral) ให้มากที่สุดเท่าที่จะเป็นไปได้ โดยคำว่า “ephemeral” ในที่นี้ หมายถึง container นั้นต้องสามารถที่จะถูกหยุด และถูกลบทิ้งได้ จากนั้นก็ถูกสร้างใหม่ และถูกแทนที่ได้ด้วยการตั้งค่า และการ set up ให้น้อยที่สุดแต่ครบถ้วนสมบูรณ์ 

อ้างถึง [Processes](https://12factor.net/processes) ภายใต้ระเบียบวิธีการของ *The Twelve-factor App* เพื่อให้รู้สึกถึงมูลเหตุจูงใจในการรัน container ในแบบไร้สถานะ (stateless fashion)