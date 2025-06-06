# expanded-flutter-architecture

# สรุปสถาปัตยกรรม Flutter (Flutter Architectural Overview)

---

## 1. Flutter ใช้ Skia ในการวาดภาพบนหน้าจอโดยตรง

* Flutter ไม่ใช้ native UI components เช่น UIButton หรือ TextView
* ใช้ **Skia** เป็น graphic engine วาด UI ลงบน canvas เองทั้งหมด
* ข้อดี

  * UI มีหน้าตาเหมือนกันทุกแพลตฟอร์ม
  * ปรับแต่ง UI ได้อิสระ
  * เรนเดอร์เร็วและลื่นไหล
  * ไม่ต้องรอ native platform update เพื่อเปลี่ยน UI

---

## 2. Flutter ใช้ Dart ที่ compile แบบ Ahead-of-Time (AOT)

* ใน build release Dart ถูกแปลงเป็น machine code ล่วงหน้า (AOT)
* ผลลัพธ์คือ

  * ประสิทธิภาพสูง
  * เปิดแอปเร็ว
* ตอนพัฒนาใช้ **JIT (Just-in-Time)** เพื่อรองรับ Hot Reload

---

## 3. Flutter Engine เขียนด้วย C++ และใช้ Skia

* Engine เป็นหัวใจที่รับ widget tree จาก Dart แล้ววาดลงจอ
* ประกอบด้วย

  * **Skia** สำหรับวาด UI 2D
  * **LibTxt** สำหรับ render ข้อความ
  * ระบบ animation, scheduling
  * Platform channels สำหรับเชื่อมกับ native API

---

## 4. Widget เป็นแบบ Immutable และ UI แสดงผลขึ้นกับ State

* Widgets เป็น immutable (แก้ไขค่าไม่ได้)
* เมื่อ state เปลี่ยน Flutter จะสร้าง widget tree ใหม่โดยเรียก `build()`
* เปรียบเทียบ widget tree ใหม่กับเดิม แล้วอัปเดตเฉพาะส่วนที่เปลี่ยน

---

## 5. Flutter ใช้ Declarative UI

* เขียนโค้ดอธิบายว่า UI ควรเป็นอย่างไรตาม state ปัจจุบัน
* ต่างจาก imperative ที่ต้องสั่งเปลี่ยน UI ทีละขั้นตอน
* ทำให้โค้ดอ่านง่าย บริหารจัดการ state ดี ลดบั๊ก

---

## 6. Flow การแสดงผลของ Flutter

1. State เปลี่ยน (เช่นผู้ใช้กดปุ่ม)
2. Flutter เรียก `build()` สร้าง widget tree ใหม่
3. เปรียบเทียบ widget tree ใหม่กับเดิม
4. อัปเดต render tree เฉพาะส่วนที่เปลี่ยน
5. Skia วาดภาพใหม่บนหน้าจอ

---

## 7. Flutter Embedders (ฝัง Flutter ลง native platform)

* Engine ถูกฝังบน Android, iOS, Windows, macOS, Linux ผ่าน embedder ของแต่ละระบบ
* Embedder จัดการ

  * การเชื่อมต่อกับ OS
  * การจัดการหน้าต่าง
  * Event และ native API

---

## สรุปภาพรวมชั้นของ Flutter

| ชั้น (Layer)         | บทบาท                                  |
| -------------------- | -------------------------------------- |
| **Framework** (Dart) | สร้าง UI, logic, animation             |
| **Engine** (C++)     | Render ภาพ, แสดงผล, จัดการ state       |
| **Embedder**         | ผูก Flutter เข้ากับ native OS แต่ละตัว |

---

ถ้าต้องการให้ช่วยทำเป็นไฟล์ PDF หรือภาพ Infographic แจ้งได้เลยครับ!
