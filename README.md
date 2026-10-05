# HIRO / IRON MAN — Web AR

เว็บ AR แบบ marker-based สำหรับมือถือ เมื่อกล้องพบ **Hiro marker มาตรฐาน** จะวาง Iron Man แบบ procedural 3D ที่ยึดกับ marker และมีแอนิเมชันพร้อมคำสั่งบนหน้าจอ 2 แบบ:

- **RUN** — เล่นแอนิเมชันวิ่งและเคลื่อนที่ไปข้างหน้าบนระนาบของ marker
- **FLY** — ยกตัวละครขึ้น เอียงตัว และเปิดเปลวไฟ/แสงไอพ่นใต้เท้า

โมเดลในโปรเจกต์เป็นชุดชิ้นส่วน Three.js ที่สร้างขึ้นใน `index.html` จึงไม่ต้องดาวน์โหลดไฟล์โมเดลจากภายนอก และไม่ใช่ไฟล์ภาพยนตร์/ทรัพย์สินทางปัญญาอย่างเป็นทางการของ Marvel

## เปิดในเครื่อง

ต้องใช้ HTTPS หรือ `localhost` เพราะเบราว์เซอร์จะไม่อนุญาตกล้องจาก HTTP ทั่วไป

```bash
npm run dev
```

จากนั้นเปิด `http://localhost:3000` บนคอมพิวเตอร์ หรือใช้ HTTPS tunnel/เซิร์ฟเวอร์ที่รองรับเพื่อทดสอบบนมือถือจริง

## วิธีใช้งาน

1. พิมพ์หรือแสดง Hiro marker ให้มีขอบดำและพื้นขาวชัดเจน — ภาพอ้างอิงอยู่ที่ `assets/hiro-marker-reference.png`
2. เปิดเว็บด้วย Safari/Chrome บนมือถือผ่าน HTTPS
3. อนุญาตการใช้กล้องหลัง
4. เล็ง Hiro marker ให้อยู่ใน reticle จนสถานะเปลี่ยนเป็น `MARKER LOCKED`
5. แตะ `RUN` เพื่อให้ Iron Man วิ่งไปข้างหน้า หรือแตะ `FLY` เพื่อเปิด thruster และโหมดบิน
6. แตะปุ่มเดิมอีกครั้งเพื่อกลับสู่โหมด standby

## GitHub Pages

โปรเจกต์นี้เป็น static site และมี `index.html` อยู่ที่ root จึงนำขึ้น GitHub ได้ตรง ๆ:

```bash
git init
git add .
git commit -m "Create Hiro AR Iron Man experience"
git branch -M main
git remote add origin https://github.com/<username>/<repository>.git
git push -u origin main
```

ใน GitHub ไปที่ **Settings → Pages → Deploy from a branch → `main` / `/ (root)` → Save** แล้วรอให้ GitHub ออกลิงก์ HTTPS จากนั้นเปิดลิงก์บนมือถือและอนุญาตกล้อง

`server.mjs` มีไว้สำหรับ Manus Preview/ทดสอบในเครื่องเท่านั้น; GitHub Pages จะเสิร์ฟ `index.html` และ assets โดยตรง ไม่ต้องรัน Node บน Pages

## ไฟล์สำคัญ

| ไฟล์ | หน้าที่ |
| --- | --- |
| `index.html` | A-Frame + AR.js scene, HUD, controls และ Iron Man procedural component |
| `server.mjs` | static server ที่พอร์ต 3000 สำหรับ Preview |
| `assets/hiro-marker-reference.png` | ภาพ Hiro marker ที่ผู้ใช้แนบมา |
| `public/manus-routes.json` | route manifest สำหรับ WebDev |
| `plan.md` | แผนสถาปัตยกรรมและทิศทางการออกแบบ |
| `TODO.md` | ผลลัพธ์ที่ต้องส่งมอบ |

## หมายเหตุการทดสอบ

- ต้องมีอินเทอร์เน็ตครั้งแรกเพื่อโหลด A-Frame และ AR.js จาก CDN
- กล้องควรมองเห็น marker ตรง ๆ มีแสงพอ และไม่สะท้อนแสง
- หาก marker หลุดจากเฟรม ระบบจะหยุดการสั่งงานใหม่จนกว่าจะ lock marker อีกครั้ง
- ประสิทธิภาพขึ้นกับมือถือและเบราว์เซอร์ โดยเฉพาะการเรนเดอร์ 3D พร้อม video background
