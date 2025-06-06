# expanded-flutter-architecture

# สรุปสถาปัตยกรรม Flutter (Flutter Architectural Overview)

---

# 🏗️ สรุปสถาปัตยกรรมของ Flutter (Flutter Architecture)

Flutter ถูกออกแบบมาเพื่อให้สร้าง **UI ที่มีประสิทธิภาพสูงและข้ามแพลตฟอร์มได้อย่างราบรื่น** โดยมีโครงสร้างแบ่งชั้นชัดเจน และใช้เทคโนโลยีที่ช่วยให้เรนเดอร์เร็ว ลื่นไหล

---

## 1. 🧱 โครงสร้างสามชั้นหลัก

Flutter แบ่งออกเป็น 3 ชั้นสำคัญ:

### 📦 Framework (Dart)
- ประกอบด้วย widgets, foundation libraries และ animation APIs
- ใช้ Dart ในการเขียน
- เป็นส่วนที่นักพัฒนาโต้ตอบบ่อยที่สุด

### ⚙️ Engine (C++)
- เขียนด้วย C++
- จัดการการวาดภาพ 2D ด้วย Skia
- จัดการการเรนเดอร์, ข้อความ (LibTxt), แอนิเมชัน, การจัดการ frame และอื่น ๆ

### 🖥️ Embedder (Platform-specific)
- ผูก Flutter กับ OS ของแต่ละแพลตฟอร์ม เช่น Android, iOS, Windows, macOS
- รับผิดชอบ:
  - การจัดการ input (touch, keyboard)
  - การจัดการ lifecycle (pause, resume)
  - สร้าง surface ให้ Flutter engine เรนเดอร์

---

## 2. 🖌️ กระบวนการเรนเดอร์ (Rendering Pipeline)

Flutter ใช้การแยกกระบวนการ UI เป็นหลายชั้น:

Widget → Element → RenderObject → Layer


## 🎨 Flutter ใช้ Skia เพื่อวาด UI เองโดยตรง

Flutter ใช้ **Skia** ซึ่งเป็น **2D rendering engine แบบโอเพ่นซอร์ส** ที่พัฒนาโดย Google  
มันทำหน้าที่เรนเดอร์กราฟิกทั้งหมดที่ปรากฏบนหน้าจอ เช่น:

- ปุ่ม
- ข้อความ
- รูปภาพ
- แอนิเมชัน

### 🚫 แทนที่จะใช้ Native UI Widgets

Flutter **ไม่ใช้ native components** ของแต่ละแพลตฟอร์ม  
(เช่น `UIButton` ของ iOS หรือ `TextView` ของ Android)

แต่เลือกที่จะวาดทุกอย่างใหม่เองลงบนหน้าจอผ่าน **Skia canvas**  
โดยที่ Flutter มีพื้นที่ของแอป (App Surface) เพื่อ render เองทั้งหมด

---

### ✅ ข้อดีของการไม่ใช้ Native UI:

#### 1. ความสม่ำเสมอของ UI (Consistency)
- UI ดูเหมือนกันทุกแพลตฟอร์ม (iOS, Android, Web, Desktop)
- ไม่ต้องเขียนโค้ดพิเศษแยกตาม platform เพื่อให้หน้าตาตรงกัน

#### 2. ประสิทธิภาพสูง (High Performance)
- Skia เรนเดอร์รวดเร็ว รองรับ GPU acceleration
- ทำให้แอปมีเฟรมเรตสูง (60–120 fps) และแอนิเมชันลื่นไหล

#### 3. ปรับแต่ง UI ได้อิสระ (Customization)
- สร้างรูปร่าง สี เอฟเฟกต์ หรือตัวควบคุม UI แบบกำหนดเองได้ง่าย
- ไม่มีข้อจำกัดจาก native components

#### 4. UI อัปเดตพร้อมกันทุกระบบ (Unified Updates)
- เมื่อ Flutter framework เพิ่มฟีเจอร์หรือ UI ใหม่
- ทุกแพลตฟอร์มจะได้ UI เดียวกันโดยไม่ต้องรอ native อัปเดต

---

### 📌 สรุปสั้น

Flutter → ไม่ใช้ native UI → ใช้ Skia วาดเอง → ได้ UI ที่:

- เร็ว
- ลื่น
- เหมือนกันทุกแพลตฟอร์ม
- ปรับแต่งได้มาก


---

## 2. Flutter ใช้ Dart ที่ compile แบบ Ahead-of-Time (AOT)

* ใน build release Dart ถูกแปลงเป็น machine code ล่วงหน้า (AOT)
* ผลลัพธ์คือ

  * ประสิทธิภาพสูง
  * เปิดแอปเร็ว
* ตอนพัฒนาใช้ **JIT (Just-in-Time)** เพื่อรองรับ Hot Reload

* ## ⚙️ AOT (Ahead-of-Time) ใน Flutter คืออะไร?

**AOT (Ahead-of-Time) compilation** คือกระบวนการที่ **แปลงโค้ด Dart เป็น machine code** ล่วงหน้าก่อนที่จะนำไปใช้งานจริง  
โค้ด Dart จะถูก compile เป็น binary ที่ CPU สามารถอ่านและรันได้โดยตรง

### 📦 วิธีทำงานของ AOT ใน Flutter

- ก่อนผู้ใช้ติดตั้งแอป (เช่น จาก App Store หรือ Play Store)
- Flutter จะ compile โค้ด Dart:
  - Android → ไฟล์ `.so`
  - iOS → ไฟล์ `.framework`
- ไม่ต้องตีความโค้ดระหว่างรัน (ต่างจาก scripting languages)

---

## 🧠 ทำไม AOT ถึงสำคัญ?

### ✅ 1. ประสิทธิภาพสูง (High Performance)
- รันได้เร็วมาก เพราะเป็น native machine code
- ไม่ต้องมี VM คอยแปลงโค้ดระหว่าง runtime

### ✅ 2. เปิดแอปรวดเร็ว (Fast Startup Time)
- ไม่มีขั้นตอน compile ตอนเปิดแอป → โหลดเร็ว

### ✅ 3. ขนาดไฟล์เล็กลง (บางกรณี)
- Compiler สามารถ optimize โค้ดก่อนสร้างไฟล์จริง

---

## 🧪 เปรียบเทียบ AOT กับ JIT

| ประเภท             | AOT (Ahead-of-Time)       | JIT (Just-in-Time)         |
|--------------------|----------------------------|-----------------------------|
| เวลา compile        | ก่อนรัน (build time)       | ระหว่างรัน (runtime)        |
| ความเร็วรัน        | เร็ว                       | ช้ากว่า                     |
| เหมาะกับ            | แอปจริง (Production)       | ตอนพัฒนา (Development)     |
| ข้อดี               | ประสิทธิภาพสูง, โหลดเร็ว | รองรับ Hot reload          |

---

## 🛠️ Flutter ใช้ทั้ง AOT และ JIT

- **ระหว่างพัฒนา**:
  - ใช้ **JIT** → รองรับ **Hot Reload**
- **ตอนปล่อยแอปจริง**:
  - ใช้ **AOT** → Performance สูงสุด

---

## 🔧 ตัวอย่างคำสั่ง build:

flutter build apk --release


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
