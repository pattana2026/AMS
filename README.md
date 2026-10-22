# Variance Monitoring Dashboard
**by Pattana Chancheam**

---

## โครงสร้างไฟล์

```
D:\Variance Monitoring Dashboard\
├── Variance_Dashboard.html     ← Dashboard หลัก (ข้อมูลฝังอยู่ใน HTML)
├── fetch_and_update.py         ← Script ดึงข้อมูลจาก Excel + Push GitHub
├── fetch_and_update.bat        ← รัน Script ด้วยมือ
├── setup_schedule.bat          ← ตั้ง Task Scheduler 09:00 ทุกวัน
├── setup_github.bat            ← Setup GitHub (ใช้แล้ว)
├── atg_site.xlsx               ← รายชื่อสถานี ATG (BIR=N → badge 📵)
├── .gh_config                  ← GitHub credentials (hidden)
├── .gitignore                  ← ไฟล์ที่ Git ไม่ track
└── reports\                    ← วางไฟล์ Excel จาก AMS3 ที่นี่
```

---

## การใช้งานประจำวัน

### ทุกวัน (อัตโนมัติ 09:00)
1. AMS3 Export ไฟล์ Excel → วางใน `reports\`
2. Task Scheduler รัน `fetch_and_update.bat` อัตโนมัติ
3. Script ดึงข้อมูล → อัปเดต HTML → Push ขึ้น GitHub
4. ทุกคนเปิด URL เห็นข้อมูลใหม่ทันที

### URL สำหรับเข้าใช้งาน
```
https://variance-dashboard.github.io/variance-dashboard/Variance_Dashboard.html
```

### Username / Password
| Username | Password | Role |
|---|---|---|
| admin | admin123 | Admin |
| pattana | variance2026 | Admin |
| user | user1234 | Viewer |
| Unlock code | unlock2026 | — |

---

## Features ใน Dashboard

### KPI Row 1 (แถวบน)
- **สถานีทั้งหมด** — จำนวนสถานีในระบบ
- **เกินเกณฑ์ ±0.5%** — คลิกกรองได้
- **วิกฤต ±5%** — คลิกกรองได้
- **อยู่ในเกณฑ์** — คลิกกรองได้

### KPI Row 2 (แถวล่าง)
- **✅ มีหมายเหตุแล้ว** — คลิกกรองสถานีที่ดำเนินการแล้ว
- **⏳ ยังไม่มีหมายเหตุ** — คลิกกรองสถานีที่ยังค้าง
- **🔁 Comment ซ้ำ >2 วัน** — คลิกกรองสถานีที่ comment ซ้ำ

### Period Bar
- **วันล่าสุด** — แสดงเฉพาะรายวัน
- **ทั้งหมด** — แสดงทุกคอลัมน์ตั้งแต่ต้น
- **กำหนดเอง** — เลือกช่วงวันที่เอง

### ตารางสถานี
- คลิก **● รายวัน ▶** → ขยายดูรายสัปดาห์ รายเดือน สะสม
- คลิก **>** ขยายดูรายถัง
- Badge **📵 ไม่ได้เชื่อมต่อ POS** — 19 สถานีที่ไม่ได้ Link POS
- Badge **วิกฤต** — สถานีที่เกิน ±5%

### รายละเอียดถัง
- ยอดขาย / ผลต่าง / % รายวัน สัปดาห์ เดือน สะสม
- **✅ หมายเหตุ** — มีหมายเหตุแล้ว
- **🔁 ซ้ำ X วัน** — comment ซ้ำเกิน 2 วัน
- **⏳ ยังไม่ดำเนินการ** — ค่าเกินเกณฑ์แต่ยังไม่มีหมายเหตุ

---

## การตั้งค่า fetch_and_update.py

```python
SOURCE_FOLDER = "reports\"          # โฟลเดอร์วางไฟล์ Excel
ATG_SITE_FILE = "atg_site.xlsx"    # ไฟล์รายชื่อสถานี ATG
KEEP_DAYS     = 90                  # เก็บข้อมูลย้อนหลัง 90 วัน
MODE          = "append"            # append=เฉพาะวันใหม่, full=โหลดทั้งหมด
```

### เมื่อต้องการโหลดข้อมูลใหม่ทั้งหมด (Full Reload)
1. เปิด `fetch_and_update.py` ด้วย Notepad
2. แก้ `MODE = "full"`
3. รัน `fetch_and_update.bat`
4. แก้กลับเป็น `MODE = "append"`

---

## การ Push ขึ้น GitHub

```cmd
d:
cd "Variance Monitoring Dashboard"
git add -f Variance_Dashboard.html
git commit -m "Update dashboard"
git push
```

---

## การตั้ง Task Scheduler (ครั้งเดียว)
คลิกขวา `setup_schedule.bat` → **Run as Administrator**
→ Task ชื่อ `VarianceMonitoringDashboard_AutoImport` รัน 09:00 ทุกวัน

---

## ไฟล์ Excel Format (ic0500)

| คอลัมน์ | Index | ชื่อ |
|---|---|---|
| A | 0 | สถานี |
| B | 1 | ชื่อสถานี |
| D | 3 | วันที่ปิด |
| E | 4 | ถังน้ำมัน |
| I | 8 | ยอดขาย |
| N | 13 | ผลต่าง |
| Z | 25 | หมายเหตุ |

## atg_site.xlsx
- คอลัมน์ I = `BIR (IC05) ?`
- **Y** = เชื่อมต่อ POS ปกติ
- **N** = ไม่ได้เชื่อมต่อ POS → badge 📵

