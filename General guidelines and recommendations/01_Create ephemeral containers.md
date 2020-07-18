## Create ephemeral containers
(ยังเกลาคำแปลไม่เสร็จ)Image ที่กำหนดค่าโดย Dockerfile ควรที่จะทำให้เกิดการสร้าง container ที่มีความ ephemeral (ไม่จีรัง) ที่สุดเท่าที่จะเป็นไปได้ โดยคำว่า “ephemeral” ในที่นี้ หมายถึง container นั้นต้องสามารถหยุด และทำลายได้ จากนั้นก็สร้างใหม่ และแทนที่ด้วยการตั้งค่าและการกำหนดค่าขั้นต่ำที่แน่นอน

อ้างถึง [Processes](https://12factor.net/processes) ภายใต้วิธีการของ *The Twelve-factor App* เพื่อให้เกิดความรู้สึกถึงแรงจูงใจในการรัน container ในแบบ stateless fashion