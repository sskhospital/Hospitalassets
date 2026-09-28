# 🩺 ระบบซ่อมบำรุงครุภัณฑ์ โรงพยาบาลสังขละบุรี (Sangkhlaburi Hospital Equipment Maintenance System)

ระบบบริหารจัดการและแจ้งซ่อมบำรุงครุภัณฑ์ทางการแพทย์และทั่วไปแบบออนไลน์สำหรับโรงพยาบาลสังขละบุรี รองรับการทำงานแบบเรียลไทม์ผ่าน **Firebase Realtime Database** พร้อมกำหนดค่าสำหรับ Deploy บน **GitHub Pages** และ **Firebase Hosting**

---

## 🌟 คุณสมบัติเด่นของระบบ
- **Realtime Database Synchronization**: ข้อมูลงานซ่อม อะไหล่ สถานะการอนุมัติ และสถิติ อัปเดตพร้อมกันแบบเรียลไทม์ทันทีทุกเครื่อง/ทุกแท็บ
- **Single Page Application (SPA)**: ใช้งานได้ทันทีโดยไม่ต้องติดตั้ง Backend หรือ Node.js ทำงานบนเว็บเบราว์เซอร์ได้ 100%
- **Offline First**: มีระบบ Fallback ใช้งานผ่าน `LocalStorage` เมื่อไม่มีสัญญาณอินเทอร์เน็ต และซิงค์ขึ้น Cloud อัตโนมัติเมื่อเชื่อมต่อ
- **รองรับ 4 ระดับสิทธิ์ผู้ใช้งาน**:
  - `admin` (ผู้ดูแลระบบ)
  - `head` (ผู้บริหาร / ผู้อนุมัติ)
  - `tech` (ช่างเทคนิค)
  - `staff` (เจ้าหน้าที่แจ้งซ่อม)
- **ระบบออกเอกสาร & รายงาน**:
  - พิมพ์ใบแจ้งส่งซ่อมครุภัณฑ์ (PDF)
  - พิมพ์รายงานสรุปสำหรับผู้บริหาร (Executive BI Report)
  - Export ข้อมูลเป็น Excel ด้วยการคลิกเพียงครั้งเดียว

---

## 📁 โครงสร้างโปรเจกต์ (Project Structure)

```text
├── index.html                               # หน้าหลักของระบบ (รองรับ GitHub Pages & Firebase Hosting)
├── ระบบซ่อมบำรุงครุภัณฑ์ รพ.สังขละบุรี (Copy)(1).html  # ไฟล์ต้นฉบับภาษาไทย
├── firebase.json                            # การตั้งค่า Firebase Hosting และ Database Rules
├── .firebaserc                              # การตั้งค่า Firebase Project Target
├── database.rules.json                      # กฎความปลอดภัย (Rules) ของ Firebase Realtime Database
├── .nojekyll                                # ปิดการทำงานของ Jekyll บน GitHub Pages
├── .github/
│   └── workflows/
│       ├── deploy-gh-pages.yml              # GitHub Actions สำหรับ Deploy ไปยัง GitHub Pages
│       └── firebase-hosting-merge.yml       # GitHub Actions สำหรับ Deploy ไปยัง Firebase Hosting
└── README.md                                # คู่มือการติดตั้งและใช้งาน
```

---

## 🚀 1. การ Deploy บน GitHub Pages

GitHub Pages รองรับการรันไฟล์ `index.html` ที่อยู่ที่รากโปรเจกต์โดยตรง

### วิธีที่ 1: Deploy ผ่าน GitHub Settings (แนะนำ สะดวกที่สุด)
1. Push โปรเจกต์นี้ขึ้นไปที่ GitHub Repository ของคุณ
2. ไปที่เมนู **Settings** ของ Repository บน GitHub
3. ในแถบด้านซ้าย เลือกหัวข้อ **Pages**
4. ภายใต้ **Build and deployment > Source**:
   - เลือก **Deploy from a branch**
   - Branch: เลือก `main` หรือ `master` และโฟลเดอร์ `/ (root)`
   - กด **Save**
5. รอประมาณ 1-2 นาที เว็บไซต์จะออนไลน์ที่:
   ```
   https://<your-username>.github.io/<repository-name>/
   ```

### วิธีที่ 2: Deploy อัตโนมัติด้วย GitHub Actions
ไฟล์ `.github/workflows/deploy-gh-pages.yml` ได้ถูกเตรียมไว้แล้ว เมื่อเปิดใช้งาน **GitHub Actions** ในเมนู **Settings > Pages > Source: GitHub Actions** ทุกครั้งที่มีการ Push โค้ด ระบบจะ Build และ Deploy ขึ้น GitHub Pages ให้โดยอัตโนมัติ

---

## 🔥 2. การ Deploy บน Firebase Hosting

### ขั้นตอนการเตรียมและ Deploy:
1. **ติดตั้ง Firebase CLI** (หากยังไม่ได้ติดตั้ง):
   ```bash
   npm install -g firebase-tools
   ```
2. **เข้าสู่ระบบ Firebase**:
   ```bash
   firebase login
   ```
3. **กำหนดโปรเจกต์ Firebase ของคุณ**:
   เปิดไฟล์ `.firebaserc` และเปลี่ยน `sangkhlaburi-hospital-repair` เป็น Project ID ของคุณ:
   ```json
   {
     "projects": {
       "default": "YOUR_FIREBASE_PROJECT_ID"
     }
   }
   ```
4. **Deploy ขึ้น Firebase Hosting & Database Rules**:
   ```bash
   firebase deploy
   ```
   หรือ Deploy เฉพาะ Hosting:
   ```bash
   firebase deploy --only hosting
   ```
5. เว็บไซต์จะออนไลน์ทันทีที่:
   ```
   https://<YOUR_FIREBASE_PROJECT_ID>.web.app
   ```

---

## ⚡ 3. การตั้งค่า Firebase Realtime Database ให้ข้อมูลอัปเดตสด (Realtime)

เพื่อให้ข้อมูลงานซ่อมและครุภัณฑ์อัปเดตแบบเรียลไทม์ระหว่างอุปกรณ์ของผู้บริหาร ช่าง และเจ้าหน้าที่ ให้ทำตามขั้นตอนดังนี้:

### ขั้นตอนที่ 1: สร้าง Realtime Database
1. เข้าไปที่ [Firebase Console](https://console.firebase.google.com/)
2. สร้างโปรเจกต์ หรือเลือกโปรเจกต์ของคุณ
3. ไปที่เมนูด้านซ้าย **Build > Realtime Database**
4. คลิก **Create Database** เลือก Location (เช่น `Singapore (asia-southeast1)`)
5. ในแท็บ **Rules** นำกฎจากไฟล์ `database.rules.json` ไปใส่:
   ```json
   {
     "rules": {
       ".read": true,
       ".write": true,
       "sk_db": {
         ".indexOn": ["date", "st", "by", "pri"]
       }
     }
   }
   ```
   แล้วกด **Publish**

### ขั้นตอนที่ 2: คัดลอกการตั้งค่า Web App (Config)
1. ไปที่ **Project Settings** (รูปเฟืองมุมซ้ายบน) > แท็บ **General**
2. เลื่อนลงมาที่หัวข้อ **Your apps** > คลิกไอคอนเว็บ `</>` เพื่อลงทะเบียน Web App
3. คัดลอกอ็อบเจ็กต์ `firebaseConfig` เช่น:
   ```json
   {
     "apiKey": "AIzaSy...",
     "authDomain": "your-project.firebaseapp.com",
     "databaseURL": "https://your-project-default-rtdb.asia-southeast1.firebasedatabase.app",
     "projectId": "your-project",
     "storageBucket": "your-project.appspot.com",
     "messagingSenderId": "123456789",
     "appId": "1:123456789:web:abcdef"
   }
   ```

### ขั้นตอนที่ 3: ใส่ Config ในระบบ
1. เปิดหน้าเว็บระบบซ่อมบำรุงครุภัณฑ์ (ไม่ว่าจะรันบนเครื่อง Local, GitHub Pages หรือ Firebase Hosting)
2. เข้าสู่ระบบด้วยบัญชีผู้ดูแลระบบ:
   - **Username**: `admin`
   - **Password**: `admin123`
3. ไปที่เมนู **🔌 Firebase & API**
4. วาง JSON Config ลงในช่อง **Firebase Web Configuration (JSON)**
5. คลิกปุ่ม **💾 บันทึกและเชื่อมต่อ Firebase**
6. สถานะมุมขวาบนและแถบเมนูจะเปลี่ยนเป็น:
   ```
   🟢 Firebase: เชื่อมต่อเรียลไทม์
   ```
7. คลิกปุ่ม **⬆️ ส่งข้อมูลขึ้น Firebase (Upload)** เพื่อให้ข้อมูลตั้งต้นขึ้นสู่คลาวด์ทันที

---

## 🧪 4. การทดสอบการอัปเดตแบบเรียลไทม์ (Live Testing)
1. เปิดเว็บไซต์ 2 หน้าต่างคู่กัน (หรือเปิดบนคอมพิวเตอร์ 1 เครื่อง และโทรศัพท์มือถือ 1 เครื่อง)
2. หน้าต่างที่ 1: เข้าสู่ระบบเป็น `staff` แล้วกด **🩺 แจ้งซ่อมออนไลน์**
3. หน้าต่างที่ 2: เข้าสู่ระบบเป็น `tech` หรือ `head` อยู่ที่หน้า **🛠 งานซ่อม** หรือ **📊 แดชบอร์ด**
4. **สังเกตผลลัพธ์**: รายการแจ้งซ่อมใหม่และตัวเลขบนแดชบอร์ดในหน้าต่างที่ 2 จะอัปเดตขึ้นมาทันทีโดย **ไม่ต้องกดรีเฟรชหน้าจอ (F5)**!

---

## 🔑 บัญชีผู้ใช้สำหรับทดสอบ (Demo Accounts)

| ชื่อผู้ใช้ (Username) | รหัสผ่าน (Password) | บทบาท (Role) | สิทธิ์การใช้งาน |
| :--- | :--- | :--- | :--- |
| `admin` | `admin123` | ผู้ดูแลระบบ | จัดการทุกส่วน, ตั้งค่าระบบ, จัดการผู้ใช้, ตั้งค่า Firebase |
| `head` | `head123` | ผู้บริหาร / ผู้อนุมัติ | ดูแดชบอร์ด BI, อนุมัติงานซ่อม, ดูรายงานสรุป |
| `tech` | `tech123` | ช่างเทคนิค | รับงานซ่อม, บันทึกการใช้อะไหล่, บันทึกผลซ่อมเสร็จ |
| `staff` | `staff123` | เจ้าหน้าที่แจ้งซ่อม | แจ้งซ่อมออนไลน์, ติดตามสถานะ, ทำแบบประเมินความพึงพอใจ |

---

## 📞 ข้อมูลหน่วยงาน
**โรงพยาบาลสังขละบุรี (Sangkhlaburi Hospital)**  
ระบบบริหารจัดการงานซ่อมบำรุงและครุภัณฑ์โรงพยาบาล
