โอเคครับ! นี่คือฉบับปรับปรุง README.md และรายละเอียดฐานข้อมูล SQL สำหรับ INKREALM.CO (V5.1) ที่คุณต้องการ โดยเน้นความละเอียดและความครบถ้วนตามที่คุณระบุมาทั้งหมด

# INKREALM.CO: ข้อกำหนดระบบและคุณสมบัติหลัก (V5.1) - README

เอกสารนี้เป็นข้อกำหนดระบบและคุณสมบัติหลักสำหรับแพลตฟอร์ม INKREALM.CO (V5.1) โดยเน้นฟังก์ชันที่จำเป็นตามการปรับปรุงล่าสุด Tech Stack หลักคือ SvelteKit และ Supabase ออกแบบมาเพื่อความสะดวกในการปรับใช้, ทดสอบ, และย้ายไปยังโดเมนต่างๆ

## 1. ปรัชญาการออกแบบโดยรวม

*   **การแสดงผลหรูหราและอ่านง่าย:**
    *   **สีหลัก:** สีทอง (Gold)
    *   **สีรอง:** สีเทา (Gray)
    *   **โหมดสว่าง (Light Mode):** พื้นหลังขาว, ตัวอักษรเทาเข้ม/ดำ, องค์ประกอบเน้นสีทอง
    *   **โหมดมืด (Dark Mode):** พื้นหลังเทาเข้ม/ดำ, ตัวอักษรเทาอ่อน/ขาว, องค์ประกอบเน้นสีทอง
    *   **ฟอนต์หลัก:** Sarabun สำหรับเนื้อหาทั้งหมด
    *   **โลโก้:** อาจใช้เทคนิค Gradient สีทอง-เทา หรือตามความเหมาะสม
*   **เครื่องมือสำหรับนักแปล:** มีฟังก์ชันสนับสนุนการทำงานของนักแปลโดยเฉพาะ
*   **ใช้งานง่าย ตอบโจทย์ทุกอุปกรณ์:** UI ใช้งานง่าย (Intuitive), Responsive Design, หน้าอ่านไม่มี Horizontal Scroll
*   **ความยืดหยุ่นและการทดสอบ:** โครงสร้างที่เอื้อต่อการเปลี่ยนแปลงโดเมนและการทดสอบระบบที่ง่าย

## 2. ภาพรวมระบบหลัก (Core System Overview)

*   **ระบบยืนยันตัวตนและจัดการผู้ใช้:** ลงทะเบียน, เข้าสู่ระบบ (อีเมล/รหัสผ่าน, Google), ลืมรหัสผ่าน, จัดการโปรไฟล์, บทบาทและสิทธิ์
*   **ระบบจัดการเนื้อหานิยาย (CMS):** การสร้าง/แก้ไข/ลบนิยายและตอน (ผ่าน Modals เป็นหลัก), อัปโหลดเนื้อหา (ปก, .txt), การจัดการหมวดหมู่นิยาย (สำหรับ Admin), ระบบจัดการเวอร์ชันสำหรับนิยายและตอน
*   **ระบบการอ่านและบุ๊คมาร์ค:** หน้าอ่านที่ปรับแต่งได้, ระบบบุ๊คมาร์คตอน, โหมดอ่านต่อเนื่อง, การสลับโหมดการอ่านสำหรับนักแปล
*   **ระบบเครื่องมือสำหรับนักแปล:** โหมดแปล (เมื่อเปิดใช้งาน), ระบบบรรทัดมาร์ค, หน้าจัดการบรรทัดมาร์ค
*   **ระบบแดชบอร์ด:** สำหรับนักแปลและผู้ดูแลระบบ (รวมถึงหน้าจัดการผู้ใช้, จัดการหมวดหมู่, จัดการประกาศ, หน้าจัดการเว็บไซต์, และ หน้าจัดการเวอร์ชัน)
*   **ระบบฐานข้อมูลและไฟล์:** Supabase (PostgreSQL, Storage)
*   **ระบบการแจ้งเตือนผู้ใช้ (User Feedback System):** การใช้ Toast notifications สำหรับการยืนยันการกระทำต่างๆ (แสดงมุมขวาล่าง)
*   **ระบบประกาศ (Announcement System):** สำหรับ Admin ในการสร้างและเผยแพร่ประกาศไปยังผู้ใช้

## 3. โครงสร้าง URL ที่เป็นมิตรและการจัดการโดเมน

เพื่อให้ URL ของเว็บไซต์ INKREALM.CO อ่านง่าย, เป็นมิตรต่อผู้ใช้, ดีต่อ SEO, และง่ายต่อการย้ายโดเมน:

*   **โครงสร้าง URL หลัก:**
    *   หน้ารวมนิยาย (หน้าแรก): `/`
    *   หน้าข้อมูลนิยาย: `/novels/[novel_slug]`
    *   หน้าอ่านตอนนิยาย: `/novels/[novel_slug]/chapter/[chapter_number]`
    *   หน้าโปรไฟล์ผู้ใช้: `/profile`
    *   หน้าบุ๊คมาร์ค: `/bookmarks`
    *   แดชบอร์ดนักแปล: `/translator/dashboard`
    *   หน้าจัดการบรรทัดมาร์ค (นักแปล): `/translator/manage-marks/[novel_slug]`
    *   แดชบอร์ดผู้ดูแลระบบ: `/admin/dashboard`
    *   หน้ารายละเอียดในแดชบอร์ดผู้ดูแล: `/admin/dashboard/users`, `/admin/dashboard/categories` ฯลฯ

*   **การสร้าง Slug:**
    *   **นิยาย:** สร้างอัตโนมัติจากชื่อเรื่อง (ตัวพิมพ์เล็ก, แทนที่ช่องว่างด้วยขีด, ตัดอักขระพิเศษ) ผู้สร้าง/Admin แก้ไขได้ (ตรวจสอบความซ้ำซ้อน)
    *   **ตอนนิยาย:** ใช้ `chapter_number` โดยตรง

*   **การจัดการโดเมนและ Base URL:**
    *   **Environment Variables:**
        *   `PUBLIC_BASE_URL`: เช่น `https://inkrealm.co` หรือ `http://localhost:3000` (สำหรับการพัฒนา)
        *   `PUBLIC_SUPABASE_URL`: URL ของ Supabase project
        *   `PUBLIC_SUPABASE_ANON_KEY`: Anon key ของ Supabase project
    *   **Internal Linking:** ใช้ Relative Paths หรืออ้างอิง `PUBLIC_BASE_URL`

## 4. บทบาทผู้ใช้และสิทธิ์ (User Roles & Permissions)

ควบคุมโดย Supabase Row-Level Security (RLS).

| บทบาท         | คำอธิบายสิทธิ์                                                                                                                                                                                                                                                                                          |
| :------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **สมาชิก (Member)** | อ่านนิยาย, บุ๊คมาร์คตอน, แก้ไขโปรไฟล์ส่วนตัว แถบเครื่องมืออ่าน: สารบัญ, ปรับแต่งการแสดงผล, บุ๊คมาร์ค, สลับธีมเว็บ, เปิด/ปิดโหมดอ่านต่อเนื่อง, นำทางตอน                                                                                                                                                           |
| **นักแปล (Translator)** | สิทธิ์ทั้งหมดของสมาชิก + เปิด/ปิด "โหมดนักแปล" สำหรับนิยายที่ตนมีสิทธิ์, เข้าถึงโหมดแปล, ใช้งานบรรทัดมาร์ค, สร้าง/จัดการนิยายและตอนของตนเอง (ผ่าน Modal), เข้าถึงแดชบอร์ดนักแปลและหน้าจัดการบรรทัดมาร์ค แถบเครื่องมือพิเศษในโหมดแปล: เพิ่ม "คัดลอกมาร์คทั้งหมด (ตอนปัจจุบัน)"                           |
| **ผู้ดูแลระบบ (Admin)** | สิทธิ์ทั้งหมดของนักแปล + เข้าถึงแดชบอร์ดผู้ดูแลระบบ, จัดการผู้ใช้, ดูข้อมูลมาร์คทั้งหมด, จัดการเว็บไซต์, จัดการหมวดหมู่นิยาย, จัดการประกาศ, จัดการเวอร์ชัน                                                                                                                                                       |

## 5. การเข้าสู่ระบบ / ลงทะเบียน / ลืมรหัสผ่าน

*   **ความสามารถ:** ลงทะเบียน, เข้าสู่ระบบ (อีเมล/รหัสผ่าน, Google), ลืมรหัสผ่าน
*   **การแสดงผล:** Modal เป็นหลัก; หน้าเว็บเฉพาะ (`/login`, `/register`, `/forgot-password`) สำหรับ Direct Access
*   **การพัฒนา:** Supabase Auth, Felte + Zod, Skeleton UI
*   **การแจ้งเตือน (Toast):**
    *   ลงทะเบียนสำเร็จ: "ลงทะเบียนบัญชีผู้ใช้สำเร็จ กรุณาตรวจสอบอีเมลเพื่อยืนยันบัญชี"
    *   เข้าสู่ระบบสำเร็จ: "เข้าสู่ระบบสำเร็จ ยินดีต้อนรับ!"
    *   ส่งคำขอรีเซ็ตรหัสผ่าน: "คำขอรีเซ็ตรหัสผ่านถูกส่งไปยังอีเมลของคุณแล้ว กรุณาตรวจสอบกล่องจดหมาย"
    *   ออกจากระบบ: "ออกจากระบบเรียบร้อยแล้ว"

## 6. โครงสร้างแถบนำทาง (Navbar Structure)

*   **ความสามารถ:** แสดงเมนูตามบทบาท (Dynamic Menu), Responsive
*   **ตัวอย่างเมนู:**
    *   **Guest:** โลโก้, ปุ่มสลับธีม (`Sun`/`Moon`), ปุ่ม "เข้าสู่ระบบ" (เปิด Login Modal)
    *   **Member:** โลโก้, "หน้าแรก", "บุ๊คมาร์ค", ปุ่มสลับธีม, โปรไฟล์ (Dropdown: ชื่อ, สิทธิ์, "หน้าโปรไฟล์", "ออกจากระบบ")
    *   **Translator:** เหมือน Member + "แดชบอร์ดนักแปล"
    *   **Admin:** เหมือน Translator + "แดชบอร์ดผู้ดูแลระบบ"
*   **การพัฒนา:** SvelteKit, Skeleton UI, TailwindCSS, Lucide-Svelte (สำหรับไอคอน)

## 7. รายละเอียดหน้าเว็บไซต์, Modals, และการจัดการเนื้อหา

### 7.1 หน้าแรก (Home Page - `/`)

*   **ส่วนแสดงประกาศ:** แสดงประกาศที่ Active และตรงตามสิทธิ์ผู้ใช้ (แบนเนอร์บนสุด, มีปุ่มปิด `X`)
*   **ส่วนเครื่องมือหลัก:**
    *   ช่องค้นหานิยาย: Input สำหรับคำค้น
    *   ฟิลเตอร์หมวดหมู่: Dropdown (ดึงจาก `categories` table)
    *   ฟิลเตอร์สถานะนิยาย: Dropdown ("ต่อเนื่อง", "จบแล้ว")
    *   ปุ่มสร้างนิยาย (นักแปล/Admin): "[+] สร้างนิยาย" (เปิด Modal 7.1.1)
*   **การแสดงรายการนิยาย:** Grid Layout (รูปปก 3:4, ชื่อเรื่อง, จำนวนตอน), Mobile 2 คอลัมน์
*   **ระบบแบ่งหน้า (Pagination)**
*   **การทำงานเริ่มต้น (Guest):** แสดง Login Modal อัตโนมัติเมื่อเข้าชมครั้งแรก (อาจมีตัวเลือก "ข้ามไปก่อน")
    *   **Toast:** (ถ้ามีการแจ้งเตือน) "กรุณาเข้าสู่ระบบเพื่อประสบการณ์การใช้งานเต็มรูปแบบ"

#### 7.1.1 Modal สร้างนิยาย (Create Novel Modal)

*   **เรียกใช้งาน:** ปุ่ม "[+] สร้างนิยาย" บนหน้าแรก
*   **UI ภายใน Modal:**
    *   หัวเรื่อง: "สร้างนิยายใหม่"
    *   ฟอร์ม:
        *   ชื่อเรื่อง (Title): Input text, จำเป็น
        *   คำโปรย (Synopsis): Textarea
        *   หมวดหมู่ (Category): Dropdown (จาก `categories`)
        *   รูปปกนิยาย (Cover Image): อัปโหลด (Crop 3:4, ย่อขนาดไฟล์ < 3MB อัตโนมัติ)
        *   ภาษาต้นฉบับ (Original Language): Dropdown ("จีน (zh)", "อังกฤษ (en)", "ไทย (th)")
        *   สิทธิ์ในการมองเห็นนิยาย (Novel Visibility): Checkbox (เลือกหลายอัน: "ทุกคน", "เฉพาะสมาชิก", "เฉพาะนักแปล")
        *   **การตั้งค่าเริ่มต้นสำหรับตอนใหม่ของนิยายนี้:**
            *   สถานะเริ่มต้นของตอน: Radio Group ("เผยแพร่" (ค่าเริ่มต้น), "ฉบับร่าง")
            *   สิทธิ์การเข้าถึงตอนเริ่มต้น: Checkbox (เลือกหลายอัน: "ทุกคน" (ค่าเริ่มต้น), "เฉพาะสมาชิก", "เฉพาะนักแปล")
*   **ปุ่มดำเนินการ:**
    *   "เผยแพร่นิยาย": **Toast:** "สร้างนิยาย '[ชื่อนิยาย]' และเผยแพร่สำเร็จแล้ว!"
    *   "บันทึกเป็นฉบับร่าง": **Toast:** "สร้างนิยาย '[ชื่อนิยาย]' เป็นฉบับร่างสำเร็จแล้ว"
    *   "ยกเลิก"

### 7.2 หน้าข้อมูลนิยายและสารบัญ (Novel Detail Page - `/novels/[novel_slug]`)

*   **ส่วนแสดงประกาศ:** แสดงประกาศที่ Active และตรงตามสิทธิ์/เป้าหมาย
*   **ส่วนข้อมูลนิยาย:** รูปปก, ชื่อเรื่อง, ผู้แต่ง, หมวดหมู่, คำโปรย, จำนวนตอน, สถานะ, ภาษาต้นฉบับ (`<html lang="...">`)
    *   (นักแปล/Admin เจ้าของ) ปุ่มสลับ "โหมดนักแปล": Toggle Switch "เปิด/ปิดโหมดนักแปล" (บันทึกสถานะเฉพาะผู้ใช้และนิยายนี้)
        *   **Toast (เมื่อเปลี่ยนสถานะ):** "เปิดโหมดนักแปลสำหรับนิยาย '[ชื่อนิยาย]' แล้ว" / "ปิดโหมดนักแปลสำหรับนิยาย '[ชื่อนิยาย]' แล้ว"
*   **ปุ่มจัดการนิยาย (ผู้สร้าง/Admin):**
    *   "แก้ไขข้อมูลนิยาย": เปิด Modal 7.2.1
    *   "ลบนิยาย": Modal ยืนยัน "คุณแน่ใจหรือไม่ว่าต้องการลบนิยาย '[ชื่อนิยาย]'? การกระทำนี้ไม่สามารถย้อนกลับได้"
        *   **Toast (เมื่อลบสำเร็จ):** "ลบนิยาย '[ชื่อนิยาย]' เรียบร้อยแล้ว"
*   **ส่วนเพิ่มตอน (ผู้สร้าง/Admin):**
    *   ปุ่ม: "เพิ่มตอนด้วย .txt" (เปิด Modal 7.2.2), "เพิ่มตอน" (เปิด Modal 7.2.3)
*   **ส่วนสารบัญ:**
    *   Collapse/Accordion, ปรับจำนวนตอนต่อกลุ่ม (10, 20, 25, 30, 50 (ค่าเริ่มต้น), 100), กลุ่มแรกขยายเสมอ
    *   รายการตอน:
        *   (ทุกคน): ไอคอนบุ๊คมาร์ค (`Bookmark`), ชื่อตอน, สถานะการอ่าน (PC: พื้นหลัง+`Eye`+"อ่านแล้ว"; Mobile: พื้นหลัง+`Eye`)
        *   (นักแปล/Admin เจ้าของ): ไอคอนสถานะตอน (`CircleSlash` / `CheckCircle2`), ปุ่ม "แก้ไขตอน" (`Edit3`), ปุ่ม "ลบตอน" (`Trash2`), ปุ่ม "คัดลอกมาร์คทั้งหมด" (`CopyCheck`) (สำหรับตอนนั้นๆ)
            *   **Toast (ลบตอน):** "ลบตอน '[ชื่อตอน]' เรียบร้อยแล้ว"
            *   **Toast (คัดลอกมาร์คตอน):** "คัดลอกบรรทัดที่มาร์คทั้งหมดของตอน '[ชื่อตอน]' ไปยังคลิปบอร์ดแล้ว"

#### 7.2.1 Modal แก้ไขข้อมูลนิยาย (Edit Novel Modal)

*   **UI:** ฟอร์มเติมข้อมูลปัจจุบัน
*   **ฟิลด์แก้ไขได้:** ชื่อเรื่อง, คำโปรย, หมวดหมู่, รูปปก (Crop 3:4, <3MB), ภาษาต้นฉบับ, สถานะนิยาย (Radio: "ต่อเนื่อง", "จบแล้ว", "ยุติการลง"), สิทธิ์การมองเห็น (Checkbox)
*   **ค่าเริ่มต้นสำหรับตอนใหม่ของนิยายนี้:** สถานะตอน (Radio), สิทธิ์เข้าถึงตอน (Checkbox)
*   **ปุ่ม:**
    *   "บันทึกการเปลี่ยนแปลง": **Toast:** "บันทึกข้อมูลนิยาย '[ชื่อนิยาย]' สำเร็จแล้ว" (สร้าง `novel_versions` ใหม่)
    *   "ยกเลิก"
    *   "ลบนิยาย" (ภายใน Modal, Modal ยืนยันอีกครั้ง): **Toast:** "ลบนิยาย '[ชื่อนิยาย]' เรียบร้อยแล้ว"

#### 7.2.2 Modal เพิ่มตอนด้วย .txt (Batch Upload Modal)

*   **ความสามารถ:** Drag & Drop, Max 500 ไฟล์, Sort ชื่อไฟล์, ชื่อไฟล์เป็นชื่อตอน, เชื่อม `novel_id`, จัดการไฟล์ซ้ำ (Overwrite/Skip), Progress Bar
*   **Toast (เมื่อสำเร็จ):** "อัปโหลดตอนด้วย .txt สำเร็จ: เพิ่ม X ตอน, เขียนทับ Y ตอน, ข้าม Z ตอน"
*   **การทำงานเบื้องหลัง:** สร้าง `mark_lines` ว่างๆ และ `chapter_versions` เริ่มต้นอัตโนมัติ

#### 7.2.3 Modal เพิ่ม/แก้ไขตอน (Create/Edit Chapter Modal)

*   **UI:**
    *   หัวเรื่อง: "เพิ่มตอนใหม่" / "แก้ไขตอน: [ชื่อตอนปัจจุบัน]"
    *   ฟอร์ม:
        *   ชื่อตอน (Chapter Title): Input text, จำเป็น
        *   เนื้อหา (Content): Rich Text Editor (TipTap - หนา, เอียง, ขีดเส้นใต้, list, link, ฟอนต์ Sarabun, คงรูปแบบจาก Word)
        *   สถานะตอน (Chapter Status): Radio Group ("เผยแพร่", "ฉบับร่าง")
        *   สิทธิ์การเข้าถึงตอน (Chapter Access Level): Checkbox ("ทุกคน", "เฉพาะสมาชิก", "เฉพาะนักแปล")
        *   *หมายเหตุ:* ค่าเริ่มต้นดึงจาก Novel settings แต่ปรับเปลี่ยนได้
*   **ปุ่ม:**
    *   เพิ่มตอน: "เผยแพร่ตอน", "บันทึกเป็นฉบับร่าง"
    *   แก้ไขตอน: "บันทึกการเปลี่ยนแปลง" (สร้าง `chapter_versions` ใหม่)
    *   (ทั้งสองกรณี): "ยกเลิก"
*   **Toast (เมื่อสำเร็จ):** "บันทึกตอน '[ชื่อตอน]' เรียบร้อยแล้ว"
*   **การทำงานเบื้องหลัง (สร้างใหม่):** สร้าง `mark_lines` ว่างๆ และ `chapter_versions` เริ่มต้น

### 7.3 หน้าบุ๊คมาร์ค (Bookmarks Page - `/bookmarks`)

*   **ความสามารถ:**
    *   รายการนิยายที่บุ๊คมาร์ค (เรียงตามวันที่บุ๊คมาร์คล่าสุดของนิยาย)
    *   แต่ละนิยาย: ปก, ชื่อ, ตอนล่าสุดที่บุ๊คมาร์ค, ปุ่ม "ไปยังบุ๊คมาร์คล่าสุด" (`PlayCircle`)
    *   ปุ่ม "จัดการบุ๊คมาร์ค" (ต่อเรื่อง, `Settings`): เปิด Modal
        *   **Modal "จัดการบุ๊คมาร์คสำหรับเรื่อง: [ชื่อนิยาย]"**:
            *   เรียงลำดับ (ตอน/วันที่), รายการ (Checkbox, ชื่อตอน, วันที่, ไฮไลท์ล่าสุด)
            *   ปุ่ม "ลบที่เลือก": **Toast:** "ลบบุ๊คมาร์ค X รายการสำเร็จ"
            *   ปุ่ม "ล้างบุ๊คมาร์คทั้งหมดของเรื่องนี้" (`Trash2`): Modal ยืนยัน "คุณแน่ใจหรือไม่ว่าต้องการล้างบุ๊คมาร์คทั้งหมดของเรื่อง '[ชื่อนิยาย]'?"
                *   **Toast (สำเร็จ):** "ล้างบุ๊คมาร์คทั้งหมดของเรื่อง '[ชื่อนิยาย]' สำเร็จแล้ว"
            *   ปุ่ม "บันทึก/เสร็จสิ้น", "ยกเลิก"
    *   ปุ่ม "ลบบุ๊คมาร์คทั้งหมด (ทุกเรื่อง)" (`Trash`): Modal ยืนยัน "คุณแน่ใจหรือไม่ว่าต้องการลบบุ๊คมาร์คทั้งหมด (ทุกเรื่อง)?"
        *   **Toast (สำเร็จ):** "ลบบุ๊คมาร์คทั้งหมด (ทุกเรื่อง) เรียบร้อยแล้ว"

### 7.4 แดชบอร์ดนักแปล (Translator Dashboard - `/translator/dashboard`)

*   **ความสามารถ:**
    *   รายการนิยายที่นักแปลจัดการ (การ์ด: ปก, ชื่อ, ความคืบหน้าการมาร์ค % + Progress Bar)
    *   ปุ่ม "จัดการบรรทัดมาร์ค" (ต่อเรื่อง, `ListChecks`) นำทางไปหน้า 7.4.1

#### 7.4.1 หน้าจัดการบรรทัดมาร์ค (Mark Line Management Page - `/translator/manage-marks/[novel_slug]`)

*   **หัวเรื่อง:** "จัดการบรรทัดมาร์คสำหรับ: [ชื่อนิยาย]"
*   **ส่วนควบคุมหลัก:** ปุ่ม "เลือกตอนเพื่อคัดลอก/ดาวน์โหลด" (`Pointer`) (Toggle โหมดเลือก, แสดง Checkbox และ Batch Actions Toolbar)
*   **สารบัญตอน:** Collapse/Accordion, ปรับจำนวนตอน, หัวกลุ่ม (ชื่อ, (โหมดเลือก) Checkbox "เลือกทั้งหมดในกลุ่ม")
*   **รายการตอน:** ลำดับ, ชื่อตอน, จำนวนมาร์ค, สถานะมาร์ค (`CheckCircle2`/`Circle`), ปุ่ม "คัดลอกมาร์คตอนนี้" (`Copy`), (โหมดเลือก) Checkbox
    *   **Toast (คัดลอกมาร์คตอนนี้):** "คัดลอกบรรทัดที่มาร์คของตอน '[ชื่อตอน]' ไปยังคลิปบอร์ดแล้ว"
*   **Batch Actions Toolbar (เมื่อเลือกตอน):**
    *   ปุ่ม "คัดลอกบรรทัดมาร์ค": **Toast:** "คัดลอกบรรทัดที่มาร์คของตอนที่เลือกไปยังคลิปบอร์ดแล้ว"
    *   ปุ่ม "ดาวน์โหลดไฟล์บรรทัดมาร์ค": เปิด Modal ตัวเลือก (รวม .txt / แยก .zip)
        *   **Toast (ดาวน์โหลด):** "กำลังเตรียมไฟล์ดาวน์โหลด..." และ "ดาวน์โหลดไฟล์บรรทัดมาร์คสำเร็จแล้ว"

### 7.5 หน้าอ่านนิยายและโหมดแปล (Reading & Translator Mode UI - `/novels/[novel_slug]/chapter/[chapter_number]`)

*   **การแสดงผลเนื้อหา:**
    *   ชื่อตอน: ชัดเจน กลางบน
    *   เนื้อหานิยาย: ย่อหน้าทุกย่อหน้า (CSS `text-indent`), PC: กว้างจำกัดกลางจอ, Mobile: เต็มจอ, ไม่มี Horizontal Scroll
    *   ปุ่ม "กลับไปบนสุด" (`ArrowUpCircle`): กึ่งโปร่งแสง ขวาล่าง เมื่อเลื่อนลง
    *   Keyboard Controls (PC): `ArrowLeft` (ก่อนหน้า), `ArrowRight` (ถัดไป), `Space` (เลื่อนลง)

*   **โหมดการอ่าน (Member/Translator ปิด "โหมดนักแปล"):**
    *   **Top Reading Toolbar (แสดงเสมอ):**
        *   PC: "สารบัญ" (`List`), "ปรับแต่ง (Aa)" (`Settings2` - Popover), "บุ๊คมาร์ค" (`Bookmark`), "สลับธีม" (`Sun`/`Moon`), "โหมดอ่านต่อเนื่อง" (`Infinity` - เปิดมี Animation สีรุ้ง), "< ตอนก่อนหน้า", "ตอนต่อไป >"
        *   Mobile: "สารบัญ", "ปรับแต่ง (Aa)", "บุ๊คมาร์ค", "สลับธีม", "โหมดอ่านต่อเนื่อง"
    *   **Toast (บุ๊คมาร์ค):** "เพิ่ม '[ชื่อตอน]' ในบุ๊คมาร์คแล้ว" / "ลบ '[ชื่อตอน]' ออกจากบุ๊คมาร์คแล้ว"
    *   **Toast (โหมดอ่านต่อเนื่อง):** "เปิดโหมดอ่านต่อเนื่องแล้ว" / "ปิดโหมดอ่านต่อเนื่องแล้ว"
    *   **Mobile Bottom Navigation Toolbar (เมื่อแตะเนื้อหา, ซ่อนเมื่อเลื่อน/แตะอีก):** "< ตอนก่อนหน้า", "ตอนต่อไป >"
    *   **ท้ายบท:**
        *   โหมดอ่านต่อเนื่องปิด: ปุ่มใหญ่ "ตอนต่อไป: [ชื่อตอนถัดไป]"
        *   โหมดอ่านต่อเนื่องเปิด: โหลดตอนถัดไปอัตโนมัติ (Infinite Scroll), ชื่อตอนด้านบนอัปเดต

*   **โหมดนักแปล (Translator เปิด "โหมดนักแปล" สำหรับนิยายนี้):**
    *   **Top Reading Toolbar:** เหมือนโหมดอ่าน + ปุ่ม "คัดลอกมาร์คทั้งหมด" (`CopyCheck`) (สำหรับตอนปัจจุบัน)
        *   **Toast:** "คัดลอกบรรทัดที่มาร์คทั้งหมดของตอนปัจจุบันไปยังคลิปบอร์ดแล้ว"
    *   **การแสดงผลเนื้อหา:**
        *   แถบเลขบรรทัด (ซ้าย, สีปรับตามธีม, ตัวเลขเด่นน้อยกว่าเนื้อหา)
        *   เนื้อหาต้นฉบับ (ถัดจากเลขบรรทัด, ย่อหน้า)
        *   Checkbox สำหรับมาร์ค (ขวาสุดของแต่ละบรรทัด)
    *   **MarkLine Interaction:**
        *   คลิก Checkbox: **Toast:** "มาร์คบรรทัดที่ X แล้ว" / "ยกเลิกการมาร์คบรรทัดที่ X"
        *   ดับเบิลคลิกบรรทัดเนื้อหา: ไฮไลท์, คัดลอกข้อความต้นฉบับ, มาร์คอัตโนมัติ
            *   **Toast:** "คัดลอกและมาร์คบรรทัดที่ X แล้ว"
    *   **ท้ายบท (โหมดนักแปล):**
        *   สรุปข้อมูลมาร์ค: "มีมาร์คทั้งหมด: [จำนวน] บรรทัด", "บรรทัดที่มาร์ค: [เลขที่ 1], [เลขที่ 2], ... (5 แรก, ปุ่ม 'ดูทั้งหมด' -> Modal)"
        *   ปุ่ม "ล้างมาร์คทั้งหมดของตอนนี้": Modal ยืนยัน "คุณแน่ใจหรือไม่ว่าต้องการล้างมาร์คทั้งหมดของตอนนี้?"
            *   **Toast:** "ล้างมาร์คทั้งหมดของตอนนี้แล้ว"
        *   ปุ่ม "คัดลอกมาร์คทั้งหมดของตอนนี้": **Toast:** "คัดลอกมาร์คทั้งหมดของตอนนี้ไปยังคลิปบอร์ดแล้ว"

### 7.6 แดชบอร์ดผู้ดูแลระบบ (Admin Dashboard - `/admin/dashboard`)

*   **Sidebar นำทาง (ซ้าย):** "ภาพรวม", "จัดการผู้ใช้", "จัดการหมวดหมู่", "จัดการประกาศ", "จัดการเว็บไซต์", "จัดการเวอร์ชัน"
*   **พื้นที่เนื้อหาหลัก (ขวา):** แสดงตามเมนู

#### 7.6.1 หน้าภาพรวมแดชบอร์ด (`/admin/dashboard` หรือ `/admin/dashboard/overview`)

*   หัวเรื่อง: "ภาพรวมแดชบอร์ดผู้ดูแลระบบ"
*   แสดง: จำนวนนิยายทั้งหมด, จำนวนผู้ใช้ทั้งหมด (การ์ด)

#### 7.6.2 หน้าจัดการผู้ใช้ (`/admin/dashboard/users`)

*   หัวเรื่อง: "จัดการผู้ใช้ทั้งหมด"
*   ควบคุม: ค้นหา (real-time, highlight), จำนวนผู้ใช้ (ทั้งหมด, ตามบทบาท)
*   ตาราง: #, Avatar, Email, ชื่อที่แสดง, สิทธิ์, วันที่สมัคร, วันที่สิทธิ์หมดอายุ, ปุ่ม "จัดการ" (`Settings`) (เรียงคอลัมน์ได้)
*   **Modal จัดการผู้ใช้:**
    *   UI: Avatar (+เปลี่ยน), Email (read-only), ชื่อที่แสดง (+แก้), สิทธิ์ (Dropdown +แก้), วันที่สมัคร (read-only), วันที่สิทธิ์หมดอายุ (Date Picker, ปุ่ม: +1/3/6 เดือน, ไม่มีวันหมดอายุ), ระงับบัญชี (Toggle)
    *   ปุ่ม: "บันทึกการเปลี่ยนแปลง", "ยกเลิก"
    *   **Toast (บันทึก):** "อัปเดตข้อมูลผู้ใช้ '[ชื่อผู้ใช้/อีเมล]' เรียบร้อยแล้ว"
    *   **Toast (เปลี่ยน Avatar):** "เปลี่ยนรูปโปรไฟล์ของผู้ใช้ '[ชื่อผู้ใช้/อีเมล]' เรียบร้อยแล้ว"

#### 7.6.3 หน้าจัดการหมวดหมู่นิยาย (`/admin/dashboard/categories`)

*   หัวเรื่อง: "จัดการหมวดหมู่นิยาย"
*   เพิ่ม: ปุ่ม "เพิ่มหมวดหมู่ใหม่" (`PlusCircle`)
    *   **Modal เพิ่มหมวดหมู่:** ชื่อ, Slug (auto-generate, แก้ไขได้), คำอธิบาย
    *   ปุ่ม: "บันทึก", "ยกเลิก"
    *   **Toast (บันทึก):** "เพิ่มหมวดหมู่ '[ชื่อหมวดหมู่]' สำเร็จแล้ว"
*   รายการหมวดหมู่ (Expandable Row/Accordion):
    *   แถวหลัก: ชื่อ, จำนวนนิยาย, ปุ่ม "แก้ไขชื่อ" (`Edit3`), ปุ่ม "ลบ" (`Trash2`), ปุ่ม "แสดง/ซ่อนนิยาย" (`ChevronDown`/`ChevronUp`)
    *   **Modal แก้ไขชื่อหมวดหมู่:** ชื่อ, Slug (auto-generate, แก้ไขได้), คำอธิบาย
        *   **Toast (บันทึก):** "แก้ไขหมวดหมู่ '[ชื่อหมวดหมู่เดิม]' เป็น '[ชื่อหมวดหมู่ใหม่]' สำเร็จแล้ว"
    *   **Modal ลบหมวดหมู่:** ยืนยัน "คุณแน่ใจหรือไม่ว่าต้องการลบหมวดหมู่ '[ชื่อหมวดหมู่]'?" + ตัวเลือกย้ายนิยายในหมวดนี้ (Dropdown เลือกหมวดอื่น / "ไม่มีหมวดหมู่")
        *   **Toast (ลบ):** "ลบหมวดหมู่ '[ชื่อหมวดหมู่]' และย้ายนิยาย (ถ้ามี) เรียบร้อยแล้ว"
    *   ส่วนขยาย: รายการนิยายในหมวด (ชื่อ + ลิงก์)

#### 7.6.4 หน้าจัดการประกาศ (`/admin/dashboard/announcements`)

*   หัวเรื่อง: "จัดการประกาศ"
*   ปุ่ม "สร้างประกาศใหม่" (`PlusCircle`)
*   ตาราง: หัวข้อ, สถานะ, หน้าที่แสดง, กลุ่มผู้รับ, วันที่เริ่ม/สิ้นสุด, ปุ่ม: แก้ไข (`Edit3`), ลบ (`Trash2`), สลับสถานะ (Toggle `ToggleLeft`/`ToggleRight`)
    *   **Toast (ลบ):** "ลบประกาศ '[หัวข้อประกาศ]' เรียบร้อยแล้ว"
    *   **Toast (สลับสถานะ):** "เปลี่ยนสถานะประกาศ '[หัวข้อประกาศ]' เป็น '[ใช้งาน/ไม่ใช้งาน]' แล้ว"
*   **Modal สร้าง/แก้ไขประกาศ:**
    *   ฟิลด์: หัวข้อ, เนื้อหา (Markdown Editor), วันที่เริ่ม/สิ้นสุด, หน้าที่แสดง (Checkbox: "หน้าแรก", "หน้านิยาย (ทุกเรื่อง)", "หน้านิยาย (เฉพาะเรื่อง)"), กลุ่มผู้รับ (Checkbox: "ทุกคน", "สมาชิก", "นักแปล", "ผู้ดูแลระบบ"), สถานะ "ใช้งาน" (Toggle)
    *   ปุ่ม: "บันทึก", "ยกเลิก"
    *   **Toast (บันทึก):** "บันทึกประกาศ '[หัวข้อประกาศ]' เรียบร้อยแล้ว"

#### 7.6.5 หน้าจัดการเว็บไซต์ (`/admin/dashboard/site-management`)

*   หัวเรื่อง: "จัดการเว็บไซต์"
*   Layout: Sections
    *   **ทั่วไป:** ชื่อเว็บไซต์, คำอธิบาย (SEO), โลโก้ (อัปโหลด, Preview), Favicon (อัปโหลด)
        *   **Toast (อัปโหลดโลโก้/Favicon):** "อัปโหลดโลโก้เว็บไซต์สำเร็จแล้ว" / "อัปโหลด Favicon สำเร็จแล้ว"
    *   **การลงทะเบียน/เข้าสู่ระบบ:** เปิด/ปิดลงทะเบียนใหม่ (Toggle), เปิด/ปิดเข้าสู่ระบบ Google (Toggle)
    *   **เนื้อหา:** จำนวนรายการต่อหน้าเริ่มต้น, การตั้งค่าเริ่มต้นนิยายใหม่ (สถานะตอน, สิทธิ์เข้าถึงตอน)
*   ปุ่ม: "บันทึกการตั้งค่า"
    *   **Toast:** "บันทึกการตั้งค่าเว็บไซต์เรียบร้อยแล้ว"

#### 7.6.6 หน้าจัดการเวอร์ชัน (`/admin/dashboard/versions`)

*   หัวเรื่อง: "จัดการเวอร์ชัน"
*   Layout: Tabs
    *   **Tab 1: เวอร์ชันเว็บไซต์ / Changelog**
        *   รายการ Changelog (เวอร์ชัน, วันที่, สรุป)
        *   ปุ่ม "เพิ่ม Changelog ใหม่" (`PlusCircle`)
            *   **Modal:** หมายเลขเวอร์ชัน, วันที่เผยแพร่, สรุป (Markdown), สถานะ (เผยแพร่/ร่าง)
            *   **Toast (บันทึก):** "บันทึก Changelog เวอร์ชัน '[หมายเลขเวอร์ชัน]' เรียบร้อยแล้ว"
        *   แต่ละ Changelog: ปุ่ม "แก้ไข" (`Edit3`), "ลบ" (`Trash2`)
            *   **Toast (ลบ):** "ลบ Changelog เวอร์ชัน '[หมายเลขเวอร์ชัน]' เรียบร้อยแล้ว"
    *   **Tab 2: เวอร์ชันนิยายและตอน**
        *   ค้นหานิยาย
        *   รายการนิยาย: ชื่อ, ปุ่ม "ดูประวัติเวอร์ชันนิยาย", "ดูประวัติเวอร์ชันตอน"
        *   **ประวัติเวอร์ชันนิยาย:** (จาก `novel_versions`) เวอร์ชัน, วันที่, ผู้แก้ไข, หมายเหตุ
            *   ปุ่ม "ดูรายละเอียด" (แสดง Snapshot), "กู้คืนเวอร์ชันนี้" (Modal ยืนยัน)
                *   **Toast (กู้คืน):** "กู้คืนข้อมูลนิยาย '[ชื่อนิยาย]' เป็นเวอร์ชัน [เลขเวอร์ชัน] สำเร็จแล้ว"
        *   **ประวัติเวอร์ชันตอน:** (เลือกตอนก่อน) (จาก `chapter_versions`) เวอร์ชัน, วันที่, ผู้แก้ไข, หมายเหตุ
            *   ปุ่ม "ดูเนื้อหา" (แสดง Snapshot), "กู้คืนเวอร์ชันนี้" (Modal ยืนยัน)
                *   **Toast (กู้คืน):** "กู้คืนเนื้อหาตอน '[ชื่อตอน]' เป็นเวอร์ชัน [เลขเวอร์ชัน] สำเร็จแล้ว"

### 7.7 หน้าโปรไฟล์ (Profile Page - `/profile`)

*   หัวเรื่อง: "โปรไฟล์ของฉัน"
*   **รูปโปรไฟล์ (Avatar):**
    *   แสดง Avatar, ปุ่ม "เปลี่ยนรูปโปรไฟล์" (เปิด File Picker, Crop 1:1)
    *   **Toast:** "เปลี่ยนรูปโปรไฟล์สำเร็จแล้ว"
*   **ชื่อที่แสดง (Display Name):**
    *   แสดงชื่อ, Input แก้ไข + ปุ่ม "บันทึกชื่อที่แสดง"
    *   **Toast:** "อัปเดตชื่อที่แสดงสำเร็จแล้ว"
*   **สิทธิ์ผู้ใช้งาน (Role):**
    *   แสดงบทบาท (Badge สี: สมาชิก-เขียว, นักแปล-ทอง, ผู้ดูแลระบบ-แดง) - Read-only
*   **อีเมล (Email):** แสดงอีเมล (Read-only)
*   **เปลี่ยนรหัสผ่าน:**
    *   ปุ่ม "เปลี่ยนรหัสผ่าน" (เปิด Modal)
    *   **Modal:** รหัสผ่านปัจจุบัน, รหัสผ่านใหม่, ยืนยันรหัสผ่านใหม่
    *   ปุ่ม "บันทึกรหัสผ่านใหม่", "ยกเลิก"
    *   **Toast:** "เปลี่ยนรหัสผ่านสำเร็จแล้ว กรุณาเข้าสู่ระบบใหม่อีกครั้ง" (บังคับล็อกอินใหม่)
*   **ออกจากระบบ:**
    *   ปุ่ม "ออกจากระบบ": Modal ยืนยัน "คุณต้องการออกจากระบบหรือไม่?"
    *   **Toast:** "ออกจากระบบเรียบร้อยแล้ว" (Redirect ไปหน้าแรก)

## 8. Key User Experience Flows & Enhancements

*   **การล็อกอิน (Modal):** สะดวก รวดเร็ว
*   **การนำทาง:** ชัดเจน เข้าใจง่าย
*   **ประสบการณ์การอ่าน:** ลื่นไหล ปรับแต่งได้ ไม่มีสะดุด
*   **เครื่องมือช่วยแปล:** ออกแบบมาเพื่อนักแปลโดยเฉพาะ
*   **การตอบสนองของระบบ:** Empty/Loading States, Error Handling ที่เป็นมิตร
*   **การออกแบบ:** สวยงาม (ทอง-เทา, Sarabun) ตอบสนองทุกอุปกรณ์
*   **ปุ่ม Arrow Up:** (`ArrowUpCircle`) ช่วยให้กลับไปบนสุดของหน้าได้ง่าย
*   **การแจ้งเตือนการกระทำ:** Toast notifications (มุมขวาล่าง) ชัดเจนทุกการกระทำ
*   **การสร้าง `mark_lines` อัตโนมัติ:** เมื่อสร้างตอนใหม่ ระบบจะสร้าง record ว่างๆ ใน `mark_lines` สำหรับนักแปลเจ้าของนิยาย

## 9. Tech Stack Summary

*   **Framework:** SvelteKit
*   **Styling:** TailwindCSS
*   **UI Library:** Skeleton UI (ปรับแต่ง Theme สีทอง-เทา)
*   **Form Handling & Validation:** Felte และ Zod
*   **Icon Library:** Lucide-Svelte
*   **Backend & Authentication:** Supabase Platform (PostgreSQL, Auth, Storage, Edge Functions (ถ้าจำเป็น))
*   **ภาษา:** TypeScript

## 10. การจัดการข้อผิดพลาดเบื้องต้น

*   **Client-Side Validation:** Zod + Felte แสดงข้อผิดพลาดใต้ฟิลด์
*   **Server-Side Validation:** Zod ใน SvelteKit actions/Supabase Functions
*   **User-Facing Error Messages:** Toast ที่เป็นมิตร (ภาษาไทย)
    *   ตัวอย่าง: "เกิดข้อผิดพลาด: ไม่สามารถบันทึกข้อมูลได้ กรุณาลองใหม่อีกครั้ง"
    *   ตัวอย่าง: "ไม่พบข้อมูลที่ท่านค้นหา"
*   **Code-Level Error Handling:** `try...catch`
*   **Logging:** `console.log/error` (Dev), Supabase Logging (Prod)

## 11. มาตรการความปลอดภัยเบื้องต้น

*   **Authentication:** Supabase Auth, Email Verification
*   **Authorization:** Row-Level Security (RLS) เข้มงวด (หัวใจสำคัญ)
*   **Input Validation:** Zod (Client & Server), Sanitize HTML output
*   **HTTPS:** ให้บริการผ่าน HTTPS
*   **Environment Variables:** เก็บ Keys ใน `.env.local` (ห้าม commit), ใช้ Service Role Key อย่างระมัดระวัง (เฉพาะ Server-side)
*   **Data Backups:** Supabase Auto-backup, พิจารณา Manual Backup
*   **Principle of Least Privilege:** สิทธิ์เท่าที่จำเป็น
*   **Software Updates:** หมั่นอัปเดต Dependencies

## 12. การตั้งค่าฐานข้อมูล Supabase และ Environment

### 12.1 Environment Variables

สร้างไฟล์ `.env` หรือ `.env.local` ใน root ของโปรเจกต์ SvelteKit (ไฟล์นี้ไม่ควรถูก commit เข้า Git):

```env
PUBLIC_BASE_URL="http://localhost:3000" # สำหรับ Local Dev, เปลี่ยนเป็น Domain จริงสำหรับ Production
PUBLIC_SUPABASE_URL="YOUR_SUPABASE_URL"
PUBLIC_SUPABASE_ANON_KEY="YOUR_SUPABASE_ANON_KEY"

# สำหรับ Server-side (เช่น SvelteKit actions หรือ Supabase Edge Functions)
SUPABASE_SERVICE_ROLE_KEY="YOUR_SUPABASE_SERVICE_ROLE_KEY"

12.2 ลำดับการตั้งค่าฐานข้อมูล Supabase (สำหรับ Local และ Production)

สร้าง Project ใน Supabase: ไปที่ app.supabase.com และสร้างโปรเจกต์ใหม่

ตั้งค่า Database:

ไปที่ "SQL Editor" ใน Dashboard ของ Supabase

คัดลอกและรันโค้ด SQL ทั้งหมดที่ให้ไว้ในส่วน "13. โค้ด SQL สำหรับสร้างตาราง (Supabase - PostgreSQL)" ด้านล่างนี้ทีละส่วน หรือทั้งหมดในครั้งเดียว

ตั้งค่า Storage Buckets:

ไปที่ "Storage" ใน Dashboard ของ Supabase

สร้าง Buckets ต่อไปนี้:

novel-covers

avatars

site-assets (สำหรับโลโก้, favicon)

chapter-text-files (สำหรับ Batch Upload .txt, อาจตั้งเป็น temporary)

กำหนด Policies สำหรับแต่ละ Bucket (สำคัญมาก!):

novel-covers:

SELECT: Public (ทุกคนสามารถดูรูปปกได้)

INSERT, UPDATE, DELETE: อนุญาตเฉพาะผู้ใช้ที่ยืนยันตัวตนแล้ว (authenticated) และเป็นเจ้าของนิยาย (เช็ค author_id ของนิยาย) หรือเป็น Admin

ตัวอย่าง Policy (ต้องปรับ get_my_claim('role') และ get_novel_author_id(bucket_id, name) ให้ถูกต้อง):

-- SELECT (Public)
(bucket_id = 'novel-covers') AND (SELECT true);

-- INSERT (Authenticated owner or Admin)
(bucket_id = 'novel-covers') AND (auth.role() = 'authenticated') AND
(
  (SELECT profiles.role_id FROM profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM roles WHERE name = 'Admin') OR
  (auth.uid() = (SELECT novels.author_id FROM novels WHERE novels.cover_image_url LIKE '%' || name)) -- ต้องปรับปรุง logic การหา novel_id จากชื่อไฟล์
);
-- UPDATE, DELETE คล้าย INSERT
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
SQL
IGNORE_WHEN_COPYING_END

avatars:

SELECT: Public

INSERT, UPDATE: อนุญาตเฉพาะผู้ใช้ที่ยืนยันตัวตนแล้วและเป็นเจ้าของโปรไฟล์นั้นๆ (เช็ค auth.uid())

ตัวอย่าง Policy (INSERT, ชื่อไฟล์ควรเป็น user_id/avatar.ext):

(bucket_id = 'avatars') AND (auth.role() = 'authenticated') AND (auth.uid() = (string_to_array(name, '/'::text))[1]::uuid);
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
SQL
IGNORE_WHEN_COPYING_END

site-assets:

SELECT: Public

INSERT, UPDATE, DELETE: อนุญาตเฉพาะ Admin

chapter-text-files:

SELECT: ไม่อนุญาต (ไฟล์ควรถูกประมวลผลและลบโดย server-side logic)

INSERT: อนุญาตเฉพาะนักแปล/Admin ที่กำลังอัปโหลด

DELETE: อนุญาตให้ Server-side logic (ใช้ Service Role Key) หรือผู้ที่อัปโหลดลบได้

หมายเหตุ: Bucket นี้ควรมีการจัดการไฟล์ที่เข้มงวด อาจมี Edge Function คอยลบไฟล์หลังประมวลผล

ตั้งค่า Authentication:

ไปที่ "Authentication" -> "Providers" และเปิดใช้งาน Email และ Google (ถ้าต้องการ)

ไปที่ "Authentication" -> "Settings" และตั้งค่า Site URL (สำหรับลิงก์ยืนยันอีเมล, etc.) ให้ตรงกับ PUBLIC_BASE_URL ของคุณ

สำหรับ Local Dev: http://localhost:3000

สำหรับ Production: https://inkrealm.co

(แนะนำ) ปรับแต่ง Email Templates ให้เป็นภาษาไทย

ตั้งค่า Row-Level Security (RLS):

ไปที่ "Authentication" -> "Policies"

สำหรับ ทุกตาราง ที่มีข้อมูลสำคัญ (เช่น novels, chapters, profiles, mark_lines) ให้ Enable RLS

สร้าง Policies สำหรับ SELECT, INSERT, UPDATE, DELETE ตามบทบาทและสิทธิ์ที่กำหนดไว้

นี่คือส่วนที่สำคัญที่สุดสำหรับความปลอดภัยของข้อมูล ตัวอย่างคร่าวๆ:

ตาราง novels:

SELECT: ทุกคนเห็นนิยายที่ visibility เป็น 'ทุกคน' หรือสมาชิกเห็น 'เฉพาะสมาชิก' หรือตรงตามบทบาท

INSERT: เฉพาะ 'Translator' หรือ 'Admin'

UPDATE, DELETE: เฉพาะ author_id ของนิยายนั้นๆ หรือ 'Admin'

ดูตัวอย่างการเขียน RLS Policy ในส่วน SQL ด้านล่าง

(Local Dev) รัน SvelteKit:

npm run dev (หรือ pnpm dev, yarn dev)

เปิดเบราว์เซอร์ไปที่ http://localhost:3000

13. โค้ด SQL สำหรับสร้างตาราง (Supabase - PostgreSQL)
-- ### EXTENSIONS ###
-- (Supabase อาจเปิดใช้งานบางส่วนให้อยู่แล้ว ตรวจสอบใน Dashboard -> Database -> Extensions)
CREATE EXTENSION IF NOT EXISTS "uuid-ossp"; -- สำหรับ uuid_generate_v4()
CREATE EXTENSION IF NOT EXISTS "moddatetime"; -- สำหรับ trigger อัปเดต updated_at

-- ### SEQUENCES ###
-- (ถ้าไม่ใช้ SERIAL แต่ต้องการ custom sequence)

-- ### TABLES ###

-- 1. ตารางบทบาทผู้ใช้ (Roles)
CREATE TABLE IF NOT EXISTS public.roles (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL UNIQUE,
    description TEXT
);
COMMENT ON TABLE public.roles IS 'ตารางเก็บประเภทบทบาทของผู้ใช้';
COMMENT ON COLUMN public.roles.name IS 'ชื่อบทบาท (Member, Translator, Admin)';

-- 2. ตารางโปรไฟล์ผู้ใช้ (Profiles)
CREATE TABLE IF NOT EXISTS public.profiles (
    id UUID PRIMARY KEY DEFAULT auth.uid() REFERENCES auth.users(id) ON DELETE CASCADE,
    email TEXT UNIQUE, -- อาจดึงจาก auth.users.email หรือเก็บแยกเพื่อความสะดวก
    display_name TEXT,
    avatar_url TEXT,
    role_id INTEGER NOT NULL REFERENCES public.roles(id) DEFAULT 1, -- Default to 'Member'
    role_expiry_date TIMESTAMP WITH TIME ZONE,
    is_suspended BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
COMMENT ON TABLE public.profiles IS 'ตารางเก็บข้อมูลโปรไฟล์เพิ่มเติมของผู้ใช้ เชื่อมกับ auth.users';
COMMENT ON COLUMN public.profiles.id IS 'UUID ของผู้ใช้, อ้างอิงจาก auth.users.id';
COMMENT ON COLUMN public.profiles.role_id IS 'FK อ้างอิงบทบาทจากตาราง roles';
COMMENT ON COLUMN public.profiles.role_expiry_date IS 'วันที่หมดอายุของบทบาท (ถ้ามี)';

-- 3. ตารางหมวดหมู่นิยาย (Categories)
CREATE TABLE IF NOT EXISTS public.categories (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL UNIQUE,
    slug TEXT NOT NULL UNIQUE,
    description TEXT
);
COMMENT ON TABLE public.categories IS 'ตารางเก็บหมวดหมู่ของนิยาย';

-- 4. ตารางนิยาย (Novels)
CREATE TABLE IF NOT EXISTS public.novels (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    title TEXT NOT NULL,
    slug TEXT NOT NULL UNIQUE,
    synopsis TEXT,
    cover_image_url TEXT, -- URL ไปยัง Supabase Storage
    category_id INTEGER REFERENCES public.categories(id) ON DELETE SET NULL,
    author_id UUID REFERENCES public.profiles(id) ON DELETE SET NULL, -- ผู้สร้าง/นักแปลหลัก
    status TEXT DEFAULT 'กำลังดำเนินการ' NOT NULL, -- เช่น 'กำลังดำเนินการ', 'จบแล้ว', 'พักการลง'
    visibility TEXT[] DEFAULT ARRAY['ทุกคน']::TEXT[], -- เช่น 'ทุกคน', 'เฉพาะสมาชิก', 'เฉพาะนักแปล'
    original_language CHAR(2) NOT NULL, -- 'zh', 'en', 'th'
    default_chapter_status TEXT DEFAULT 'เผยแพร่' NOT NULL, -- 'เผยแพร่', 'ฉบับร่าง' (สำหรับตอนใหม่ของนิยายนี้)
    default_chapter_access_level TEXT[] DEFAULT ARRAY['ทุกคน']::TEXT[], -- (สำหรับตอนใหม่ของนิยายนี้)
    word_count INTEGER DEFAULT 0, -- อาจจะรวมจากทุกตอน หรืออัปเดตเป็นระยะ
    total_chapters INTEGER DEFAULT 0, -- จำนวนตอนทั้งหมด
    last_published_at TIMESTAMP WITH TIME ZONE, -- วันที่ตอนล่าสุดเผยแพร่
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
COMMENT ON TABLE public.novels IS 'ตารางเก็บข้อมูลหลักของนิยายแต่ละเรื่อง';
COMMENT ON COLUMN public.novels.author_id IS 'ผู้สร้างนิยาย หรือนักแปลหลักที่ได้รับสิทธิ์จัดการ';
COMMENT ON COLUMN public.novels.visibility IS 'กำหนดว่าใครสามารถมองเห็นนิยายนี้ได้บ้าง';

-- 5. ตารางตอนของนิยาย (Chapters)
CREATE TABLE IF NOT EXISTS public.chapters (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    novel_id UUID NOT NULL REFERENCES public.novels(id) ON DELETE CASCADE,
    chapter_number INTEGER NOT NULL,
    title TEXT NOT NULL,
    -- slug TEXT, -- อาจจะไม่จำเป็นถ้าใช้ chapter_number ใน URL โดยตรง
    content_original TEXT, -- เนื้อหาต้นฉบับ (เช่น ภาษาจีน)
    content_display TEXT, -- เนื้อหาที่แสดงผล (เช่น ภาษาไทยที่แปลแล้ว หรือเนื้อหาที่จัดรูปแบบแล้ว)
    status TEXT DEFAULT 'เผยแพร่' NOT NULL, -- 'เผยแพร่', 'ฉบับร่าง'
    access_level TEXT[] DEFAULT ARRAY['ทุกคน']::TEXT[], -- 'ทุกคน', 'เฉพาะสมาชิก', 'เฉพาะนักแปล'
    word_count INTEGER DEFAULT 0,
    -- read_by_users JSONB, -- อาจจะซับซ้อนไป พิจารณาตารางแยกถ้าต้องการเก็บละเอียดว่าใครอ่านตอนไหนบ้าง
    created_by UUID REFERENCES public.profiles(id) ON DELETE SET NULL, -- ผู้สร้าง/แก้ไขตอนล่าสุด
    published_at TIMESTAMP WITH TIME ZONE, -- วันที่เผยแพร่ตอนนี้
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE (novel_id, chapter_number)
);
COMMENT ON TABLE public.chapters IS 'ตารางเก็บเนื้อหาของแต่ละตอนในนิยาย';
COMMENT ON COLUMN public.chapters.content_original IS 'เนื้อหาต้นฉบับก่อนการแปลหรือการแก้ไขหลัก';
COMMENT ON COLUMN public.chapters.content_display IS 'เนื้อหาที่จัดรูปแบบและพร้อมแสดงผลแก่ผู้อ่าน';

-- 6. ตารางบุ๊คมาร์คของผู้ใช้ (User Bookmarks)
CREATE TABLE IF NOT EXISTS public.user_bookmarks (
    id SERIAL PRIMARY KEY,
    user_id UUID NOT NULL REFERENCES public.profiles(id) ON DELETE CASCADE,
    novel_id UUID NOT NULL REFERENCES public.novels(id) ON DELETE CASCADE,
    chapter_id UUID NOT NULL REFERENCES public.chapters(id) ON DELETE CASCADE,
    bookmarked_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE (user_id, chapter_id) -- ผู้ใช้คนหนึ่งบุ๊คมาร์คตอนหนึ่งได้ครั้งเดียว
);
COMMENT ON TABLE public.user_bookmarks IS 'ตารางเก็บข้อมูลการบุ๊คมาร์คตอนนิยายของผู้ใช้';

-- 7. ตารางบรรทัดที่มาร์คโดยนักแปล (Mark Lines)
CREATE TABLE IF NOT EXISTS public.mark_lines (
    id BIGSERIAL PRIMARY KEY,
    chapter_id UUID NOT NULL REFERENCES public.chapters(id) ON DELETE CASCADE,
    translator_id UUID NOT NULL REFERENCES public.profiles(id) ON DELETE CASCADE,
    line_number INTEGER NOT NULL, -- เลขบรรทัดภายใน content_original ของตอนนั้นๆ
    original_text_segment TEXT, -- ส่วนของข้อความต้นฉบับที่ถูกมาร์ค (เพื่อความสะดวกในการ export)
    marked_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE (chapter_id, translator_id, line_number)
);
COMMENT ON TABLE public.mark_lines IS 'ตารางเก็บข้อมูลบรรทัดที่นักแปลมาร์คไว้ในแต่ละตอน';
COMMENT ON COLUMN public.mark_lines.line_number IS 'เลขบรรทัดอ้างอิงตามเนื้อหาต้นฉบับ (content_original) ของตอน';
COMMENT ON COLUMN public.mark_lines.original_text_segment IS 'ข้อความของบรรทัดต้นฉบับที่ถูกมาร์ค';

-- 8. ตารางประกาศ (Announcements)
CREATE TABLE IF NOT EXISTS public.announcements (
    id SERIAL PRIMARY KEY,
    title TEXT NOT NULL,
    content TEXT NOT NULL, -- รองรับ Markdown
    start_date TIMESTAMP WITH TIME ZONE,
    end_date TIMESTAMP WITH TIME ZONE,
    target_roles TEXT[] DEFAULT ARRAY['ทุกคน']::TEXT[], -- เช่น 'Member', 'Translator', 'Admin' (อ้างอิงจาก roles.name)
    target_pages TEXT[] DEFAULT ARRAY['หน้าแรก']::TEXT[], -- เช่น 'หน้าแรก', 'หน้านิยายทั้งหมด', 'หน้านิยายเฉพาะเรื่อง:[novel_id]'
    is_active BOOLEAN DEFAULT TRUE,
    created_by UUID REFERENCES public.profiles(id) ON DELETE SET NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
COMMENT ON TABLE public.announcements IS 'ตารางสำหรับประกาศจากผู้ดูแลระบบ';

-- 9. ตารางการตั้งค่าเว็บไซต์ (Site Settings)
CREATE TABLE IF NOT EXISTS public.site_settings (
    key TEXT PRIMARY KEY,
    value JSONB NOT NULL,
    description TEXT,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
COMMENT ON TABLE public.site_settings IS 'ตารางเก็บการตั้งค่าต่างๆ ของเว็บไซต์';
COMMENT ON COLUMN public.site_settings.key IS 'Key ของการตั้งค่า เช่น site_name, default_pagination, etc.';
COMMENT ON COLUMN public.site_settings.value IS 'Value ของการตั้งค่าในรูปแบบ JSONB';

-- 10. ตารางประวัติการเปลี่ยนแปลงเว็บไซต์ (Site Changelogs)
CREATE TABLE IF NOT EXISTS public.site_changelogs (
    id SERIAL PRIMARY KEY,
    version_number TEXT NOT NULL UNIQUE,
    release_date DATE NOT NULL,
    summary TEXT NOT NULL, -- รองรับ Markdown
    is_published BOOLEAN DEFAULT FALSE,
    created_by UUID REFERENCES public.profiles(id) ON DELETE SET NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
COMMENT ON TABLE public.site_changelogs IS 'ตารางเก็บ Changelog ของเว็บไซต์';

-- 11. ตารางเวอร์ชันของข้อมูลนิยาย (Novel Versions)
CREATE TABLE IF NOT EXISTS public.novel_versions (
    id SERIAL PRIMARY KEY,
    novel_id UUID NOT NULL REFERENCES public.novels(id) ON DELETE CASCADE,
    version_number INTEGER NOT NULL,
    metadata_snapshot JSONB NOT NULL, -- เก็บ Snapshot ของข้อมูลนิยาย (title, synopsis, category, etc.)
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    created_by UUID REFERENCES public.profiles(id) ON DELETE SET NULL, -- ผู้ที่ทำให้เกิดเวอร์ชันนี้
    notes TEXT, -- หมายเหตุเกี่ยวกับการเปลี่ยนแปลง
    UNIQUE(novel_id, version_number)
);
COMMENT ON TABLE public.novel_versions IS 'ตารางเก็บประวัติการเปลี่ยนแปลงข้อมูลเมตาของนิยาย';

-- 12. ตารางเวอร์ชันของเนื้อหาตอน (Chapter Versions)
CREATE TABLE IF NOT EXISTS public.chapter_versions (
    id SERIAL PRIMARY KEY,
    chapter_id UUID NOT NULL REFERENCES public.chapters(id) ON DELETE CASCADE,
    version_number INTEGER NOT NULL,
    content_original_snapshot TEXT,
    content_display_snapshot TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    created_by UUID REFERENCES public.profiles(id) ON DELETE SET NULL, -- ผู้ที่ทำให้เกิดเวอร์ชันนี้
    notes TEXT, -- หมายเหตุเกี่ยวกับการเปลี่ยนแปลง
    UNIQUE(chapter_id, version_number)
);
COMMENT ON TABLE public.chapter_versions IS 'ตารางเก็บประวัติการเปลี่ยนแปลงเนื้อหาของตอน';

-- ### FUNCTIONS & TRIGGERS ###

-- Function to update 'updated_at' column
CREATE OR REPLACE FUNCTION public.handle_updated_at()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Triggers for 'updated_at'
CREATE TRIGGER on_profiles_update
    BEFORE UPDATE ON public.profiles
    FOR EACH ROW EXECUTE PROCEDURE public.handle_updated_at();

CREATE TRIGGER on_novels_update
    BEFORE UPDATE ON public.novels
    FOR EACH ROW EXECUTE PROCEDURE public.handle_updated_at();

CREATE TRIGGER on_chapters_update
    BEFORE UPDATE ON public.chapters
    FOR EACH ROW EXECUTE PROCEDURE public.handle_updated_at();

CREATE TRIGGER on_announcements_update
    BEFORE UPDATE ON public.announcements
    FOR EACH ROW EXECUTE PROCEDURE public.handle_updated_at();

CREATE TRIGGER on_site_settings_update
    BEFORE UPDATE ON public.site_settings
    FOR EACH ROW EXECUTE PROCEDURE public.handle_updated_at();

-- Function to get user role name
CREATE OR REPLACE FUNCTION public.get_user_role(user_id UUID)
RETURNS TEXT AS $$
DECLARE
    role_name TEXT;
BEGIN
    SELECT r.name INTO role_name
    FROM public.profiles p
    JOIN public.roles r ON p.role_id = r.id
    WHERE p.id = user_id;
    RETURN role_name;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER; -- SECURITY DEFINER ถ้าจำเป็นต้องอ่าน roles โดยไม่สน RLS ของ roles เอง

-- ### INITIAL DATA ###
INSERT INTO public.roles (name, description) VALUES
    ('Member', 'สมาชิกทั่วไป สามารถอ่านนิยาย บุ๊คมาร์ค'),
    ('Translator', 'นักแปล สามารถสร้าง จัดการ และแปลนิยายของตนเองได้'),
    ('Admin', 'ผู้ดูแลระบบ มีสิทธิ์จัดการทุกอย่างในระบบ')
ON CONFLICT (name) DO NOTHING;

-- ตัวอย่างการตั้งค่าเริ่มต้นใน site_settings
INSERT INTO public.site_settings (key, value, description) VALUES
    ('site_name', '{"value": "INKREALM.CO"}', 'ชื่อเว็บไซต์หลัก'),
    ('site_description', '{"value": "แพลตฟอร์มนิยายแปลคุณภาพสำหรับทุกคน"}', 'คำอธิบายเว็บไซต์สำหรับ SEO'),
    ('allow_new_registration', '{"value": true}', 'เปิด/ปิดการลงทะเบียนผู้ใช้ใหม่'),
    ('allow_google_login', '{"value": true}', 'เปิด/ปิดการเข้าสู่ระบบด้วย Google'),
    ('default_items_per_page', '{"value": 20}', 'จำนวนรายการเริ่มต้นต่อหน้าสำหรับการแบ่งหน้า'),
    ('novel_default_chapter_status', '{"value": "เผยแพร่"}', 'สถานะเริ่มต้นของตอนใหม่สำหรับนิยายที่สร้างใหม่'),
    ('novel_default_chapter_access', '{"value": ["ทุกคน"]}', 'สิทธิ์การเข้าถึงเริ่มต้นของตอนใหม่สำหรับนิยายที่สร้างใหม่')
ON CONFLICT (key) DO UPDATE SET value = EXCLUDED.value, description = EXCLUDED.description;


-- ### ROW-LEVEL SECURITY (RLS) POLICIES - ตัวอย่าง ###
-- **สำคัญมาก: ต้องเปิดใช้งาน RLS สำหรับแต่ละตารางใน Supabase Dashboard ก่อน Policy เหล่านี้จึงจะมีผล**
-- **และ Policy ที่ให้นี้เป็นเพียงตัวอย่างเบื้องต้น ต้องปรับแก้ให้ละเอียดและครอบคลุมทุกกรณีตามความต้องการจริง**

-- **Profiles Table**
ALTER TABLE public.profiles ENABLE ROW LEVEL SECURITY;

CREATE POLICY "ผู้ใช้สามารถดูโปรไฟล์ตัวเองได้"
    ON public.profiles FOR SELECT
    USING (auth.uid() = id);

CREATE POLICY "ผู้ใช้สามารถอัปเดตโปรไฟล์ตัวเองได้"
    ON public.profiles FOR UPDATE
    USING (auth.uid() = id)
    WITH CHECK (auth.uid() = id);

CREATE POLICY "Admin สามารถจัดการโปรไฟล์ทั้งหมดได้"
    ON public.profiles FOR ALL -- SELECT, INSERT, UPDATE, DELETE
    USING (public.get_user_role(auth.uid()) = 'Admin')
    WITH CHECK (public.get_user_role(auth.uid()) = 'Admin');

-- **Novels Table**
ALTER TABLE public.novels ENABLE ROW LEVEL SECURITY;

CREATE POLICY "ทุกคนสามารถดูนิยายที่ตั้งค่าเป็น 'ทุกคน' หรือตามสิทธิ์"
    ON public.novels FOR SELECT
    USING (
        'ทุกคน' = ANY(visibility) OR
        (auth.role() = 'authenticated' AND 'เฉพาะสมาชิก' = ANY(visibility)) OR
        (auth.role() = 'authenticated' AND public.get_user_role(auth.uid()) = ANY(visibility)) OR -- กรณี 'เฉพาะนักแปล', 'เฉพาะผู้ดูแลระบบ'
        (public.get_user_role(auth.uid()) = 'Admin') -- Admin เห็นหมด
    );

CREATE POLICY "นักแปลและ Admin สามารถสร้างนิยายได้"
    ON public.novels FOR INSERT
    WITH CHECK (
        auth.role() = 'authenticated' AND
        (public.get_user_role(auth.uid()) IN ('Translator', 'Admin')) AND
        author_id = auth.uid() -- ผู้สร้างต้องเป็นตัวเอง
    );

CREATE POLICY "เจ้าของนิยายและ Admin สามารถแก้ไข/ลบนิยายได้"
    ON public.novels FOR UPDATE, DELETE
    USING (
        auth.role() = 'authenticated' AND
        (
            (author_id = auth.uid() AND public.get_user_role(auth.uid()) IN ('Translator', 'Admin')) OR
            (public.get_user_role(auth.uid()) = 'Admin')
        )
    );

-- **Chapters Table**
ALTER TABLE public.chapters ENABLE ROW LEVEL SECURITY;

CREATE POLICY "การเข้าถึงตอนขึ้นอยู่กับการตั้งค่าของตอนและนิยายแม่"
    ON public.chapters FOR SELECT
    USING (
        EXISTS (
            SELECT 1 FROM public.novels n
            WHERE n.id = chapters.novel_id AND (
                'ทุกคน' = ANY(n.visibility) OR
                (auth.role() = 'authenticated' AND 'เฉพาะสมาชิก' = ANY(n.visibility)) OR
                (auth.role() = 'authenticated' AND public.get_user_role(auth.uid()) = ANY(n.visibility)) OR
                (public.get_user_role(auth.uid()) = 'Admin')
            )
        ) AND (
            status = 'เผยแพร่' AND (
                'ทุกคน' = ANY(access_level) OR
                (auth.role() = 'authenticated' AND 'เฉพาะสมาชิก' = ANY(access_level)) OR
                (auth.role() = 'authenticated' AND public.get_user_role(auth.uid()) = ANY(access_level))
            ) OR
            -- เจ้าของนิยาย หรือ Admin สามารถเห็นตอนที่เป็นฉบับร่างได้
            (EXISTS (SELECT 1 FROM public.novels n WHERE n.id = chapters.novel_id AND n.author_id = auth.uid())) OR
            (public.get_user_role(auth.uid()) = 'Admin')
        )
    );


CREATE POLICY "เจ้าของนิยายและ Admin สามารถสร้าง/แก้ไข/ลบตอนได้"
    ON public.chapters FOR INSERT, UPDATE, DELETE
    USING (
        auth.role() = 'authenticated' AND
        EXISTS (
            SELECT 1 FROM public.novels n
            WHERE n.id = chapters.novel_id AND
            (
                (n.author_id = auth.uid() AND public.get_user_role(auth.uid()) IN ('Translator', 'Admin')) OR
                (public.get_user_role(auth.uid()) = 'Admin')
            )
        )
    )
    WITH CHECK ( -- สำหรับ INSERT และ UPDATE
        auth.role() = 'authenticated' AND
        EXISTS (
            SELECT 1 FROM public.novels n
            WHERE n.id = chapters.novel_id AND
            (
                (n.author_id = auth.uid() AND public.get_user_role(auth.uid()) IN ('Translator', 'Admin')) OR
                (public.get_user_role(auth.uid()) = 'Admin')
            )
        )
        -- สำหรับ INSERT, created_by ควรเป็นผู้ใช้ปัจจุบัน
        AND (TG_OP = 'DELETE' OR chapters.created_by ISNULL OR chapters.created_by = auth.uid())
    );


-- **Mark Lines Table**
ALTER TABLE public.mark_lines ENABLE ROW LEVEL SECURITY;

CREATE POLICY "นักแปลสามารถจัดการมาร์คของตัวเองในตอนที่เขามีสิทธิ์"
    ON public.mark_lines FOR ALL -- SELECT, INSERT, UPDATE, DELETE
    USING (
        auth.role() = 'authenticated' AND
        translator_id = auth.uid() AND
        EXISTS ( -- ตรวจสอบว่านักแปลมีสิทธิ์ในนิยายของตอนนี้
            SELECT 1 FROM public.chapters c
            JOIN public.novels n ON c.novel_id = n.id
            WHERE c.id = mark_lines.chapter_id AND n.author_id = auth.uid()
        )
    )
    WITH CHECK (
        auth.role() = 'authenticated' AND
        translator_id = auth.uid() AND
        EXISTS (
            SELECT 1 FROM public.chapters c
            JOIN public.novels n ON c.novel_id = n.id
            WHERE c.id = mark_lines.chapter_id AND n.author_id = auth.uid()
        )
    );

CREATE POLICY "Admin สามารถดูมาร์คทั้งหมดได้"
    ON public.mark_lines FOR SELECT
    USING (public.get_user_role(auth.uid()) = 'Admin');


-- **User Bookmarks Table**
ALTER TABLE public.user_bookmarks ENABLE ROW LEVEL SECURITY;

CREATE POLICY "ผู้ใช้สามารถจัดการบุ๊คมาร์คของตัวเองได้"
    ON public.user_bookmarks FOR ALL
    USING (auth.uid() = user_id)
    WITH CHECK (auth.uid() = user_id);

-- **หมายเหตุสำคัญเกี่ยวกับ RLS:**
-- 1. Policy ข้างต้นเป็นตัวอย่างเบื้องต้นและอาจจะต้องปรับปรุงให้ซับซ้อนขึ้นตาม use case จริง
-- 2. การใช้ `public.get_user_role(auth.uid())` บ่อยครั้งอาจมีผลต่อ performance พิจารณา denormalize role name ใน profiles table หรือ optimize function
-- 3. การตรวจสอบสิทธิ์ซ้อนกัน (เช่น เช็คสิทธิ์นิยายก่อนเช็คสิทธิ์ตอน) เป็นสิ่งสำคัญ
-- 4. ทดสอบ Policy ทั้งหมดอย่างละเอียด!

-- **การดูข้อมูล Mark Lines แบบเข้าใจง่าย (ตัวอย่าง Query):**
-- SELECT
--     ml.id AS mark_id,
--     p_translator.display_name AS translator_name,
--     p_translator.email AS translator_email,
--     n.title AS novel_title,
--     n.slug AS novel_slug,
--     c.chapter_number,
--     c.title AS chapter_title,
--     ml.line_number,
--     ml.original_text_segment,
--     ml.marked_at
-- FROM
--     public.mark_lines ml
-- JOIN
--     public.chapters c ON ml.chapter_id = c.id
-- JOIN
--     public.novels n ON c.novel_id = n.id
-- JOIN
--     public.profiles p_translator ON ml.translator_id = p_translator.id
-- WHERE
--     n.slug = 'your-novel-slug' -- (Optional) กรองตามนิยาย
-- ORDER BY
--     n.title, c.chapter_number, ml.line_number;
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
SQL
IGNORE_WHEN_COPYING_END
หวังว่าเอกสาร README.md และโค้ด SQL นี้จะครอบคลุมและละเอียดตามที่คุณต้องการนะครับ! หากมีส่วนไหนต้องการปรับแก้อีก แจ้งได้เลยครับ
IGNORE_WHEN_COPYING_START
content_copy
download
Use code with caution.
IGNORE_WHEN_COPYING_END
