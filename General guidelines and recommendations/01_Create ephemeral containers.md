## Create ephemeral containers
Image ที่กำหนดค่าโดย `Dockerfile` ของคุณ ควรที่จะทำให้เกิดการสร้าง container ที่มีความ ephemeral (ไม่จีรัง) ที่สุดเท่าที่จะเป็นไปได้ โดยคำว่า “ephemeral” ในที่นี้ หมายถึง container นั้นต้องสามารถที่จะถูกหยุด และถูกลบทิ้งได้ จากนั้นก็ถูกสร้างใหม่ และถูกแทนที่ได้ด้วยการตั้งค่า และการ set up ขั้นต่ำที่แน่นอนครบถ้วน 

อ้างถึง [Processes](https://12factor.net/processes) ภายใต้วิธีการของ *The Twelve-factor App* เพื่อให้เกิดความรู้สึกถึงแรงจูงใจในการรัน container ในแบบ stateless fashion