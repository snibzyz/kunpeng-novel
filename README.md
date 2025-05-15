ยอดเยี่ยมเลยครับ! นี่คือฉบับปรับปรุง README.md และโค้ด SQL สำหรับ INKREALM.CO (V5.1) ที่คุณต้องการ โดยเน้นความละเอียด จัดระเบียบ และความเป็นภาษาไทยทั้งหมดครับ

# INKREALM.CO: ข้อกำหนดระบบและคุณสมบัติหลัก (V5.1) - ฉบับปรับปรุง

เอกสารนี้เป็นข้อกำหนดระบบและคุณสมบัติหลักสำหรับแพลตฟอร์ม INKREALM.CO (V5.1) โดยเน้นฟังก์ชันที่จำเป็นตามการปรับปรุงล่าสุด โดยยังคงอ้างอิง Tech Stack (SvelteKit & Supabase Stack) ที่กำหนดไว้ และออกแบบมาเพื่อความสะดวกในการปรับใช้, ทดสอบ, และย้ายไปยังโดเมนต่างๆ

## 1. ปรัชญาการออกแบบโดยรวม

*   **การแสดงผลหรูหราและอ่านง่าย**: ใช้โทนสี **ทอง-เทา** เป็นหลัก (โหมดสว่าง: ทอง-เทา-ขาว, โหมดมืด: ทอง-เทา-ดำ) และฟอนต์ **Sarabun** เพื่อความสบายตาและเป็นทางการ
*   **เครื่องมือสำหรับนักแปล**: มีฟังก์ชันสนับสนุนการทำงานของนักแปลโดยเฉพาะ เพื่อเพิ่มประสิทธิภาพและความสะดวกในการทำงาน
*   **ใช้งานง่าย ตอบโจทย์ทุกอุปกรณ์**: UI/UX ที่ใช้งานง่าย (Intuitive), Responsive Design รองรับทุกขนาดหน้าจอ, หน้าอ่านไม่มี Horizontal Scroll เด็ดขาด
*   **ความยืดหยุ่นและการทดสอบ**: โครงสร้างที่เอื้อต่อการเปลี่ยนแปลงโดเมน และการทดสอบระบบที่ง่ายดาย

## 1.1 ภาพรวมระบบหลัก (Core System Overview)

| คุณสมบัติหลัก                               | รายละเอียด                                                                                                                                                                                                |
| :------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **ระบบยืนยันตัวตนและจัดการผู้ใช้**             | ลงทะเบียน, เข้าสู่ระบบ (อีเมล/รหัสผ่าน, Google), ลืมรหัสผ่าน, จัดการโปรไฟล์, บทบาทและสิทธิ์ (Roles & Permissions)                                                                                             |
| **ระบบจัดการเนื้อหานิยาย (CMS)**            | การสร้าง/แก้ไข/ลบนิยายและตอน (ผ่าน Modals เป็นหลัก), อัปโหลดเนื้อหา (ปก, .txt), การจัดการหมวดหมู่นิยาย (สำหรับ Admin), ระบบจัดการเวอร์ชันสำหรับข้อมูลนิยายและเนื้อหาตอน                                         |
| **ระบบการอ่านและบุ๊คมาร์ค**                   | หน้าอ่านที่ปรับแต่งได้, ระบบบุ๊คมาร์คตอน, โหมดอ่านต่อเนื่อง, การสลับโหมดการอ่านสำหรับนักแปล                                                                                                                       |
| **ระบบเครื่องมือสำหรับนักแปล**               | โหมดแปล (เมื่อเปิดใช้งาน), ระบบบรรทัดมาร์ค (Mark Line System), หน้าจัดการบรรทัดมาร์ค                                                                                                                         |
| **ระบบแดชบอร์ด**                             | สำหรับนักแปล และ ผู้ดูแลระบบ (รวมถึงหน้าจัดการผู้ใช้, จัดการหมวดหมู่, จัดการประกาศ, หน้าจัดการเว็บไซต์, และ หน้าจัดการเวอร์ชัน)                                                                                  |
| **ระบบฐานข้อมูลและไฟล์**                     | Supabase (PostgreSQL สำหรับฐานข้อมูล, Supabase Storage สำหรับไฟล์)                                                                                                                                        |
| **ระบบการแจ้งเตือนผู้ใช้ (User Feedback)**   | การใช้ Toast notifications (มุมล่างขวา) สำหรับการยืนยันการกระทำต่างๆ และแจ้งข้อผิดพลาด                                                                                                                       |
| **ระบบประกาศ (Announcement System)**         | สำหรับ Admin ในการสร้างและเผยแพร่ประกาศไปยังผู้ใช้กลุ่มต่างๆ ในหน้าต่างๆ                                                                                                                                     |

## 1.2 โครงสร้าง URL ที่เป็นมิตรและการจัดการโดเมน

เพื่อให้ URL ของเว็บไซต์ INKREALM.CO อ่านง่าย, เป็นมิตรต่อผู้ใช้, ดีต่อ SEO, และง่ายต่อการย้ายโดเมน:

| หน้าเว็บ                                      | โครงสร้าง URL                                       |
| :-------------------------------------------- | :--------------------------------------------------- |
| หน้ารวมนิยาย (หน้าแรก)                         | `/`                                                  |
| หน้าข้อมูลนิยาย                               | `/novels/[novel_slug]`                               |
| หน้าอ่านตอนนิยาย                              | `/novels/[novel_slug]/chapter/[chapter_number]`      |
| หน้าโปรไฟล์ผู้ใช้                              | `/profile`                                           |
| หน้าบุ๊คมาร์ค                                  | `/bookmarks`                                         |
| แดชบอร์ดนักแปล                                | `/translator/dashboard`                              |
| หน้าจัดการบรรทัดมาร์ค (นักแปล)                 | `/translator/manage-marks/[novel_slug]`              |
| แดชบอร์ดผู้ดูแลระบบ                           | `/admin/dashboard`                                   |
| ... หน้าย่อยในแดชบอร์ดผู้ดูแลระบบ              | `/admin/dashboard/users`, `/admin/dashboard/categories` ฯลฯ |

### การสร้าง Slug:

*   **Novel Slug**: สร้างอัตโนมัติจากชื่อเรื่อง (พิมพ์เล็ก, ขีดกลางแทนช่องว่าง, ตัดอักขระพิเศษ) ผู้สร้าง/Admin แก้ไขได้ (พร้อมตรวจสอบความซ้ำซ้อน)
*   **Chapter Identifier**: ใช้ `chapter_number` โดยตรงใน URL

### การจัดการโดเมนและ Base URL:

*   **Environment Variables**:
    *   `PUBLIC_BASE_URL`: กำหนด Base URL ของเว็บ (เช่น `https://inkrealm.co` หรือ `http://localhost:5173`)
    *   `PUBLIC_SUPABASE_URL`: URL ของ Supabase project
    *   `PUBLIC_SUPABASE_ANON_KEY`: Anon key ของ Supabase project
*   **Internal Linking**: สร้างลิงก์ภายในโดยอ้างอิง `PUBLIC_BASE_URL` หรือใช้ Relative Paths

## 2. บทบาทผู้ใช้และสิทธิ์ (User Roles & Permissions)

ควบคุมโดย Supabase Row-Level Security (RLS)

| บทบาท        | สิทธิ์หลัก                                                                                                                                                                                                                         | แถบเครื่องมือพิเศษ (ถ้ามี)                                                                                                                                                                                                |
| :------------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **สมาชิก (Member)** | อ่านนิยาย, บุ๊คมาร์คตอน, แก้ไขโปรไฟล์ส่วนตัว                                                                                                                                                                                   | **แถบเครื่องมืออ่าน:** สารบัญ, ปรับแต่งการแสดงผล, บุ๊คมาร์ค, สลับธีมเว็บ, เปิด/ปิดโหมดอ่านต่อเนื่อง, นำทางตอน                                                                                                         |
| **นักแปล (Translator)** | สิทธิ์ทั้งหมดของสมาชิก, เปิด/ปิด "โหมดนักแปล" สำหรับนิยายที่ตนมีสิทธิ์ (ตั้งค่าในหน้าข้อมูลนิยาย), เข้าถึงโหมดแปล, ใช้งานบรรทัดมาร์ค, สร้าง/จัดการนิยายและตอนของตนเอง (ผ่าน Modal), เข้าถึงแดชบอร์ดนักแปล, จัดการบรรทัดมาร์ค  | **แถบเครื่องมืออ่าน (ปกติ):** เหมือน Member<br>**แถบเครื่องมืออ่าน (โหมดนักแปล):** สารบัญ, ปรับแต่งการแสดงผล, บุ๊คมาร์ค, สลับธีมเว็บ, เปิด/ปิดโหมดอ่านต่อเนื่อง, **คัดลอกมาร์คทั้งหมด (ตอนปัจจุบัน)**, นำทางตอน |
| **ผู้ดูแลระบบ (Admin)** | สิทธิ์ทั้งหมดของนักแปล, เข้าถึงแดชบอร์ดผู้ดูแลระบบ, จัดการผู้ใช้, ดูข้อมูลมาร์คทั้งหมด, จัดการเว็บไซต์, จัดการหมวดหมู่นิยาย, จัดการประกาศ, จัดการเวอร์ชัน                                                                        | เหมือน Translator                                                                                                                                                                                                    |

## 3. การเข้าสู่ระบบ / ลงทะเบียน / ลืมรหัสผ่าน (Authentication)

*   **การแสดงผล**: Modal เป็นหลัก (เรียกจากภายในเว็บ), มีหน้าเฉพาะ `/login`, `/register`, `/forgot-password` สำหรับการเข้าถึงโดยตรง
*   **Tech Stack**: Supabase Auth, Felte + Zod (สำหรับการตรวจสอบฟอร์ม), Skeleton UI
*   **Toast Notifications (ภาษาไทย, มุมล่างขวา)**:
    *   `"เข้าสู่ระบบสำเร็จ ยินดีต้อนรับ!"`
    *   `"ลงทะเบียนเรียบร้อย กรุณาตรวจสอบอีเมลเพื่อยืนยันบัญชี"` (หากเปิดใช้ Email Verification)
    *   `"ลงทะเบียนเรียบร้อย ยินดีต้อนรับ!"` (หากไม่ได้เปิดใช้ Email Verification)
    *   `"คำขอรีเซ็ตรหัสผ่านถูกส่งไปยังอีเมลของคุณแล้ว กรุณาตรวจสอบ"`
    *   `"รหัสผ่านถูกรีเซ็ตเรียบร้อยแล้ว กรุณาเข้าสู่ระบบใหม่"`
    *   `"เกิดข้อผิดพลาด: [ข้อความแสดงข้อผิดพลาดที่เข้าใจง่าย]"`

## 4. โครงสร้างแถบนำทาง (Navbar Structure)

Responsive และแสดงเมนูตามบทบาทผู้ใช้ (Dynamic Menu) โลโก้เว็บไซต์ (อาจเป็น Gradient ทอง-เทา) จะแสดงเสมอ

*   **Guest (ผู้เยี่ยมชม)**: โลโก้, ปุ่มสลับธีม (สว่าง/มืด), ปุ่ม "เข้าสู่ระบบ" (เปิด Login Modal)
*   **Member (สมาชิก)**: โลโก้, "หน้าแรก", "บุ๊คมาร์ค", ปุ่มสลับธีม, เมนูโปรไฟล์ (Dropdown: แสดงชื่อและบทบาท, ลิงก์ "โปรไฟล์ของฉัน", "ออกจากระบบ")
*   **Translator (นักแปล)**: เหมือน Member + "แดชบอร์ดนักแปล"
*   **Admin (ผู้ดูแลระบบ)**: เหมือน Translator + "แดชบอร์ดผู้ดูแลระบบ"

## 5. รายละเอียดหน้าเว็บไซต์, Modals, และการจัดการเนื้อหา

### 5.1 หน้าแรก (Home Page - `/`)

*   **ส่วนแสดงประกาศ**: แสดงประกาศที่ Active และตรงเป้าหมายผู้ใช้/หน้านี้ (แบนเนอร์ด้านบน, มีปุ่มปิด)
*   **ส่วนเครื่องมือหลัก**:
    *   ช่องค้นหานิยาย (Input placeholder: `ค้นหานิยาย...`)
    *   ฟิลเตอร์หมวดหมู่ (Dropdown label: `หมวดหมู่ทั้งหมด`)
    *   ฟิลเตอร์สถานะนิยาย (Dropdown label: `สถานะทั้งหมด`, ตัวเลือก: `ต่อเนื่อง`, `จบแล้ว`)
    *   ปุ่ม `[+ สร้างนิยาย]` (สำหรับนักแปล/Admin, เปิด Modal สร้างนิยาย)
*   **การแสดงรายการนิยาย**: Grid Layout (รูปปก 3:4, ชื่อเรื่อง, จำนวนตอน). Mobile 2 คอลัมน์.
*   **ระบบแบ่งหน้า (Pagination)**
*   **สำหรับ Guest (ครั้งแรก)**: แสดง Login Modal อัตโนมัติ

#### 5.1.1 Modal สร้างนิยาย (Create Novel Modal)

*   **หัวเรื่อง**: `สร้างนิยายใหม่`
*   **ฟอร์ม**:
    *   ชื่อเรื่อง (จำเป็น)
    *   คำโปรย
    *   หมวดหมู่ (Dropdown)
    *   รูปปกนิยาย (อัปโหลด, Crop 3:4, ย่อขนาด < 3MB)
    *   ภาษาต้นฉบับ (Dropdown: `จีน (zh)`, `อังกฤษ (en)`, `ไทย (th)`)
    *   สิทธิ์ในการมองเห็นนิยาย (Checkbox กลุ่ม, เลือกได้หลายอัน: `ทุกคน`, `เฉพาะสมาชิก`, `เฉพาะนักแปล`)
    *   **การตั้งค่าเริ่มต้นสำหรับตอนใหม่ของนิยายนี้**:
        *   สถานะเริ่มต้นของตอน (Radio กลุ่ม: `เผยแพร่` (ค่าเริ่มต้น), `ฉบับร่าง`)
        *   สิทธิ์การเข้าถึงตอนเริ่มต้น (Checkbox กลุ่ม: `ทุกคน` (ค่าเริ่มต้น), `เฉพาะสมาชิก`, `เฉพาะนักแปล`)
*   **ปุ่มดำเนินการ**:
    *   `เผยแพร่นิยาย`: Toast `สร้างนิยาย '[ชื่อนิยาย]' และเผยแพร่สำเร็จแล้ว`
    *   `บันทึกเป็นฉบับร่าง`: Toast `สร้างนิยาย '[ชื่อนิยาย]' เป็นฉบับร่างสำเร็จแล้ว`
    *   `ยกเลิก`

### 5.2 หน้าข้อมูลนิยายและสารบัญ (`/novels/[novel_slug]`)

*   **ส่วนแสดงประกาศ**: แสดงประกาศที่ Active และตรงเป้าหมาย
*   **ส่วนข้อมูลนิยาย**: รูปปก, ชื่อเรื่อง, ผู้แต่ง, หมวดหมู่, คำโปรย, จำนวนตอน, สถานะ, ภาษาต้นฉบับ (`lang` attribute)
    *   **(นักแปล/Admin เจ้าของเรื่อง)** ปุ่มสลับ `โหมดนักแปล` (Toggle Switch: เปิด/ปิด) - บันทึกสถานะเฉพาะบุคคล/เรื่อง
*   **ปุ่มจัดการนิยาย (ผู้สร้าง/Admin)**:
    *   `แก้ไขข้อมูลนิยาย`: เปิด Modal แก้ไข (ข้อ 5.2.1)
    *   `ลบนิยาย`: Modal ยืนยัน (`คุณแน่ใจหรือไม่ว่าต้องการลบนิยาย '[ชื่อนิยาย]'? การกระทำนี้ไม่สามารถย้อนกลับได้`), Toast `ลบนิยาย '[ชื่อนิยาย]' เรียบร้อยแล้ว`
*   **ส่วนเพิ่มตอน (ผู้สร้าง/Admin)**:
    *   `เพิ่มตอนด้วย .txt`: เปิด Modal (ข้อ 5.2.2)
    *   `เพิ่มตอน`: เปิด Modal (ข้อ 5.2.3)
*   **ส่วนสารบัญ**: Collapse/Accordion (ปรับจำนวนตอนต่อกลุ่ม 10, 20, 25, 30, 50 (ค่าเริ่มต้น), 100), กลุ่มแรกขยาย
    *   **รายการตอน (ทุกคน)**: ไอคอนบุ๊คมาร์ค, ชื่อตอน, สถานะการอ่าน (PC: พื้นหลังเปลี่ยนสี + ไอคอนตา `อ่านแล้ว`; Mobile: พื้นหลังเปลี่ยนสี + ไอคอนตา)
    *   **รายการตอน (นักแปล/Admin)**: ไอคอนสถานะตอน (เผยแพร่/ร่าง), ปุ่ม `แก้ไขตอน`, ปุ่ม `ลบตอน`, ปุ่ม `คัดลอกมาร์คทั้งหมด` (สำหรับตอนนั้น)

#### 5.2.1 Modal แก้ไขข้อมูลนิยาย (Edit Novel Modal)

*   **หัวเรื่อง**: `แก้ไขข้อมูลนิยาย: [ชื่อนิยายปัจจุบัน]`
*   **ฟิลด์ที่แก้ไขได้**: ชื่อเรื่อง, คำโปรย, หมวดหมู่, รูปปก, ภาษาต้นฉบับ, สถานะนิยาย (Radio: `ต่อเนื่อง`, `จบแล้ว`, `ยุติการลง`), สิทธิ์การมองเห็นนิยาย
*   **ค่าเริ่มต้นสำหรับการสร้างตอนใหม่ของนิยายนี้**: สถานะตอน, สิทธิ์เข้าถึงตอน (เหมือน Modal สร้างนิยาย)
*   **ปุ่มดำเนินการ**:
    *   `บันทึกการเปลี่ยนแปลง`: Toast `บันทึกข้อมูลนิยาย '[ชื่อนิยาย]' สำเร็จแล้ว` (สร้าง novel_versions ถ้ามีการเปลี่ยนแปลง)
    *   `ยกเลิก`
    *   `ลบนิยาย` (ภายใน Modal): Modal ยืนยัน, Toast `ลบนิยาย '[ชื่อนิยาย]' เรียบร้อยแล้ว`

#### 5.2.2 Modal เพิ่มตอนด้วย .txt (Batch Upload Modal)

*   **หัวเรื่อง**: `เพิ่มหลายตอนด้วยไฟล์ .txt สำหรับนิยาย: [ชื่อนิยาย]`
*   **ความสามารถ**: Drag & Drop, Max 500 ไฟล์, Sort ชื่อไฟล์ (ใช้เป็นชื่อตอน), เชื่อมโยง novel_id, จัดการไฟล์ซ้ำ (เขียนทับ/ข้าม), Progress Bar
*   **Toast สำเร็จ**: `อัปโหลดตอนเสร็จสิ้น: เพิ่ม X ตอน, เขียนทับ Y ตอน, ข้าม Z ตอน`
*   **การทำงานเบื้องหลัง**: สร้าง `mark_lines` (ว่าง) และ `chapter_versions` เริ่มต้นอัตโนมัติ

#### 5.2.3 Modal เพิ่ม/แก้ไขตอน (Create/Edit Chapter Modal)

*   **หัวเรื่อง**: `เพิ่มตอนใหม่` หรือ `แก้ไขตอน: [ชื่อตอนปัจจุบัน]`
*   **ฟอร์ม**:
    *   ชื่อตอน (จำเป็น)
    *   เนื้อหา (Rich Text Editor - TipTap, ฟอนต์ Sarabun, รองรับ Bold, Italic, Underline, List, Link, คัดลอก/วางจาก Word รักษา Format พื้นฐาน)
    *   สถานะตอน (Radio กลุ่ม: `เผยแพร่`, `ฉบับร่าง`) - ค่าเริ่มต้นจาก Novel Settings
    *   สิทธิ์การเข้าถึงตอน (Checkbox กลุ่ม: `ทุกคน`, `เฉพาะสมาชิก`, `เฉพาะนักแปล`) - ค่าเริ่มต้นจาก Novel Settings
*   **ปุ่มดำเนินการ**:
    *   **เพิ่มตอนใหม่**: `เผยแพร่ตอน`, `บันทึกเป็นฉบับร่าง`
    *   **แก้ไขตอน**: `บันทึกการเปลี่ยนแปลง`
    *   `(ทั้งสองกรณี)`: `ยกเลิก`
*   **Toast สำเร็จ**: `บันทึกตอน '[ชื่อตอน]' เรียบร้อยแล้ว` (สร้าง `chapter_versions` ถ้ามีการเปลี่ยนแปลงเนื้อหา)
*   **การทำงานเบื้องหลัง (สร้างใหม่)**: สร้าง `mark_lines` (ว่าง) และ `chapter_versions` เริ่มต้นอัตโนมัติ

### 5.3 หน้าบุ๊คมาร์ค (`/bookmarks`)

*   **แสดงรายการนิยายที่บุ๊คมาร์ค** (เรียงตามวันที่บุ๊คมาร์คล่าสุดของนิยายนั้นๆ)
    *   การ์ดนิยาย: รูปปก, ชื่อนิยาย, ตอนล่าสุดที่บุ๊คมาร์ค, ปุ่ม `ไปยังบุ๊คมาร์คล่าสุด`
    *   ปุ่ม `จัดการบุ๊คมาร์คเรื่องนี้`: เปิด Modal `จัดการบุ๊คมาร์คสำหรับ: [ชื่อนิยาย]`
        *   **ภายใน Modal**: เรียงลำดับ (ตอน/วันที่), รายการบุ๊คมาร์ค (Checkbox, ชื่อตอน, วันที่/เวลา, ไฮไลท์ล่าสุด), ปุ่ม `ลบที่เลือก` (Toast `ลบบุ๊คมาร์ค X รายการสำเร็จ`), ปุ่ม `ล้างบุ๊คมาร์คเรื่องนี้` (Modal ยืนยัน `คุณแน่ใจหรือไม่ว่าต้องการล้างบุ๊คมาร์คทั้งหมดของเรื่อง '[ชื่อนิยาย]'?`, Toast `ล้างบุ๊คมาร์คทั้งหมดของเรื่อง '[ชื่อนิยาย]' สำเร็จแล้ว`), ปุ่ม `เสร็จสิ้น`, `ยกเลิก`
*   ปุ่ม `ลบบุ๊คมาร์คทั้งหมด (ทุกเรื่อง)`: Modal ยืนยัน `คุณแน่ใจหรือไม่ว่าต้องการลบบุ๊คมาร์คทั้งหมด (ทุกเรื่อง)?`, Toast `ลบบุ๊คมาร์คทั้งหมด (ทุกเรื่อง) เรียบร้อยแล้ว`

### 5.4 แดชบอร์ดนักแปล (`/translator/dashboard`)

*   แสดงรายการนิยายที่นักแปลจัดการ (การ์ด: ปก, ชื่อ, ความคืบหน้าการมาร์ค % + Progress Bar)
*   แต่ละนิยายมีปุ่ม `จัดการบรรทัดมาร์ค` (นำทางไปหน้า 5.4.1)

#### 5.4.1 หน้าจัดการบรรทัดมาร์ค (`/translator/manage-marks/[novel_slug]`)

*   **หัวเรื่อง**: `จัดการบรรทัดมาร์คสำหรับ: [ชื่อนิยาย]`
*   **ส่วนควบคุมหลัก**: ปุ่ม `เลือกตอนเพื่อคัดลอก/ดาวน์โหลด` (Toggle โหมดเลือก, แสดง/ซ่อน Checkbox และ Batch Actions Toolbar)
*   **สารบัญตอน**: Collapse/Accordion, ปรับจำนวนตอนต่อกลุ่ม, หัวกลุ่ม (ชื่อกลุ่ม, (โหมดเลือก) Checkbox "เลือกทั้งหมดในกลุ่ม")
*   **รายการตอน**: เลขลำดับ, ชื่อตอน, จำนวนบรรทัดมาร์ค, สถานะมาร์ค (ไอคอน `CheckCircle2`/`Circle`), ปุ่ม `คัดลอกมาร์คตอนนี้` (ไอคอน `Copy`, Toast `คัดลอกมาร์คของตอน '[ชื่อตอน]' ไปยังคลิปบอร์ดแล้ว`), (โหมดเลือก) Checkbox เลือกตอน
*   **Batch Actions Toolbar (เมื่อเลือกตอน)**:
    *   ปุ่ม `คัดลอกบรรทัดมาร์คที่เลือก`: Toast `คัดลอกมาร์คของตอนที่เลือกไปยังคลิปบอร์ดแล้ว`
    *   ปุ่ม `ดาวน์โหลดไฟล์บรรทัดมาร์ค`: เปิด Modal ตัวเลือก (`รวมเป็นไฟล์ .txt เดียว`, `แยกไฟล์เป็น .zip`), Toast `กำลังเตรียมไฟล์ดาวน์โหลด...` และ `ดาวน์โหลดไฟล์ [ชื่อไฟล์] สำเร็จ`

### 5.5 หน้าอ่านนิยายและโหมดแปล (`/novels/[novel_slug]/chapter/[chapter_number]`)

*   **การแสดงผลเนื้อหา**:
    *   ชื่อตอน: แสดงชัดเจนตรงกลางบน
    *   เนื้อหานิยาย: ย่อหน้าทุกย่อหน้า (CSS `text-indent`), PC: กว้างจำกัดกลางจอ, Mobile: เต็มขอบจอ, ไม่มี Horizontal Scroll
    *   ปุ่ม `Arrow Up` (กลับบนสุด): มุมขวาล่าง กึ่งโปร่งแสง เมื่อเลื่อนลง
*   **ควบคุมด้วยคีย์บอร์ด (PC)**: ลูกศรซ้าย (ก่อนหน้า), ลูกศรขวา (ถัดไป), Space Bar (เลื่อนลง)

*   **โหมดการอ่าน (Member/Translator ที่ไม่ได้เปิด "โหมดนักแปล" สำหรับนิยายนั้น)**:
    *   **แถบเครื่องมือบน (Top Toolbar - แสดงเสมอ)**:
        *   **PC**: `สารบัญ` (Modal), `ปรับแต่งการแสดงผล (Aa)` (Popover), `บุ๊คมาร์ค` (Toast `บุ๊คมาร์คตอน '[ชื่อตอน]' แล้ว`/`ยกเลิกบุ๊คมาร์คตอน '[ชื่อตอน]' แล้ว`), `สลับธีม`, `โหมดอ่านต่อเนื่อง` (ไอคอน Infinity มี Animation สีรุ้งเมื่อเปิด, Toast `เปิด/ปิดโหมดอ่านต่อเนื่องแล้ว`), `< ตอนก่อนหน้า`, `ตอนต่อไป >`
        *   **Mobile**: `สารบัญ`, `ปรับแต่ง (Aa)`, `บุ๊คมาร์ค`, `สลับธีม`, `โหมดอ่านต่อเนื่อง`
    *   **การนำทางตอน (Mobile - Bottom Toolbar)**: ปรากฏเมื่อแตะเนื้อหา, ซ่อนเมื่อเลื่อน/แตะอีกครั้ง. ปุ่ม: `< ตอนก่อนหน้า`, `ตอนต่อไป >`
    *   **ท้ายบท**:
        *   **อ่านต่อเนื่องปิด**: ปุ่มใหญ่ `ตอนต่อไป: [ชื่อตอนถัดไป]`
        *   **อ่านต่อเนื่องเปิด**: โหลดตอนถัดไปอัตโนมัติ (Infinite Scroll), ชื่อตอนด้านบนอัปเดต

*   **โหมดนักแปล (Translator Mode - เมื่อเปิด "โหมดนักแปล" สำหรับนิยายนี้)**:
    *   **แถบเครื่องมือบน (Top Toolbar - แสดงเสมอ)**:
        *   **PC/Mobile**: เหมือนโหมดอ่านปกติ + ปุ่ม `คัดลอกมาร์คทั้งหมด` (ไอคอน `CopyCheck`, สำหรับตอนปัจจุบัน, Toast `คัดลอกมาร์คทั้งหมดของตอนนี้ไปยังคลิปบอร์ดแล้ว`)
    *   **การแสดงผลเนื้อหา**:
        *   **แถบเลขบรรทัด (Gutter)**: ด้านซ้าย, สีปรับตามธีม (ตัวเลขเด่นน้อยกว่าเนื้อหา)
        *   **เนื้อหาต้นฉบับ (Original Content)**: ถัดจากเลขบรรทัด, ย่อหน้า
        *   **Checkbox มาร์ค**: ขวาสุดของแต่ละบรรทัด
    *   **MarkLine Interaction**:
        *   **คลิก Checkbox**: มาร์ค/ยกเลิก (Toast `มาร์คบรรทัดที่ X แล้ว`/`ยกเลิกมาร์คบรรทัดที่ X แล้ว`)
        *   **ดับเบิลคลิกบรรทัดเนื้อหา**: ไฮไลท์ชั่วครู่, คัดลอกข้อความต้นฉบับของบรรทัดนั้น, มาร์คบรรทัดนั้นอัตโนมัติ (Toast `คัดลอกและมาร์คบรรทัดที่ X แล้ว`)
    *   **ท้ายบท (โหมดนักแปล)**:
        *   สรุปข้อมูล: `มีมาร์คทั้งหมด: [X] บรรทัด`, `บรรทัดที่มาร์ค: [เลขที่ 1], [เลขที่ 2], ...` (5 บรรทัดแรก + ปุ่ม `ดูทั้งหมด` -> Modal)
        *   ปุ่ม `ล้างมาร์คทั้งหมดของตอนนี้`: Modal ยืนยัน (`คุณแน่ใจหรือไม่ว่าต้องการล้างมาร์คทั้งหมดของตอนนี้?`), Toast `ล้างมาร์คทั้งหมดของตอนนี้แล้ว`
        *   ปุ่ม `คัดลอกมาร์คทั้งหมดของตอนนี้`: Toast `คัดลอกมาร์คทั้งหมดของตอนนี้ไปยังคลิปบอร์ดแล้ว`

### 5.6 แดชบอร์ดผู้ดูแลระบบ (`/admin/dashboard`)

*   **Sidebar ซ้าย**: `ภาพรวม`, `จัดการผู้ใช้`, `จัดการหมวดหมู่`, `จัดการประกาศ`, `จัดการเว็บไซต์`, `จัดการเวอร์ชัน`
*   **พื้นที่เนื้อหาหลัก**: แสดงตามเมนูที่เลือก

#### 5.6.1 หน้าภาพรวมแดชบอร์ด (`/admin/dashboard`)

*   **หัวเรื่อง**: `ภาพรวมแดชบอร์ดผู้ดูแลระบบ`
*   แสดงข้อมูล: จำนวนนิยายทั้งหมด, จำนวนผู้ใช้ทั้งหมด

#### 5.6.2 หน้าจัดการผู้ใช้ (`/admin/dashboard/users`)

*   **หัวเรื่อง**: `จัดการผู้ใช้ทั้งหมด`
*   **ควบคุม**: ค้นหาผู้ใช้ (Real-time), สรุปจำนวนผู้ใช้
*   **ตารางผู้ใช้**: #, Avatar, Email, ชื่อที่แสดง, สิทธิ์, วันที่สมัคร, วันที่สิทธิ์หมดอายุ, ปุ่ม `จัดการ` (เปิด Modal)
*   **Modal จัดการผู้ใช้**:
    *   UI: Avatar (เปลี่ยนได้), Email (Read-only), ชื่อที่แสดง (แก้ไขได้), สิทธิ์ (Dropdown แก้ไขได้), วันที่สมัคร (Read-only), วันที่สิทธิ์หมดอายุ (Date Picker, ปุ่มช่วย: `+1ด`, `+3ด`, `+6ด`, `ไม่มีวันหมดอายุ`), ระงับบัญชี (Toggle)
    *   ปุ่ม: `บันทึกการเปลี่ยนแปลง` (Toast `อัปเดตข้อมูลผู้ใช้ [Email] สำเร็จแล้ว`), `ยกเลิก`

#### 5.6.3 หน้าจัดการหมวดหมู่นิยาย (`/admin/dashboard/categories`)

*   **หัวเรื่อง**: `จัดการหมวดหมู่นิยาย`
*   **ส่วนเพิ่ม**: ปุ่ม `+ เพิ่มหมวดหมู่ใหม่` (เปิด Modal: ชื่อ, Slug (auto, แก้ไขได้), คำอธิบาย; ปุ่ม `บันทึก` (Toast `เพิ่มหมวดหมู่ '[ชื่อหมวดหมู่]' สำเร็จแล้ว`), `ยกเลิก`)
*   **รายการหมวดหมู่ (Accordion)**:
    *   แถวหลัก: ชื่อ, จำนวนนิยาย, ปุ่ม `แก้ไข` (Modal: ชื่อ, Slug, คำอธิบาย; Toast `แก้ไขหมวดหมู่ '[ชื่อหมวดหมู่]' สำเร็จแล้ว`), ปุ่ม `ลบ` (Modal ยืนยัน `คุณแน่ใจหรือไม่ว่าต้องการลบหมวดหมู่ '[ชื่อหมวดหมู่]'? นิยายในหมวดหมู่นี้จะถูกย้ายไปที่ [หมวดหมู่ที่เลือก/ไม่มีหมวดหมู่]`, Toast `ลบหมวดหมู่ '[ชื่อหมวดหมู่]' สำเร็จแล้ว`), ปุ่ม `แสดง/ซ่อนนิยาย`
    *   ส่วนขยาย: รายการนิยายในหมวด (ชื่อ + ลิงก์)

#### 5.6.4 หน้าจัดการประกาศ (`/admin/dashboard/announcements`)

*   **หัวเรื่อง**: `จัดการประกาศ`
*   ปุ่ม `+ สร้างประกาศใหม่` (เปิด Modal)
*   **ตารางประกาศ**: หัวข้อ, สถานะ, หน้าที่แสดง, กลุ่มผู้รับ, วันที่เริ่ม/สิ้นสุด, ปุ่ม: `แก้ไข` (Modal), `ลบ` (Modal ยืนยัน `คุณแน่ใจหรือไม่ว่าต้องการลบประกาศ '[หัวข้อประกาศ]'?`, Toast `ลบประกาศ '[หัวข้อประกาศ]' สำเร็จแล้ว`), `สลับสถานะใช้งาน` (Toggle, Toast `เปลี่ยนสถานะประกาศ '[หัวข้อประกาศ]' เป็น [ใช้งาน/ไม่ใช้งาน] แล้ว`)
*   **Modal สร้าง/แก้ไขประกาศ**: หัวข้อ, เนื้อหา (Markdown Editor), วันที่เริ่ม/สิ้นสุด, หน้าที่แสดง (Checkbox: `หน้าแรก`, `หน้านิยาย (ทุกเรื่อง)`, `หน้านิยาย (เฉพาะเรื่อง)` - ถ้าเลือกอันหลังจะมี Input ให้ใส่ Novel ID/Slug), กลุ่มผู้รับ (Checkbox: `ทุกคน`, `สมาชิก`, `นักแปล`, `ผู้ดูแลระบบ`), สถานะ `ใช้งาน` (Toggle), ปุ่ม `บันทึก` (Toast `บันทึกประกาศ '[หัวข้อประกาศ]' สำเร็จแล้ว`), `ยกเลิก`

#### 5.6.5 หน้าจัดการเว็บไซต์ (`/admin/dashboard/site-settings`)

*   **หัวเรื่อง**: `จัดการเว็บไซต์`
*   **ส่วนทั่วไป**: ชื่อเว็บไซต์, คำอธิบาย (SEO), โลโก้ (อัปโหลด, Preview, Toast `อัปเดตโลโก้เว็บไซต์สำเร็จ`), Favicon (อัปโหลด, Toast `อัปเดต Favicon สำเร็จ`)
*   **ส่วนการลงทะเบียน/เข้าสู่ระบบ**: เปิด/ปิดการลงทะเบียนใหม่ (Toggle), เปิด/ปิดเข้าสู่ระบบด้วย Google (Toggle)
*   **ส่วนเนื้อหา**: จำนวนรายการต่อหน้า (Pagination), การตั้งค่าเริ่มต้นสำหรับนิยายใหม่ (สถานะตอน, สิทธิ์เข้าถึงตอน)
*   ปุ่ม `บันทึกการตั้งค่า`: Toast `บันทึกการตั้งค่าเว็บไซต์สำเร็จแล้ว`

#### 5.6.6 หน้าจัดการเวอร์ชัน (`/admin/dashboard/versions`)

*   **หัวเรื่อง**: `จัดการเวอร์ชัน`
*   **Tab 1: เวอร์ชันเว็บไซต์ / Changelog**
    *   แสดงรายการ Changelog (เวอร์ชัน, วันที่, สรุป)
    *   ปุ่ม `+ เพิ่ม Changelog ใหม่`: Modal (เวอร์ชัน, วันที่, สรุป (Markdown), สถานะ (เผยแพร่/ร่าง))
    *   แต่ละรายการ: ปุ่ม `แก้ไข`, `ลบ` (Modal ยืนยัน, Toast `ลบ Changelog เวอร์ชัน [เวอร์ชัน] สำเร็จแล้ว`)
*   **Tab 2: เวอร์ชันนิยายและตอน**
    *   ค้นหานิยาย
    *   รายการนิยาย: ชื่อ, ปุ่ม `ดูประวัติเวอร์ชันนิยาย`, `ดูประวัติเวอร์ชันตอน`
    *   **ประวัติเวอร์ชันนิยาย**: รายการ (เวอร์ชัน, วันที่, ผู้แก้ไข, หมายเหตุ), ปุ่ม `ดูรายละเอียด` (Snapshot), `กู้คืนเวอร์ชันนี้` (Modal ยืนยัน `คุณแน่ใจหรือไม่ว่าต้องการกู้คืนข้อมูลนิยายนี้เป็นเวอร์ชัน [เลขเวอร์ชัน]? การเปลี่ยนแปลงปัจจุบันจะถูกบันทึกเป็นเวอร์ชันใหม่ก่อนกู้คืน`, Toast `กู้คืนข้อมูลนิยายเป็นเวอร์ชัน [เลขเวอร์ชัน] สำเร็จแล้ว`)
    *   **ประวัติเวอร์ชันตอน**: เลือกตอน (Dropdown), รายการ (เวอร์ชัน, วันที่, ผู้แก้ไข, หมายเหตุ), ปุ่ม `ดูเนื้อหา` (Snapshot), `กู้คืนเวอร์ชันนี้` (Modal ยืนยัน `คุณแน่ใจหรือไม่ว่าต้องการกู้คืนเนื้อหาตอนนี้เป็นเวอร์ชัน [เลขเวอร์ชัน]? เนื้อหาปัจจุบันจะถูกบันทึกเป็นเวอร์ชันใหม่ก่อนกู้คืน`, Toast `กู้คืนเนื้อหาตอนเป็นเวอร์ชัน [เลขเวอร์ชัน] สำเร็จแล้ว`)

### 5.7 Modal สร้างนิยาย (ซ้ำกับ 5.1.1)

(รายละเอียดตามข้อ 5.1.1)

### 5.8 การจัดการตอน

#### 5.8.1 Modal เพิ่มตอนด้วย .txt (ซ้ำกับ 5.2.2)

(รายละเอียดตามข้อ 5.2.2)

#### 5.8.2 Modal เพิ่ม/แก้ไขตอน (ซ้ำกับ 5.2.3)

(รายละเอียดตามข้อ 5.2.3)

### 5.9 หน้าโปรไฟล์ (`/profile`)

*   **หัวเรื่อง**: `โปรไฟล์ของฉัน`
*   **รูปโปรไฟล์ (Avatar)**: แสดง, ปุ่ม `เปลี่ยนรูปโปรไฟล์` (File Picker, Crop 1:1, Toast `เปลี่ยนรูปโปรไฟล์สำเร็จแล้ว`)
*   **ชื่อที่แสดง**: แสดง, Input แก้ไข, ปุ่ม `บันทึกชื่อที่แสดง` (Toast `อัปเดตชื่อที่แสดงสำเร็จแล้ว`)
*   **สิทธิ์ผู้ใช้งาน (Role)**: แสดงบทบาท (Read-only, Badge สี: สมาชิก-เขียว, นักแปล-ทอง, ผู้ดูแลระบบ-แดง)
*   **อีเมล**: แสดง (Read-only)
*   **เปลี่ยนรหัสผ่าน**: ปุ่ม `เปลี่ยนรหัสผ่าน` (เปิด Modal)
    *   **Modal เปลี่ยนรหัสผ่าน**: ฟิลด์ (รหัสผ่านปัจจุบัน, ใหม่, ยืนยันใหม่), ปุ่ม `บันทึกรหัสผ่านใหม่` (Toast `เปลี่ยนรหัสผ่านสำเร็จแล้ว กรุณาเข้าสู่ระบบใหม่อีกครั้ง` และบังคับ Logout), `ยกเลิก`
*   **ออกจากระบบ**: ปุ่ม `ออกจากระบบ` (Modal ยืนยัน `คุณต้องการออกจากระบบหรือไม่?`, Toast `ออกจากระบบแล้ว`, Redirect ไปหน้าแรก)

## 6. Key User Experience Flows & Enhancements

*   การล็อกอิน (Modal)
*   การนำทางที่ชัดเจน
*   ประสบการณ์การอ่านที่ดี (ปรับแต่งได้, โหมดต่อเนื่อง, ไม่มี scroll แนวนอน)
*   เครื่องมือช่วยแปลที่ใช้งานง่าย (บรรทัดมาร์ค, คัดลอกง่าย)
*   การตอบสนองของระบบ (Empty/Loading States, Error Handling พร้อม Toast ภาษาไทย)
*   ดีไซน์สวยงาม (ทอง-เทา, Sarabun) Responsive ทุกอุปกรณ์
*   ปุ่ม `Arrow Up` กลับไปบนสุด
*   **Toast Notifications**: ยืนยันทุกการกระทำสำคัญ, แสดงมุมล่างขวา
*   **สร้าง mark_lines อัตโนมัติ**: เมื่อสร้างตอนใหม่ (manual/batch) สำหรับนักแปลเจ้าของนิยาย

## 7. Database Structure (Supabase - PostgreSQL)

ดูโค้ด SQL ด้านล่างสำหรับโครงสร้างตารางทั้งหมด

*   **ตารางหลัก**: `roles`, `profiles`, `categories`, `novels`, `chapters`, `user_bookmarks`, `mark_lines`, `announcements`, `site_settings`, `site_changelogs`, `novel_versions`, `chapter_versions`
*   **การรักษาความปลอดภัย**: Row-Level Security (RLS) บังคับใช้อย่างเข้มงวด

## 8. Supabase Storage Buckets

*   `novel-covers` (สิทธิ์ public read)
*   `chapter-text-files` (สำหรับ Batch Upload, temporary, สิทธิ์ private, ลบหลังประมวลผล)
*   `avatars` (สิทธิ์ public read)
*   `site_assets` (สำหรับโลโก้, favicon, สิทธิ์ public read)

## 9. สรุป Tech Stack (Tech Stack Summary)

| ประเภท         | เทคโนโลยี                                                                                 |
| :-------------- | :----------------------------------------------------------------------------------------- |
| Framework       | SvelteKit                                                                                  |
| Styling         | TailwindCSS                                                                                |
| UI Library      | Skeleton UI (ปรับแต่ง Theme ทอง-เทา)                                                       |
| Form Handling   | Felte และ Zod                                                                              |
| Icon Library    | Lucide-Svelte                                                                              |
| Backend & Auth  | Supabase Platform (PostgreSQL, Auth, Storage, Edge Functions (ถ้าจำเป็น))                   |
| ภาษา            | TypeScript                                                                                 |
| Rich Text Editor| TipTap (สำหรับเนื้อหาตอน)                                                                  |
| Image Cropper   | JavaScript library (เช่น Cropper.js หรือ tương tự)                                         |

## 10. การจัดการข้อผิดพลาดเบื้องต้น (Basic Error Handling)

*   **Client-Side Validation**: Felte + Zod แสดงข้อผิดพลาดใต้ฟิลด์ (ภาษาไทย)
*   **Server-Side Validation**: SvelteKit actions/loaders, Supabase Edge Functions (ใช้ Zod ด้วย)
*   **User-Facing Error Messages**: Toast notifications ที่เป็นมิตร (ภาษาไทย) เช่น `เกิดข้อผิดพลาด: ไม่สามารถบันทึกข้อมูลได้ กรุณาลองใหม่อีกครั้ง`
*   **Code-Level Error Handling**: `try...catch` ใน SvelteKit และ Supabase Functions
*   **Error Logging**: `console.error` ระหว่างพัฒนา, Supabase Logs

## 11. มาตรการความปลอดภัยเบื้องต้น (Basic Security Measures)

*   **Authentication**: Supabase Auth (พิจารณาเปิด Email Verification)
*   **Authorization (RLS)**: หัวใจสำคัญ! กำหนด RLS เข้มงวดสำหรับ `novels`, `chapters`, และตารางข้อมูลอ่อนไหวอื่นๆ
*   **Input Validation**: Zod (Client & Server), Sanitize HTML output (เช่น DOMPurify ถ้า TipTap ไม่จัดการให้เพียงพอ)
*   **HTTPS**: บังคับใช้
*   **Environment Variables**: `.env` (ไม่ commit `.env.local`), Service Role Key ใช้เฉพาะฝั่ง Server
*   **Data Backups**: Supabase Auto-backup, พิจารณา Manual Backup
*   **Principle of Least Privilege**: สิทธิ์เท่าที่จำเป็น
*   **Software Updates**: หมั่นอัปเดต Dependencies

## 12. การติดตั้งและเริ่มต้นใช้งาน (Setup and Getting Started)

1.  **Clone a repository:**
    ```bash
    git clone [Your-Repo-URL]
    cd inkrealm-project
    ```
2.  **Install dependencies:**
    ```bash
    npm install
    # หรือ
    yarn install
    # หรือ
    pnpm install
    ```
3.  **Set up Environment Variables:**
    คัดลอกไฟล์ `.env.example` (ถ้ามี) ไปเป็น `.env` หรือ `.env.local` และกรอกค่าที่จำเป็น:
    ```env
    PUBLIC_SUPABASE_URL="YOUR_SUPABASE_URL"
    PUBLIC_SUPABASE_ANON_KEY="YOUR_SUPABASE_ANON_KEY"
    # อาจจะมี SUPABASE_SERVICE_ROLE_KEY สำหรับ server-side operations

    PUBLIC_BASE_URL="http://localhost:5173" # สำหรับ local development
    ```
4.  **Run the development server:**
    ```bash
    npm run dev
    # หรือ
    yarn dev
    # หรือ
    pnpm dev
    ```
    เปิดเบราว์เซอร์ไปที่ `http://localhost:5173` (หรือ port ที่ SvelteKit กำหนด)

## 13. สีและธีม (Color Scheme & Theming)

*   **สีหลัก**: ทอง (Gold - เช่น `#FFD700`, `#B8860B`), เทา (Gray - เช่น `#808080`, `#A9A9A9`, `#2F2F2F`)
*   **โหมดสว่าง (Light Mode)**: พื้นหลังขาว/เทาอ่อน, ตัวอักษรเทาเข้ม/ดำ, Accent สีทอง
*   **โหมดมืด (Dark Mode)**: พื้นหลังดำ/เทาเข้ม, ตัวอักษรเทาอ่อน/ขาว, Accent สีทอง
*   **โลโก้**: อาจใช้ Gradient ทอง-เทา หรือสีทองเด่น
*   **การสลับธีม**: ใช้ความสามารถของ Skeleton UI หรือ TailwindCSS dark mode (`class="dark"`) ควบคุมผ่าน Svelte store และ localStorage เพื่อจดจำการตั้งค่าของผู้ใช้ การเปลี่ยนธีมจะต้องไม่ทำให้สีเพี้ยน และองค์ประกอบ UI อื่นๆ ต้องปรับตามอย่างถูกต้อง

---

โค้ด SQL สำหรับ Supabase (PostgreSQL)

นี่คือโค้ด SQL สำหรับสร้างตารางทั้งหมดที่เกี่ยวข้องกับระบบ INKREALM.CO V5.1:

-- ========== EXTENSIONS (ถ้ายังไม่มี) ==========
-- CREATE EXTENSION IF NOT EXISTS moddatetime SCHEMA extensions; -- สำหรับ auto updated_at
-- CREATE EXTENSION IF NOT EXISTS "uuid-ossp" SCHEMA extensions; -- สำหรับ uuid_generate_v4()

-- ========== ENUM TYPES (ถ้าต้องการใช้ แทน TEXT สำหรับบางฟิลด์) ==========
CREATE TYPE novel_status_enum AS ENUM ('ต่อเนื่อง', 'จบแล้ว', 'ยุติการลง', 'เผยแพร่', 'ฉบับร่าง');
CREATE TYPE chapter_status_enum AS ENUM ('เผยแพร่', 'ฉบับร่าง');
CREATE TYPE language_enum AS ENUM ('zh', 'en', 'th');
CREATE TYPE visibility_enum AS ENUM ('ทุกคน', 'เฉพาะสมาชิก', 'เฉพาะนักแปล'); -- อาจใช้ JSONB ถ้าต้องการหลายค่า

-- ========== ROLES TABLE ==========
CREATE TABLE public.roles (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL UNIQUE,
    description TEXT
);

-- Seed initial roles
INSERT INTO public.roles (name, description) VALUES
('Member', 'สมาชิกทั่วไป'),
('Translator', 'นักแปล'),
('Admin', 'ผู้ดูแลระบบ');

-- ========== PROFILES TABLE (เชื่อมกับ auth.users) ==========
CREATE TABLE public.profiles (
    id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
    display_name TEXT,
    avatar_url TEXT,
    role_id INTEGER NOT NULL REFERENCES public.roles(id) DEFAULT 1, -- Default to Member
    role_expiry_date TIMESTAMP WITH TIME ZONE NULL,
    is_suspended BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);

-- RLS for profiles
ALTER TABLE public.profiles ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow individual user to read their own profile" ON public.profiles FOR SELECT USING (auth.uid() = id);
CREATE POLICY "Allow individual user to update their own profile" ON public.profiles FOR UPDATE USING (auth.uid() = id) WITH CHECK (auth.uid() = id);
CREATE POLICY "Admins can manage all profiles" ON public.profiles FOR ALL USING (
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
);

-- Trigger to automatically update `updated_at`
CREATE TRIGGER handle_updated_at_profiles
BEFORE UPDATE ON public.profiles
FOR EACH ROW
EXECUTE FUNCTION extensions.moddatetime (updated_at);

-- Function to create a profile when a new user signs up
CREATE OR REPLACE FUNCTION public.handle_new_user()
RETURNS TRIGGER
LANGUAGE plpgsql
SECURITY DEFINER SET search_path = public
AS $$
BEGIN
  INSERT INTO public.profiles (id, display_name, role_id)
  VALUES (
    NEW.id,
    NEW.raw_user_meta_data->>'display_name', -- Or some default if not provided
    (SELECT id FROM public.roles WHERE name = 'Member') -- Default role
  );
  RETURN NEW;
END;
$$;

-- Trigger to call handle_new_user on new user creation
CREATE TRIGGER on_auth_user_created
  AFTER INSERT ON auth.users
  FOR EACH ROW EXECUTE PROCEDURE public.handle_new_user();


-- ========== CATEGORIES TABLE ==========
CREATE TABLE public.categories (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL UNIQUE,
    slug TEXT NOT NULL UNIQUE,
    description TEXT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);
-- RLS for categories
ALTER TABLE public.categories ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow public read access to categories" ON public.categories FOR SELECT USING (true);
CREATE POLICY "Allow admins to manage categories" ON public.categories FOR ALL USING (
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
);
-- Trigger for categories updated_at
CREATE TRIGGER handle_updated_at_categories
BEFORE UPDATE ON public.categories
FOR EACH ROW
EXECUTE FUNCTION extensions.moddatetime (updated_at);

-- ========== NOVELS TABLE ==========
CREATE TABLE public.novels (
    id UUID PRIMARY KEY DEFAULT extensions.uuid_generate_v4(),
    title TEXT NOT NULL,
    slug TEXT NOT NULL UNIQUE,
    synopsis TEXT,
    cover_image_url TEXT,
    category_id INTEGER REFERENCES public.categories(id) ON DELETE SET NULL,
    author_id UUID NOT NULL REFERENCES public.profiles(id) ON DELETE CASCADE, -- The translator/creator on this platform
    original_language language_enum NOT NULL DEFAULT 'th',
    status novel_status_enum NOT NULL DEFAULT 'ฉบับร่าง',
    visibility JSONB DEFAULT '["ทุกคน"]'::jsonb, -- e.g., ["ทุกคน"], ["เฉพาะสมาชิก"], ["เฉพาะนักแปล", "เฉพาะสมาชิก"]
    default_chapter_status chapter_status_enum NOT NULL DEFAULT 'เผยแพร่',
    default_chapter_access_level JSONB DEFAULT '["ทุกคน"]'::jsonb,
    total_chapters INTEGER DEFAULT 0,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);
-- Index for novels
CREATE INDEX idx_novels_author_id ON public.novels(author_id);
CREATE INDEX idx_novels_category_id ON public.novels(category_id);
CREATE INDEX idx_novels_status ON public.novels(status);
-- RLS for novels (Example - needs refinement based on exact logic)
ALTER TABLE public.novels ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow public read for published novels accessible to everyone"
  ON public.novels FOR SELECT USING (
    status = 'เผยแพร่' AND visibility @> '["ทุกคน"]'::jsonb
  );
CREATE POLICY "Allow member read for published novels accessible to members"
  ON public.novels FOR SELECT USING (
    status = 'เผยแพร่' AND visibility @> '["เฉพาะสมาชิก"]'::jsonb AND
    (SELECT role_id FROM profiles WHERE id = auth.uid()) IS NOT NULL -- checks if user is logged in
  );
CREATE POLICY "Allow translator/admin to read their novels regardless of status/visibility"
  ON public.novels FOR SELECT USING (
    author_id = auth.uid() OR
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
CREATE POLICY "Allow author (translator/admin) to create novels"
  ON public.novels FOR INSERT WITH CHECK (
    author_id = auth.uid() AND
    ( (SELECT role_id FROM profiles WHERE id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Translator') OR
      (SELECT role_id FROM profiles WHERE id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin') )
  );
CREATE POLICY "Allow author to update their own novels"
  ON public.novels FOR UPDATE USING (author_id = auth.uid()) WITH CHECK (author_id = auth.uid());
CREATE POLICY "Allow admin to update any novel"
  ON public.novels FOR UPDATE USING (
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
CREATE POLICY "Allow author to delete their own novels"
  ON public.novels FOR DELETE USING (author_id = auth.uid());
CREATE POLICY "Allow admin to delete any novel"
  ON public.novels FOR DELETE USING (
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
-- Trigger for novels updated_at
CREATE TRIGGER handle_updated_at_novels
BEFORE UPDATE ON public.novels
FOR EACH ROW
EXECUTE FUNCTION extensions.moddatetime (updated_at);


-- ========== CHAPTERS TABLE ==========
CREATE TABLE public.chapters (
    id UUID PRIMARY KEY DEFAULT extensions.uuid_generate_v4(),
    novel_id UUID NOT NULL REFERENCES public.novels(id) ON DELETE CASCADE,
    chapter_number INTEGER NOT NULL,
    title TEXT NOT NULL,
    -- slug TEXT NOT NULL, -- URL uses chapter_number, slug here might be for other purposes if needed
    content TEXT, -- HTML content from Rich Text Editor
    content_original TEXT, -- Original text for translator mode
    status chapter_status_enum NOT NULL DEFAULT 'ฉบับร่าง',
    access_level JSONB DEFAULT '["ทุกคน"]'::jsonb, -- e.g., ["ทุกคน"], ["เฉพาะสมาชิก"]
    created_by UUID NOT NULL REFERENCES public.profiles(id) ON DELETE RESTRICT, -- Creator of the chapter (usually same as novel author)
    word_count INTEGER DEFAULT 0,
    read_by_users JSONB DEFAULT '[]'::jsonb, -- Array of user_ids who have read this chapter
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    CONSTRAINT unique_novel_chapter_number UNIQUE (novel_id, chapter_number)
);
-- Index for chapters
CREATE INDEX idx_chapters_novel_id ON public.chapters(novel_id);
-- RLS for chapters (Example - needs refinement)
ALTER TABLE public.chapters ENABLE ROW LEVEL SECURITY;
-- Public read for published chapters accessible to everyone
CREATE POLICY "Public read access for chapters" ON public.chapters
  FOR SELECT USING (
    status = 'เผยแพร่' AND access_level @> '["ทุกคน"]'::jsonb AND
    (SELECT novels.status FROM public.novels WHERE novels.id = chapters.novel_id) = 'เผยแพร่' AND
    (SELECT novels.visibility FROM public.novels WHERE novels.id = chapters.novel_id) @> '["ทุกคน"]'::jsonb
  );
-- Member read for published chapters accessible to members
CREATE POLICY "Member read access for chapters" ON public.chapters
  FOR SELECT USING (
    status = 'เผยแพร่' AND access_level @> '["เฉพาะสมาชิก"]'::jsonb AND
    (SELECT novels.status FROM public.novels WHERE novels.id = chapters.novel_id) = 'เผยแพร่' AND
    (SELECT novels.visibility FROM public.novels WHERE novels.id = chapters.novel_id) @> '["เฉพาะสมาชิก"]'::jsonb AND
    (SELECT role_id FROM profiles WHERE id = auth.uid()) IS NOT NULL
  );
-- Translator read for their own chapters
CREATE POLICY "Translator/Admin read their own/managed chapters" ON public.chapters
  FOR SELECT USING (
    created_by = auth.uid() OR
    (SELECT novels.author_id FROM public.novels WHERE novels.id = chapters.novel_id) = auth.uid() OR
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
-- Allow novel author or admin to manage chapters
CREATE POLICY "Author/Admin can manage chapters of their novels" ON public.chapters
  FOR ALL USING (
    (SELECT novels.author_id FROM public.novels WHERE novels.id = chapters.novel_id) = auth.uid() OR
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  ) WITH CHECK (
    (SELECT novels.author_id FROM public.novels WHERE novels.id = chapters.novel_id) = auth.uid() OR
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
-- Trigger for chapters updated_at
CREATE TRIGGER handle_updated_at_chapters
BEFORE UPDATE ON public.chapters
FOR EACH ROW
EXECUTE FUNCTION extensions.moddatetime (updated_at);

-- Function to update novel's total_chapters count
CREATE OR REPLACE FUNCTION public.update_novel_total_chapters()
RETURNS TRIGGER
LANGUAGE plpgsql
AS $$
BEGIN
  IF (TG_OP = 'INSERT') THEN
    UPDATE public.novels
    SET total_chapters = total_chapters + 1
    WHERE id = NEW.novel_id;
  ELSIF (TG_OP = 'DELETE') THEN
    UPDATE public.novels
    SET total_chapters = total_chapters - 1
    WHERE id = OLD.novel_id;
  END IF;
  RETURN NULL; -- result is ignored since this is an AFTER trigger
END;
$$;
-- Trigger to update total_chapters
CREATE TRIGGER after_chapter_insert_delete
AFTER INSERT OR DELETE ON public.chapters
FOR EACH ROW EXECUTE FUNCTION public.update_novel_total_chapters();


-- ========== USER_BOOKMARKS TABLE ==========
CREATE TABLE public.user_bookmarks (
    id SERIAL PRIMARY KEY,
    user_id UUID NOT NULL REFERENCES public.profiles(id) ON DELETE CASCADE,
    chapter_id UUID NOT NULL REFERENCES public.chapters(id) ON DELETE CASCADE,
    novel_id UUID NOT NULL REFERENCES public.novels(id) ON DELETE CASCADE, -- Denormalized for easier querying
    bookmarked_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    CONSTRAINT unique_user_chapter_bookmark UNIQUE (user_id, chapter_id)
);
-- Index for user_bookmarks
CREATE INDEX idx_user_bookmarks_user_novel ON public.user_bookmarks(user_id, novel_id);
-- RLS for user_bookmarks
ALTER TABLE public.user_bookmarks ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users can manage their own bookmarks" ON public.user_bookmarks
  FOR ALL USING (user_id = auth.uid()) WITH CHECK (user_id = auth.uid());


-- ========== MARK_LINES TABLE (for Translator Mode) ==========
CREATE TABLE public.mark_lines (
    id SERIAL PRIMARY KEY,
    chapter_id UUID NOT NULL REFERENCES public.chapters(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES public.profiles(id) ON DELETE CASCADE, -- Translator who marked
    line_number INTEGER NOT NULL, -- The line number in content_original
    marked_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    CONSTRAINT unique_mark_line_user_chapter UNIQUE (chapter_id, user_id, line_number)
);
-- Index for mark_lines
CREATE INDEX idx_mark_lines_user_chapter ON public.mark_lines(user_id, chapter_id);
-- RLS for mark_lines
ALTER TABLE public.mark_lines ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Translators can manage their own marks" ON public.mark_lines
  FOR ALL USING (
    user_id = auth.uid() AND
    ( (SELECT role_id FROM profiles WHERE id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Translator') OR
      (SELECT role_id FROM profiles WHERE id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin') )
  ) WITH CHECK (
    user_id = auth.uid() AND
    ( (SELECT role_id FROM profiles WHERE id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Translator') OR
      (SELECT role_id FROM profiles WHERE id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin') )
  );
CREATE POLICY "Admins can view all marks" ON public.mark_lines
    FOR SELECT USING ((SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin'));


-- ========== ANNOUNCEMENTS TABLE ==========
CREATE TABLE public.announcements (
    id SERIAL PRIMARY KEY,
    title TEXT NOT NULL,
    content TEXT NOT NULL, -- Markdown content
    start_date TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT timezone('utc'::text, now()),
    end_date TIMESTAMP WITH TIME ZONE NOT NULL,
    target_roles JSONB DEFAULT '["ทุกคน"]'::jsonb, -- e.g., ["Member"], ["Translator"] or role_ids
    target_pages JSONB DEFAULT '["หน้าแรก"]'::jsonb, -- e.g., ["หน้าแรก"], ["หน้านิยาย (ทุกเรื่อง)"], ["หน้านิยาย (เฉพาะเรื่อง):novel_id_or_slug"]
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);
-- RLS for announcements
ALTER TABLE public.announcements ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow public read for active announcements based on role and page" ON public.announcements
  FOR SELECT USING (
    is_active = TRUE AND
    timezone('utc'::text, now()) BETWEEN start_date AND end_date AND
    ( target_roles @> jsonb_build_array((SELECT r.name FROM public.roles r JOIN public.profiles p ON p.role_id = r.id WHERE p.id = auth.uid())) OR
      target_roles @> '["ทุกคน"]'::jsonb OR
      (auth.uid() IS NULL AND target_roles @> '["ผู้เยี่ยมชม"]'::jsonb) -- For guests if you add "ผู้เยี่ยมชม"
      -- Logic for target_pages needs to be handled client-side or via RPC, as RLS cannot easily know the current page.
    )
  );
CREATE POLICY "Allow admins to manage announcements" ON public.announcements
  FOR ALL USING (
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
-- Trigger for announcements updated_at
CREATE TRIGGER handle_updated_at_announcements
BEFORE UPDATE ON public.announcements
FOR EACH ROW
EXECUTE FUNCTION extensions.moddatetime (updated_at);


-- ========== SITE_SETTINGS TABLE ==========
CREATE TABLE public.site_settings (
    key TEXT PRIMARY KEY,
    value JSONB,
    description TEXT,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);
-- Seed initial site settings
INSERT INTO public.site_settings (key, value, description) VALUES
('site_name', '"INKREALM.CO"'::jsonb, 'ชื่อเว็บไซต์หลัก'),
('site_description', '"แพลตฟอร์มนิยายแปลสำหรับทุกคน"'::jsonb, 'คำอธิบายเว็บไซต์สำหรับ SEO'),
('logo_url', 'null'::jsonb, 'URL ของโลโก้เว็บไซต์'),
('favicon_url', 'null'::jsonb, 'URL ของ Favicon'),
('allow_registration', 'true'::jsonb, 'เปิด/ปิดการลงทะเบียนใหม่ (true/false)'),
('allow_google_login', 'true'::jsonb, 'เปิด/ปิดการเข้าสู่ระบบด้วย Google (true/false)'),
('default_items_per_page', '20'::jsonb, 'จำนวนรายการต่อหน้าเริ่มต้นสำหรับการแบ่งหน้า'),
('novel_default_chapter_status', '"เผยแพร่"'::jsonb, 'สถานะตอนเริ่มต้นสำหรับนิยายใหม่ ("เผยแพร่" หรือ "ฉบับร่าง")'),
('novel_default_chapter_access', '["ทุกคน"]'::jsonb, 'สิทธิ์เข้าถึงตอนเริ่มต้นสำหรับนิยายใหม่ (JSON Array)');
-- RLS for site_settings
ALTER TABLE public.site_settings ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow public read for site settings" ON public.site_settings FOR SELECT USING (true);
CREATE POLICY "Allow admins to manage site settings" ON public.site_settings
  FOR ALL USING (
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
-- Trigger for site_settings updated_at
CREATE TRIGGER handle_updated_at_site_settings
BEFORE UPDATE ON public.site_settings
FOR EACH ROW
EXECUTE FUNCTION extensions.moddatetime (updated_at);


-- ========== SITE_CHANGELOGS TABLE ==========
CREATE TABLE public.site_changelogs (
    id SERIAL PRIMARY KEY,
    version_number TEXT NOT NULL UNIQUE,
    release_date DATE NOT NULL,
    summary TEXT, -- Markdown content
    is_published BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);
-- RLS for site_changelogs
ALTER TABLE public.site_changelogs ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow public read for published changelogs" ON public.site_changelogs FOR SELECT USING (is_published = TRUE);
CREATE POLICY "Allow admins to manage changelogs" ON public.site_changelogs
  FOR ALL USING (
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );


-- ========== NOVEL_VERSIONS TABLE (for tracking changes to novel metadata) ==========
CREATE TABLE public.novel_versions (
    id SERIAL PRIMARY KEY,
    novel_id UUID NOT NULL REFERENCES public.novels(id) ON DELETE CASCADE,
    version_number INTEGER NOT NULL,
    metadata_snapshot JSONB NOT NULL, -- Snapshot of novel fields (title, synopsis, category, status, etc.)
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    created_by UUID REFERENCES public.profiles(id) ON DELETE SET NULL, -- User who made the change
    notes TEXT, -- Optional notes about the version
    UNIQUE(novel_id, version_number)
);
-- RLS for novel_versions
ALTER TABLE public.novel_versions ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow admins and novel author to read novel versions" ON public.novel_versions
  FOR SELECT USING (
    (SELECT novels.author_id FROM public.novels WHERE novels.id = novel_versions.novel_id) = auth.uid() OR
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
CREATE POLICY "Allow admins to manage novel versions (primarily for restore, creation is programmatic)" ON public.novel_versions
  FOR ALL USING ( -- Be cautious with direct manipulation
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );


-- ========== CHAPTER_VERSIONS TABLE (for tracking changes to chapter content) ==========
CREATE TABLE public.chapter_versions (
    id SERIAL PRIMARY KEY,
    chapter_id UUID NOT NULL REFERENCES public.chapters(id) ON DELETE CASCADE,
    version_number INTEGER NOT NULL,
    content_snapshot TEXT, -- Snapshot of chapter's HTML content
    content_original_snapshot TEXT, -- Snapshot of chapter's original text content
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    created_by UUID REFERENCES public.profiles(id) ON DELETE SET NULL, -- User who made the change
    notes TEXT, -- Optional notes about the version
    UNIQUE(chapter_id, version_number)
);
-- RLS for chapter_versions
ALTER TABLE public.chapter_versions ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow admins and chapter author to read chapter versions" ON public.chapter_versions
  FOR SELECT USING (
    (SELECT chapters.created_by FROM public.chapters WHERE chapters.id = chapter_versions.chapter_id) = auth.uid() OR
    (SELECT novels.author_id FROM public.novels JOIN public.chapters ON novels.id = chapters.novel_id WHERE chapters.id = chapter_versions.chapter_id) = auth.uid() OR
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
CREATE POLICY "Allow admins to manage chapter versions (primarily for restore, creation is programmatic)" ON public.chapter_versions
  FOR ALL USING ( -- Be cautious with direct manipulation
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );

-- ========== STORAGE BUCKETS SETUP (Run these in Supabase SQL Editor or via Management API) ==========
-- Note: RLS for storage is typically managed through policies on the storage.objects table
-- For `novel-covers`: Public read, authenticated insert/update/delete by author/admin
-- For `avatars`: Public read, authenticated insert/update by owner, admin can manage
-- For `site_assets`: Public read, admin insert/update/delete
-- For `chapter-text-files`: Private, authenticated insert by author/admin, server-side delete after processing.

-- Example for novel-covers (you'll need to adjust based on your exact RLS needs for storage)
/*
INSERT INTO storage.buckets (id, name, public)
VALUES ('novel-covers', 'novel-covers', true); -- Public true means files are accessible via URL without token if policy allows

CREATE POLICY "Allow authenticated users to upload covers"
ON storage.objects FOR INSERT TO authenticated WITH CHECK (bucket_id = 'novel-covers');
-- Add more specific policies for select, update, delete based on user roles and ownership.
-- e.g., only novel author or admin can update/delete their novel's cover.
-- This often involves checking against a metadata column in storage.objects or joining with your public tables.
-- Supabase documentation provides good examples for storage RLS.
*/

-- Example for avatars
/*
INSERT INTO storage.buckets (id, name, public)
VALUES ('avatars', 'avatars', true);

CREATE POLICY "Allow users to upload their own avatar"
ON storage.objects FOR INSERT TO authenticated WITH CHECK (
    bucket_id = 'avatars' AND owner = auth.uid()
);
CREATE POLICY "Allow users to update their own avatar"
ON storage.objects FOR UPDATE TO authenticated USING (
    bucket_id = 'avatars' AND owner = auth.uid()
);
CREATE POLICY "Allow users to view their own avatar or public avatars" -- (if avatars are public)
ON storage.objects FOR SELECT USING (bucket_id = 'avatars');
*/

-- Remember to set up RLS policies for your storage buckets according to your access control needs.
-- The `public` flag on a bucket just makes it *possible* for files to be public, RLS still applies.
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
SQL
IGNORE_WHEN_COPYING_END

หวังว่านี่จะครอบคลุมและละเอียดตามที่คุณต้องการนะครับ! หากมีส่วนไหนต้องการปรับแก้อีก บอกได้เลยครับ
