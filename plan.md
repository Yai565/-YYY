# แผนพัฒนา Hiro AR Iron Man

## ขอบเขต
เว็บ AR แบบ static-first ใน `index.html` ตรวจจับ Hiro marker ด้วย AR.js + A-Frame เมื่อพบ marker จะแสดงตัวละคร Iron Man แบบ procedural 3D ที่ประกอบด้วยวัสดุโลหะ แสงเตาปฏิกรณ์ และเอฟเฟกต์ไอพ่น พร้อมปุ่มบินและวิ่งบนหน้าจอมือถือ ใช้งานได้ผ่าน HTTPS และนำขึ้น GitHub Pages ได้

## การตัดสินใจทางเทคนิค
- ใช้ A-Frame 1.5.x และ AR.js 3.4.x จาก CDN เพื่อให้เปิด `index.html` ได้โดยไม่ต้อง build bundle
- ใช้ `a-marker preset="hiro"` จึงรองรับ Hiro marker มาตรฐานตามภาพอ้างอิงของผู้ใช้
- โมเดล Iron Man สร้างด้วย custom A-Frame component และ Three.js primitives ในหน้าเว็บ เพื่อลดความเสี่ยงจากไฟล์โมเดลภายนอก/ลิงก์หมดอายุ ขณะยังมีวัสดุ PBR, แสงสีแดง-ทอง, arc reactor และ thruster glow
- ใช้ custom animation component คุม idle, run และ flight โดยให้ตำแหน่งโมเดลยึดกับ marker เสมอ และเพิ่มการเคลื่อนที่ไปด้านหน้าสัมพันธ์กับพื้นที่ AR เมื่อวิ่ง
- ใช้ปุ่มแบบ touch-friendly พร้อมสถานะ `aria-pressed`, ข้อความสถานะการตรวจจับ marker/กล้อง และ fallback หากเบราว์เซอร์ไม่รองรับกล้อง
- ใช้ Node `server.mjs` สำหรับ Preview ที่พอร์ต 3000 แต่ไฟล์หน้าเว็บและ asset อยู่ที่ root พร้อมนำไป GitHub Pages ได้ทันที

## Design direction
- **Design Movement:** tactical HUD / cinematic suit interface ผสมแผงควบคุม sci-fi แบบห้องนักบิน
- **Core Principles:** อ่านง่ายในแสงกลางแจ้ง, ข้อมูลเป็นชั้น, ปุ่มตอบสนองทันที, เอฟเฟกต์มีหน้าที่บอกสถานะ
- **Color Philosophy:** พื้นดำโปร่งเพื่อไม่กลบภาพกล้อง, cyan ใช้เป็นสัญญาณระบบพร้อมใช้งาน, amber ใช้กับสถานะกำลังตรวจจับ, red ใช้กับการบิน/พลังงาน เพื่อให้ผู้ใช้เข้าใจได้จากสีเดียว
- **Layout Paradigm:** กล้องเต็มจอเป็นผืนหลัก, HUD วางตามขอบและมีแถบสถานะลอยด้านบน, ปุ่มควบคุมยึดขอบล่างแบบ thumb zone แทนการ์ดกลางจอ
- **Signature Elements:** กรอบมุมแบบ reticle, เส้นสแกน marker, แถบพลังงาน arc-reactor และวงแหวนปุ่มคำสั่ง
- **Interaction Philosophy:** แตะเพื่อสลับโหมด, กดแล้วเห็นแรงตอบสนองทันทีผ่าน glow/label/status, หยุดวิ่งก่อนบินเพื่อกันสถานะขัดแย้ง
- **Animation:** idle หายใจและ reactor pulse เบา ๆ; run ขยับแขนขาและ bob; flight ยกตัวพร้อมเอียงไปข้างหน้าและเปิด thruster particles; ทุกเอฟเฟกต์ต้องลดความถี่เมื่อ `prefers-reduced-motion`
- **Typography System:** ใช้ `Rajdhani` สำหรับ HUD และ `Inter` สำหรับคำอธิบาย/ปุ่ม ให้ตัวเลขและ label มีลักษณะ technical แต่ยังอ่านง่าย
- **Brand Essence:** "เปลี่ยน Hiro marker ให้เป็นฐานปล่อยพลังของ Iron Man ในไม่กี่วินาที" — cinematic, responsive, playful
- **Brand Voice:** สั้น กระชับ แบบระบบภารกิจ เช่น “เล็ง Hiro marker ให้อยู่ในกรอบ” และ “ระบบขับดันพร้อมใช้งาน”
- **Wordmark & Logo:** เครื่องหมาย arc-reactor แบบวงแหวนสามชั้นข้างคำว่า `HIRO / IRON MAN`
- **Signature Brand Color:** Reactor Cyan `#5cf5ff`

## โครงสร้างโปรเจกต์
- `index.html` — ฉาก A-Frame/AR.js, HUD, ปุ่มควบคุม, component และ animation logic
- `server.mjs` — static server สำหรับ Preview และการเปิดจากเครื่องตัวเอง
- `public/manus-routes.json` — route manifest ของหน้า `/`
- `assets/hiro-marker-reference.png` — marker reference ที่ผู้ใช้แนบมา
- `README.md` — วิธีเปิดกล้อง, พิมพ์ marker, เปิด GitHub Pages และข้อจำกัดของ browser
- `TODO.md` — acceptance outcomes ของคำขอ
- `app.config.ts` — project logo metadata สำหรับ checkpoint

## ข้อจำกัดที่บันทึกไว้
- กล้องมือถือและ WebAR ต้องเปิดผ่าน HTTPS หรือ localhost และผู้ใช้ต้องอนุญาตกล้อง
- การตรวจจับแบบ marker-based ต้องเห็น Hiro marker ชัด มีแสงพอ และไม่บิดเบี้ยว
- โมเดล procedural มีลักษณะเกราะ Iron Man ที่ปรับแต่งได้ แต่ไม่ใช่ไฟล์ภาพยนตร์/ลิขสิทธิ์จาก Marvel โดยตรง
- การเชื่อม repository ไปยัง GitHub ต้องผ่านการยืนยันของเจ้าของโปรเจกต์ใน WebDev Git flow; session นี้ยังไม่ได้เชื่อมบัญชี GitHub
