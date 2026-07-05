# ร้านบุญเลิศ เมล็ดพันธุ์ข้าว — เว็บไซต์

เว็บไซต์ธุรกิจ (static site, Mobile First) สำหรับร้านบุญเลิศ เมล็ดพันธุ์ข้าว
อำเภอเก้าเลี้ยว จังหวัดนครสวรรค์ — "คัดเมล็ดพันธุ์ดี เพื่อผลผลิตที่มั่นใจ"

> **หมายเหตุสำคัญ:** เว็บไซต์นี้สร้างเป็น **HTML/CSS/JS แบบ static ไฟล์เดียว** (ไม่มี build step,
> ไม่ต้องใช้ Node/Next.js ในการรัน) เพื่อให้ deploy ได้ทันทีโดยไม่ต้องติดตั้ง dependency ใดๆ
> หากต้องการเวอร์ชัน Next.js + React + TypeScript เต็มรูปแบบ สามารถแจ้งเพื่อทำเฟสถัดไปได้

## โครงสร้างไฟล์

```
RiceSeedWebsite/
├── index.html          ← หน้าเว็บหลักทั้งหมด (HTML+CSS+JS ในไฟล์เดียว)
├── manifest.json        ← PWA manifest (ติดตั้งเป็นแอปบนมือถือได้)
├── robots.txt
├── sitemap.xml
├── README.md
└── public/
    ├── products.json     ← ข้อมูลสินค้า (CMS-ready, แก้ไข/เพิ่มพันธุ์ข้าวได้ที่นี่)
    └── icons/
        ├── favicon.svg
        ├── icon-192.png  ← ไอคอน PWA (placeholder ไอคอน สามารถเปลี่ยนภายหลัง)
        └── icon-512.png
```

## วิธีเปิดดูตอนพัฒนา (Local Preview)

เนื่องจากหน้าเว็บโหลดข้อมูลสินค้าจาก `public/products.json` ด้วย `fetch()`
การเปิดไฟล์ `index.html` ตรงๆ แบบ `file://` อาจถูกเบราว์เซอร์บล็อก (CORS)
แนะนำให้รันเซิร์ฟเวอร์เล็กๆ ระหว่างพัฒนา:

```bash
# ตัวเลือกที่ 1: ใช้ Python (มีมาให้ในเครื่องส่วนใหญ่)
python3 -m http.server 8080

# ตัวเลือกที่ 2: ใช้ Node
npx serve .
```

จากนั้นเปิดเบราว์เซอร์ไปที่ `http://localhost:8080`

(เมื่อ deploy ขึ้น GitHub Pages / Vercel / Netlify จริง จะไม่มีปัญหานี้
เพราะเสิร์ฟผ่าน http/https ตามปกติ — ถ้าเปิดแบบ file:// แล้ว fetch พลาด
หน้าเว็บจะ fallback ไปใช้ข้อมูลสินค้าที่ฝังไว้ในโค้ดแทนอัตโนมัติ)

## การแก้ไขเนื้อหา

| ต้องการแก้ไข | แก้ที่ไฟล์ |
|---|---|
| เพิ่ม/ลบ/แก้พันธุ์ข้าว | `public/products.json` (และ fallback array ท้าย `index.html` ถ้าต้องการให้ตรงกันตอนเปิดแบบ file://) |
| เบอร์โทร / พื้นที่บริการ | ค้นหา `089-959-3342` ใน `index.html` แล้วแทนที่ทุกจุด |
| ลิงก์ Google Maps | แก้ตัวแปร `mapLink.href` ใน `<script>` ท้ายไฟล์ และในกล่อง `#mapBox` |
| ลิงก์ Line/Facebook | แก้ `fabLine.href` และลิงก์ใน footer |
| สี/ฟอนต์/ธีม | แก้ตัวแปร CSS ใน `:root` บนสุดของ `<style>` |
| คำถามที่พบบ่อย | แก้ในส่วน `<div id="faqList">` |
| รีวิวลูกค้า | แก้ในส่วน `.review-track` |

## Deployment

### GitHub Pages
1. สร้าง repository ใหม่ชื่อ `RiceSeedWebsite` แล้ว push โค้ดทั้งหมดขึ้นไป
2. ไปที่ Settings → Pages → Source เลือก branch `main` และโฟลเดอร์ `/ (root)`
3. รอสักครู่ เว็บไซต์จะออนไลน์ที่ `https://<username>.github.io/RiceSeedWebsite/`

```bash
git init
git add .
git commit -m "Initial commit: Bunlert Rice Seed website"
git branch -M main
git remote add origin https://github.com/<username>/RiceSeedWebsite.git
git push -u origin main
```

### Vercel
1. ไปที่ [vercel.com](https://vercel.com) → New Project → Import จาก GitHub repo นี้
2. Framework Preset เลือก **Other** (เพราะเป็น static site ไม่มี build step)
3. Deploy ได้ทันที

### Netlify
1. ลาก-วางโฟลเดอร์ `RiceSeedWebsite` เข้า Netlify Drop (https://app.netlify.com/drop)
   หรือเชื่อมต่อ GitHub repo แล้ว deploy โดยไม่ต้องตั้งค่า build command ใดๆ

## สิ่งที่รวมอยู่แล้ว

- ✅ Mobile First / Responsive ทุกขนาดหน้าจอ ไม่มี horizontal scroll
- ✅ Sticky header + Bottom navigation บนมือถือ
- ✅ Floating action buttons (โทร / Line / Google Maps / กลับด้านบน)
- ✅ Hero พร้อม motion signature: ทุ่งนาเคลื่อนไหวเบาๆ + พระอาทิตย์ยามเช้า (เคารพ `prefers-reduced-motion`)
- ✅ ส่วนสินค้าโหลดจาก JSON (CMS-ready) พร้อมลิงก์สินค้าจริงจาก kla-krang.com
- ✅ FAQ แบบ Accordion, รีวิวแบบเลื่อนแนวนอน, ขั้นตอนสั่งซื้อแบบ Timeline
- ✅ SEO: title, meta description, keywords, Open Graph, Twitter Card, Schema.org (LocalBusiness/Store)
- ✅ robots.txt, sitemap.xml, manifest.json (PWA installable)
- ✅ Scroll-reveal animation แบบเบาๆ ด้วย IntersectionObserver

## สิ่งที่ควรทำต่อ (Future Ready)

- แทนที่ไอคอน `icon-192.png` / `icon-512.png` ด้วยโลโก้จริงของร้าน
- ใส่ลิงก์ Google Maps, Line OA, Facebook Page จริง
- เปลี่ยนรีวิวลูกค้าให้เป็นข้อมูลจริง
- หากต้องการระบบหลังบ้าน/CMS/ระบบสั่งซื้อออนไลน์ในอนาคต โครงสร้างข้อมูลสินค้าที่แยกเป็น `products.json`
  ทำให้ย้ายไป backend/CMS ได้ง่ายโดยไม่ต้องแก้โครงหน้าเว็บ
