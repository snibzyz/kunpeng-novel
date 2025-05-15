เยี่ยมเลยครับ! นี่คือไฟล์ README.md ที่ปรับปรุงใหม่ พร้อมโครงสร้าง SQL ที่สมบูรณ์สำหรับ INKREALM.CO V5.1 ครับ

```markdown
# INKREALM.CO: ข้อกำหนดระบบและคุณสมบัติหลัก (V5.1)

เอกสารนี้เป็นข้อกำหนดระบบและคุณสมบัติหลักสำหรับแพลตฟอร์ม INKREALM.CO (V5.1) โดยเน้นฟังก์ชันที่จำเป็นตามการปรับปรุงล่าสุด อ้างอิง Tech Stack (SvelteKit & Supabase Stack) ที่กำหนดไว้ และออกแบบมาเพื่อความสะดวกในการปรับใช้, ทดสอบ, และย้ายไปยังโดเมนต่างๆ

##  filosofia การออกแบบโดยรวม (Overall Design Philosophy)

*   **การแสดงผลหรูหราและอ่านง่าย**:
    *   **สีหลัก**: ทอง (Golden)
    *   **สีรอง**: เทา (Gray)
    *   **โหมดสว่าง (Light Mode)**: พื้นหลังขาว, ตัวอักษรเทาเข้ม/ดำ, องค์ประกอบเน้นสีทอง
    *   **โหมดมืด (Dark Mode)**: พื้นหลังเทาเข้ม/ดำ, ตัวอักษรเทาอ่อน/ขาว, องค์ประกอบเน้นสีทอง
    *   **ฟอนต์**: Sarabun (สำหรับเนื้อหาภาษาไทยและอังกฤษ)
    *   **โลโก้**: อาจใช้สี Gradient ทอง-เทา หรือสีทองเด่น
*   **เครื่องมือสำหรับนักแปล**: มีฟังก์ชันสนับสนุนการทำงานของนักแปลโดยเฉพาะ
*   **ใช้งานง่าย ตอบโจทย์ทุกอุปกรณ์**: UI ใช้งานง่าย (Intuitive), Responsive Design, หน้าอ่านไม่มี Horizontal Scroll
*   **ความยืดหยุ่นและการทดสอบ**: โครงสร้างที่เอื้อต่อการเปลี่ยนแปลงโดเมนและการทดสอบระบบที่ง่าย

## ภาพรวมระบบหลัก (Core System Overview)

*   **ระบบยืนยันตัวตนและจัดการผู้ใช้**: ลงทะเบียน, เข้าสู่ระบบ (อีเมล/รหัสผ่าน, Google), ลืมรหัสผ่าน, จัดการโปรไฟล์, บทบาทและสิทธิ์
*   **ระบบจัดการเนื้อหานิยาย (CMS)**: การสร้าง/แก้ไข/ลบนิยายและตอน (ผ่าน Modals), อัปโหลดเนื้อหา (ปก, .txt), จัดการหมวดหมู่นิยาย, ระบบจัดการเวอร์ชัน
*   **ระบบการอ่านและบุ๊คมาร์ค**: หน้าอ่านปรับแต่งได้, บุ๊คมาร์คตอน, โหมดอ่านต่อเนื่อง, สลับโหมดอ่านสำหรับนักแปล
*   **ระบบเครื่องมือสำหรับนักแปล**: โหมดแปล, ระบบบรรทัดมาร์ค, หน้าจัดการบรรทัดมาร์ค
*   **ระบบแดชบอร์ด**: สำหรับนักแปลและผู้ดูแลระบบ (จัดการผู้ใช้, หมวดหมู่, ประกาศ, เว็บไซต์, เวอร์ชัน)
*   **ระบบฐานข้อมูลและไฟล์**: Supabase (PostgreSQL, Storage)
*   **ระบบการแจ้งเตือนผู้ใช้**: Toast notifications (ภาษาไทย)
*   **ระบบประกาศ**: สำหรับ Admin สร้างและเผยแพร่ประกาศ

## โครงสร้าง URL และการจัดการโดเมน (User-Friendly URL Structure & Domain Management)

*   **หน้ารวมนิยาย (หน้าแรก)**: `/`
*   **หน้าข้อมูลนิยาย**: `/novels/[novel_slug]`
*   **หน้าอ่านตอนนิยาย**: `/novels/[novel_slug]/chapter/[chapter_number]`
*   **หน้าโปรไฟล์ผู้ใช้**: `/profile`
*   **หน้าบุ๊คมาร์ค**: `/bookmarks`
*   **แดชบอร์ดนักแปล**: `/translator/dashboard`
*   **หน้าจัดการบรรทัดมาร์ค (นักแปล)**: `/translator/manage-marks/[novel_slug]`
*   **แดชบอร์ดผู้ดูแลระบบ**: `/admin/dashboard`
    *   `/admin/dashboard/users`
    *   `/admin/dashboard/categories`
    *   `/admin/dashboard/announcements`
    *   `/admin/dashboard/site-settings`
    *   `/admin/dashboard/versions`

**การสร้าง Slug**:
*   **นิยาย**: สร้างอัตโนมัติจากชื่อเรื่อง (ตัวพิมพ์เล็ก, ขีดกลางแทนช่องว่าง, ตัดอักขระพิเศษ) แก้ไขได้พร้อมตรวจสอบความซ้ำซ้อน
*   **ตอน**: ใช้ `chapter_number`

**การจัดการโดเมนและ Base URL**:
*   ใช้ Environment Variables (`PUBLIC_BASE_URL`) สำหรับ Base URL
*   ลิงก์ภายในใช้ Relative Paths หรืออ้างอิง Base URL
*   Supabase URL และ Anon Key เก็บใน Environment Variables (`PUBLIC_SUPABASE_URL`, `PUBLIC_SUPABASE_ANON_KEY`)

## บทบาทผู้ใช้และสิทธิ์ (User Roles & Permissions)

ควบคุมโดย Supabase Row-Level Security (RLS)

| บทบาท         | สิทธิ์หลัก                                                                                                                                                                                                                            | แถบเครื่องมือ/คุณสมบัติพิเศษ                                                                                                                                                            |
| :------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **สมาชิก (Member)** | อ่านนิยาย, บุ๊คมาร์คตอน, แก้ไขโปรไฟล์ส่วนตัว                                                                                                                                                                                          | **แถบเครื่องมืออ่าน**: สารบัญ, ปรับแต่งการแสดงผล, บุ๊คมาร์ค, สลับธีม, โหมดอ่านต่อเนื่อง, นำทางตอน                                                                                             |
| **นักแปล (Translator)** | สิทธิ์ทั้งหมดของสมาชิก, เปิด/ปิด "โหมดนักแปล" สำหรับนิยายที่ตนมีสิทธิ์, เข้าถึงโหมดแปล, ใช้งานบรรทัดมาร์ค, สร้าง/จัดการนิยายและตอนของตนเอง (ผ่าน Modal), เข้าถึงแดชบอร์ดนักแปลและหน้าจัดการบรรทัดมาร์ค                                  | **แถบเครื่องมือพิเศษในโหมดแปล**: เหมือนสมาชิก เพิ่ม "คัดลอกมาร์คทั้งหมด (ตอนปัจจุบัน)"                                                                                                     |
| **ผู้ดูแลระบบ (Admin)** | สิทธิ์ทั้งหมดของนักแปล, เข้าถึงแดชบอร์ดผู้ดูแลระบบ, จัดการผู้ใช้, ดูข้อมูลมาร์คทั้งหมด, จัดการเว็บไซต์, จัดการหมวดหมู่นิยาย, จัดการประกาศ, จัดการเวอร์ชัน (เว็บไซต์, นิยาย, ตอน)                                                         | เข้าถึงทุกส่วนของ Admin Dashboard                                                                                                                                                          |

## การยืนยันตัวตน (Authentication)

*   **ความสามารถ**: ลงทะเบียน, เข้าสู่ระบบ, ลืมรหัสผ่าน
*   **การแสดงผล**: Modal เป็นหลัก, มีหน้าเฉพาะ (`/login`, `/register`, `/forgot-password`)
*   **เทคโนโลยี**: Supabase Auth, Felte + Zod, Skeleton UI
*   **การแจ้งเตือน**: Toast notification (ภาษาไทย) เช่น "เข้าสู่ระบบสำเร็จ", "ลงทะเบียนเรียบร้อย"

## แถบนำทาง (Navbar Structure)

Responsive และแสดงเมนูตามบทบาทผู้ใช้

*   **Guest**: โลโก้, ปุ่มสลับธีม, ปุ่ม "เข้าสู่ระบบ" (เปิด Login Modal)
*   **Member**: โลโก้, "หน้าแรก", "บุ๊คมาร์ค", ปุ่มสลับธีม, โปรไฟล์ (Dropdown: ชื่อ, สิทธิ์, ลิงก์ "หน้าโปรไฟล์", ออกจากระบบ)
*   **Translator**: เหมือน Member เพิ่ม "แดชบอร์ดนักแปล"
*   **Admin**: เหมือน Translator เพิ่ม "แดชบอร์ดผู้ดูแลระบบ"

## รายละเอียดหน้าเว็บไซต์, Modals, และการจัดการเนื้อหา

### 5.1 หน้าแรก (Home Page)

*   **ส่วนแสดงประกาศ**: หากมีประกาศที่ Active และตรงเป้าหมาย/สิทธิ์ผู้ใช้ (แบนเนอร์ด้านบน, มีปุ่มปิด)
*   **เครื่องมือหลัก**:
    *   ช่องค้นหานิยาย
    *   ฟิลเตอร์หมวดหมู่ (Dropdown จาก `categories` table)
    *   ฟิลเตอร์สถานะนิยาย (Dropdown: ต่อเนื่อง, จบแล้ว)
    *   ปุ่ม `[สร้างนิยาย]` (สำหรับนักแปล/Admin) -> เปิด Modal สร้างนิยาย (5.1.1)
*   **การแสดงรายการนิยาย**: Grid Layout, การ์ดนิยาย (ปก 3:4, ชื่อ, จำนวนตอน), Mobile 2 คอลัมน์
*   **ระบบแบ่งหน้า (Pagination)**
*   **สำหรับ Guest ครั้งแรก**: แสดง Login Modal อัตโนมัติ

#### 5.1.1 Modal สร้างนิยาย (Create Novel Modal)

*   **UI**: ชื่อเรื่อง, คำโปรย, หมวดหมู่ (Dropdown), รูปปก (อัปโหลด, Crop 3:4, ย่อขนาด ≤3MB), ภาษาต้นฉบับ (Dropdown: จีน, อังกฤษ, ไทย), สิทธิ์ในการมองเห็นนิยาย (Checkbox: ทุกคน, สมาชิก, นักแปล), การตั้งค่าเริ่มต้นสำหรับตอนใหม่ (สถานะ: เผยแพร่/ร่าง, สิทธิ์เข้าถึง: ทุกคน/สมาชิก/นักแปล)
*   **ปุ่ม**: "เผยแพร่นิยาย", "บันทึกเป็นฉบับร่าง", "ยกเลิก" (Toast ภาษาไทย)

### 5.2 หน้าข้อมูลนิยายและสารบัญ (Novel Detail Page)

*   **ส่วนแสดงประกาศ**: หากมีประกาศที่ Active และตรงเป้าหมาย/สิทธิ์ผู้ใช้
*   **ข้อมูลนิยาย**: ปก, ชื่อ, ผู้แต่ง, หมวดหมู่, คำโปรย, จำนวนตอน, สถานะ, ภาษาต้นฉบับ (`lang` attribute)
*   **(นักแปล/Admin ที่มีสิทธิ์)** ปุ่มสลับ "โหมดนักแปล" (Toggle Switch, บันทึกสถานะเฉพาะบุคคล/นิยาย)
*   **ปุ่มจัดการนิยาย (ผู้สร้าง/Admin)**:
    *   "แก้ไขข้อมูลนิยาย" -> Modal แก้ไขข้อมูลนิยาย (5.2.1)
    *   "ลบนิยาย" -> Modal ยืนยัน (Toast ภาษาไทย)
*   **ส่วนเพิ่มตอน (ผู้สร้าง/Admin)**:
    *   "เพิ่มตอนด้วย .txt" -> Modal เพิ่มตอนด้วย .txt (5.2.2)
    *   "เพิ่มตอน" -> Modal เพิ่ม/แก้ไขตอน (5.2.3)
*   **สารบัญ**: Collapse/Accordion, ปรับจำนวนตอนต่อกลุ่ม (10-100, default 50), กลุ่มแรกขยาย
    *   **รายการตอน (ทุกคน)**: ไอคอนบุ๊คมาร์ค, ชื่อตอน, สถานะการอ่าน
    *   **รายการตอน (นักแปล/Admin)**: ไอคอนสถานะตอน, ปุ่ม "แก้ไขตอน", "ลบตอน", "คัดลอกมาร์คทั้งหมด"

#### 5.2.1 Modal แก้ไขข้อมูลนิยาย (Edit Novel Modal)

*   **UI (เติมข้อมูลปัจจุบัน)**: ชื่อเรื่อง, คำโปรย, หมวดหมู่, รูปปก (Crop 3:4, ย่อขนาด ≤3MB), ภาษาต้นฉบับ, สถานะนิยาย (Radio: ต่อเนื่อง, จบแล้ว, ยุติการลง), สิทธิ์มองเห็นนิยาย (Checkbox), ค่าเริ่มต้นสำหรับตอนใหม่ (สถานะ, สิทธิ์เข้าถึง)
*   **ปุ่ม**: "บันทึกการเปลี่ยนแปลง" (Toast, สร้าง `novel_versions`), "ยกเลิก", "ลบนิยาย" (Modal ยืนยัน, Toast)

#### 5.2.2 Modal เพิ่มตอนด้วย .txt (Batch Upload Modal)

*   **ความสามารถ**: Drag & Drop, Max 500 ไฟล์, Sort ชื่อไฟล์, ชื่อไฟล์เป็นชื่อตอน, เชื่อม novel_id, จัดการไฟล์ซ้ำ (Overwrite/Skip), Progress Bar
*   **การแจ้งเตือน**: Toast สรุปผล (เพิ่ม X, เขียนทับ Y, ข้าม Z ตอน - ภาษาไทย)
*   **อัตโนมัติ**: สร้าง `mark_lines` ว่างๆ และ `chapter_versions` เริ่มต้น

#### 5.2.3 Modal เพิ่ม/แก้ไขตอน (Create/Edit Chapter Modal)

*   **UI**: ชื่อตอน, เนื้อหา (Rich Text Editor - TipTap, ฟอนต์ Sarabun, รองรับ copy/paste จาก Word), สถานะตอน (Radio: เผยแพร่/ร่าง), สิทธิ์เข้าถึงตอน (Checkbox) - ค่าเริ่มต้นจากระดับนิยายแต่แก้ไขได้
*   **ปุ่ม (เพิ่มใหม่)**: "เผยแพร่ตอน", "บันทึกเป็นฉบับร่าง"
*   **ปุ่ม (แก้ไข)**: "บันทึกการเปลี่ยนแปลง" (Toast, สร้าง `chapter_versions` หากเนื้อหาเปลี่ยน)
*   **(ทั้งสองกรณี)**: "ยกเลิก"
*   **อัตโนมัติ (สร้างใหม่)**: สร้าง `mark_lines` ว่างๆ และ `chapter_versions` เริ่มต้น
*   **การแจ้งเตือน**: Toast "บันทึกตอน '[ชื่อตอน]' เรียบร้อยแล้ว" (ภาษาไทย)

### 5.3 หน้าบุ๊คมาร์ค (Bookmarks Page)

*   แสดงนิยายที่บุ๊คมาร์ค (เรียงตามวันที่บุ๊คมาร์คล่าสุดของนิยาย)
*   **แต่ละนิยาย**: ปก, ชื่อ, ตอนล่าสุดที่บุ๊คมาร์ค, ปุ่ม "ไปยังบุ๊คมาร์กล่าสุด"
*   ปุ่ม "จัดการบุ๊คมาร์ค" (ต่อเรื่อง) -> Modal "จัดการบุ๊คมาร์คสำหรับเรื่อง: [ชื่อนิยาย]"
    *   **ภายใน Modal**: เรียงลำดับ, รายการบุ๊คมาร์ค (Checkbox, ชื่อตอน, วันที่), "ลบที่เลือก" (Toast), "ล้างบุ๊คมาร์คทั้งหมดของเรื่องนี้" (Modal ยืนยัน, Toast), "บันทึก/เสร็จสิ้น", "ยกเลิก"
*   ปุ่ม "ลบบุ๊คมาร์คทั้งหมด (ทุกเรื่อง)" (Modal ยืนยัน, Toast ภาษาไทย)

### 5.4 แดชบอร์ดนักแปล (Translator Dashboard)

*   แสดงนิยายที่นักแปลจัดการ (การ์ด: ปก, ชื่อ, ความคืบหน้าการมาร์ค % + Progress Bar)
*   ปุ่ม "จัดการบรรทัดมาร์ค" -> หน้าจัดการบรรทัดมาร์ค (5.4.1)

#### 5.4.1 หน้าจัดการบรรทัดมาร์ค (Mark Line Management Page)

*   **หัวเรื่อง**: "จัดการบรรทัดมาร์คสำหรับ: [ชื่อนิยาย]"
*   **ควบคุม**: ปุ่ม "เลือกตอนเพื่อคัดลอก/ดาวน์โหลด" (Toggle โหมดเลือก)
*   **สารบัญตอน**: Collapse/Accordion, ปรับจำนวนตอนต่อกลุ่ม, (โหมดเลือก) Checkbox "เลือกทั้งหมดในกลุ่ม"
*   **รายการตอน**: เลขลำดับ, ชื่อตอน, จำนวนมาร์ค, สถานะมาร์ค, ปุ่ม "คัดลอกมาร์คตอนนี้" (Toast), (โหมดเลือก) Checkbox
*   **Batch Actions Toolbar (เมื่อเลือกตอน)**: "คัดลอกบรรทัดมาร์ค" (Toast), "ดาวน์โหลดไฟล์บรรทัดมาร์ค" (Modal ตัวเลือก: รวม .txt / แยก .zip, Toast)

### 5.5 หน้าอ่านนิยายและโหมดแปล (Reading & Translator Mode UI)

*   **การแสดงผลเนื้อหา**:
    *   ชื่อตอน: แสดงชัดเจนด้านบน
    *   เนื้อหานิยาย: ย่อหน้าทุกย่อหน้า (CSS `text-indent`), PC: กว้างจำกัดกลางจอ, Mobile: เต็มความกว้าง, ไม่มี Horizontal Scroll
    *   ปุ่ม "Arrow Up" (กลับบนสุด): มุมขวาล่างเมื่อเลื่อนลง
    *   **ควบคุมด้วยคีย์บอร์ด (PC)**: ลูกศรซ้าย/ขวา (ตอนก่อน/ถัดไป), Space Bar (เลื่อนลง)
*   **โหมดการอ่าน (Member/Translator - เมื่อไม่เปิด "โหมดนักแปล")**:
    *   **แถบเครื่องมือบน (แสดงเสมอ)**:
        *   **PC**: "สารบัญ" (Modal), "ปรับแต่ง (Aa)" (Popover), "บุ๊คมาร์ค" (Toggle, Toast), "สลับธีม", "โหมดอ่านต่อเนื่อง" (Toggle, ไอคอนมี Animation สีรุ้งเมื่อเปิด, Toast), นำทางตอน "< ก่อนหน้า", "ต่อไป >"
        *   **Mobile**: "สารบัญ", "ปรับแต่ง (Aa)", "บุ๊คมาร์ค", "สลับธีม", "โหมดอ่านต่อเนื่อง"
    *   **การนำทางตอน (Mobile - แถบเครื่องมือล่าง)**: ปรากฏเมื่อแตะเนื้อหา, ซ่อนเมื่อเลื่อน/แตะอีกครั้ง. ปุ่ม "< ก่อนหน้า", "ต่อไป >"
    *   **เมื่อถึงท้ายบท**:
        *   **อ่านต่อเนื่องปิด**: ปุ่มใหญ่ "ตอนต่อไป: [ชื่อตอน]" (รีเฟรชหน้า)
        *   **อ่านต่อเนื่องเปิด**: โหลดตอนถัดไปอัตโนมัติ (Infinite Scroll), ชื่อตอนด้านบนอัปเดต
*   **โหมดนักแปล (เมื่อเปิด "โหมดนักแปล" สำหรับนิยายนี้)**:
    *   **แถบเครื่องมือบน (แสดงเสมอ)**: เหมือนโหมดอ่าน เพิ่มปุ่ม "คัดลอกมาร์คทั้งหมด" (Toast)
    *   **การแสดงผลเนื้อหา**:
        *   แถบเลขบรรทัด (ซ้าย): สีปรับตามธีม, ตัวเลขเด่นน้อยกว่าเนื้อหา
        *   เนื้อหาต้นฉบับ: ถัดจากเลขบรรทัด, ย่อหน้า
        *   Checkbox สำหรับมาร์ค: ขวาสุดของแต่ละบรรทัด
    *   **MarkLine Interaction**:
        *   คลิก Checkbox: มาร์ค/ยกเลิก (Toast "มาร์ค/ยกเลิกมาร์คบรรทัดที่ X")
        *   ดับเบิลคลิกบรรทัด: ไฮไลท์ชั่วครู่, คัดลอกข้อความต้นฉบับ (จาก `chapters.content_original`), มาร์คอัตโนมัติ (Toast "คัดลอกและมาร์คบรรทัดที่ X แล้ว")
    *   **เมื่อถึงท้ายบท (โหมดนักแปล)**:
        *   สรุปข้อมูลมาร์ค: "มีมาร์คทั้งหมด: X บรรทัด", "บรรทัดที่มาร์ค: 1, 2, 3, ..." (แสดง 5 แรก, ปุ่ม "ดูทั้งหมด" -> Modal)
        *   ปุ่ม "ล้างมาร์คทั้งหมดของตอนนี้" (Modal ยืนยัน, Toast)
        *   ปุ่ม "คัดลอกมาร์คทั้งหมดของตอนนี้" (Toast)

### 5.6 แดชบอร์ดผู้ดูแลระบบ (Admin Dashboard)

*   **โครงสร้าง**: Sidebar ซ้าย (ภาพรวม, จัดการผู้ใช้, หมวดหมู่, ประกาศ, เว็บไซต์, เวอร์ชัน), พื้นที่เนื้อหาหลัก
#### 5.6.1 ภาพรวมแดชบอร์ด
    * แสดงข้อมูลสรุป (จำนวนนิยาย, ผู้ใช้)
#### 5.6.2 จัดการผู้ใช้
    * ค้นหา, ตารางผู้ใช้ (Avatar, Email, ชื่อ, สิทธิ์, วันที่สมัคร, วันหมดอายุสิทธิ์, ปุ่ม "จัดการ")
    * Modal จัดการผู้ใช้: แก้ไข Avatar, ชื่อ, สิทธิ์, วันหมดอายุ (Picker, Easy Action: +1/3/6 เดือน, ไม่มีหมดอายุ), ระงับบัญชี (Toggle) (Toast)
#### 5.6.3 จัดการหมวดหมู่นิยาย
    * ปุ่ม "เพิ่มหมวดหมู่ใหม่" (Modal: ชื่อ, Slug, คำอธิบาย; Toast)
    * รายการหมวดหมู่ (Accordion): ชื่อ, จำนวนนิยาย, "แก้ไขชื่อ" (Modal, Toast), "ลบ" (Modal ยืนยัน+ย้ายนิยาย, Toast), "แสดง/ซ่อนนิยาย" (แสดงรายการนิยายในหมวด)
#### 5.6.4 จัดการประกาศ
    * ปุ่ม "สร้างประกาศใหม่" (เปิด Modal)
    * ตารางประกาศ (หัวข้อ, สถานะ, หน้าที่แสดง, กลุ่มผู้รับ, วันที่เริ่ม/สิ้นสุด, ปุ่ม: แก้ไข, ลบ, สลับสถานะ) (Toast)
    * Modal สร้าง/แก้ไขประกาศ: หัวข้อ, เนื้อหา (Markdown), วันที่เริ่ม/สิ้นสุด, หน้าที่แสดง (Checkbox), กลุ่มผู้รับ (Checkbox), สถานะ "ใช้งาน" (Toggle) (Toast)
#### 5.6.5 จัดการเว็บไซต์
    * UI: ชื่อเว็บ, คำอธิบาย (SEO), โลโก้ (อัปโหลด, Preview), Favicon (อัปโหลด), เปิด/ปิดลงทะเบียน, เปิด/ปิด Google Login, จำนวนรายการต่อหน้า, ค่าเริ่มต้นนิยายใหม่ (สถานะตอน, สิทธิ์เข้าถึงตอน)
    * ปุ่ม "บันทึกการตั้งค่า" (Toast "บันทึกการตั้งค่าเว็บไซต์แล้ว")
#### 5.6.6 จัดการเวอร์ชัน
    * **Tab 1: เวอร์ชันเว็บไซต์ / Changelog**:
        * รายการ Changelog (เวอร์ชัน, วันที่, สรุป)
        * ปุ่ม "เพิ่ม Changelog ใหม่" (Modal: เวอร์ชัน, วันที่, สรุป Markdown, สถานะ)
        * ปุ่ม "แก้ไข", "ลบ" (Modal ยืนยัน)
    * **Tab 2: เวอร์ชันนิยายและตอน**:
        * ค้นหานิยาย
        * รายการนิยาย: ชื่อ, "ดูประวัติเวอร์ชันนิยาย", "ดูประวัติเวอร์ชันตอน"
        * คลิก "ดูประวัติเวอร์ชันนิยาย": รายการเวอร์ชัน (จาก `novel_versions`), "ดูรายละเอียด", "กู้คืนเวอร์ชันนี้" (Modal ยืนยัน, Toast)
        * คลิก "ดูประวัติเวอร์ชันตอน": เลือกตอน, รายการเวอร์ชัน (จาก `chapter_versions`), "ดูเนื้อหา", "กู้คืนเวอร์ชันนี้" (Modal ยืนยัน, Toast)

### 5.7 หน้าโปรไฟล์ (Profile Page)

*   **หัวเรื่อง**: "โปรไฟล์ของฉัน" / "ตั้งค่าบัญชี"
*   **ความสามารถ**:
    *   รูปโปรไฟล์: แสดง, ปุ่ม "เปลี่ยนรูปโปรไฟล์" (File Picker, Crop 1:1, Toast "เปลี่ยนรูปโปรไฟล์สำเร็จแล้ว")
    *   ชื่อที่แสดง: แสดง, Input แก้ไข, ปุ่ม "บันทึกชื่อที่แสดง" (Toast "อัปเดตชื่อที่แสดงสำเร็จแล้ว")
    *   สิทธิ์ผู้ใช้งาน: แสดง (Read-only) พร้อม Badge สี:
        *   สมาชิก: เขียว
        *   นักแปล: เหลือง/ทอง
        *   ผู้ดูแลระบบ: แดง
    *   อีเมล: แสดง (Read-only)
    *   เปลี่ยนรหัสผ่าน: ปุ่ม "เปลี่ยนรหัสผ่าน" -> Modal (รหัสผ่านปัจจุบัน, ใหม่, ยืนยันใหม่; Toast "เปลี่ยนรหัสผ่านสำเร็จแล้ว", บังคับล็อกอินใหม่)
    *   ออกจากระบบ: ปุ่ม "ออกจากระบบ" (Modal ยืนยัน, Toast "ออกจากระบบแล้ว", Redirect ไปหน้าแรก)

## ประสบการณ์ผู้ใช้หลัก (Key User Experience Flows)

*   การล็อกอิน (Modal), การนำทางที่ง่าย, ประสบการณ์การอ่านที่ลื่นไหล, เครื่องมือช่วยแปลที่ตอบโจทย์, การตอบสนองของระบบ (Empty/Loading States, Error Handling), ปุ่ม Arrow Up
*   **การแจ้งเตือน**: Toast notifications (ภาษาไทย) มุมล่างขวา
*   **สร้าง mark_lines อัตโนมัติ**: เมื่อสร้างตอนใหม่ (Manual/Batch)

## Tech Stack

| ประเภท                      | เทคโนโลยี                                                                 |
| :-------------------------- | :------------------------------------------------------------------------ |
| **Framework**               | SvelteKit                                                                 |
| **Styling**                 | TailwindCSS                                                               |
| **UI Library**              | Skeleton UI                                                               |
| **Form Handling/Validation**| Felte และ Zod                                                             |
| **Icon Library**            | Lucide-Svelte                                                             |
| **Backend & Auth**          | Supabase (PostgreSQL, Auth, Storage, Edge Functions ถ้าจำเป็น)             |
| **ภาษา**                    | TypeScript                                                                |
| **Text Editor (Content)**   | TipTap (หรือเทียบเท่า)                                                    |
| **Image Cropping**          | JavaScript library (เช่น Cropper.js หรือ tương tự)                      |

## การจัดการข้อผิดพลาดและความปลอดภัย

*   **Client-Side Validation**: Zod + Felte, แสดงข้อผิดพลาดชัดเจน (ภาษาไทย)
*   **Server-Side Validation**: Zod ใน SvelteKit actions/load functions และ Edge Functions
*   **User-Facing Error Messages**: Toast หรือ UI ที่เป็นมิตร (ภาษาไทย), ไม่แสดง technical details
*   **Code-Level Error Handling**: `try...catch`
*   **Error Logging**: `console.log/error` (dev), Supabase logging
*   **Authentication**: Supabase Auth, Email Verification
*   **Authorization**: Row-Level Security (RLS) เข้มงวด โดยเฉพาะ `novels`, `chapters`
*   **Input Validation**: Zod (Client/Server), Sanitize HTML output
*   **HTTPS**: ให้บริการผ่าน HTTPS
*   **Environment Variables**: เก็บ Keys ใน `.env.local` (ห้าม commit), ใช้ Service Role Key อย่างระมัดระวัง (Server-side เท่านั้น)
*   **Data Backups**: Supabase auto-backups, พิจารณา Manual Backups
*   **Principle of Least Privilege**
*   **Software Updates**: หมั่นอัปเดต Dependencies

## โครงสร้างฐานข้อมูล (Supabase - PostgreSQL)

```sql
-- เปิดใช้งาน UUID extension หากยังไม่มี
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- ตารางบทบาทผู้ใช้
CREATE TABLE roles (
    id SERIAL PRIMARY KEY,
    name TEXT UNIQUE NOT NULL -- e.g., 'member', 'translator', 'admin'
);

-- ตารางโปรไฟล์ผู้ใช้ (เชื่อมกับ auth.users ของ Supabase)
CREATE TABLE profiles (
    id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
    display_name TEXT,
    avatar_url TEXT,
    role_id INTEGER NOT NULL REFERENCES roles(id) DEFAULT 1, -- default to 'member'
    role_expiry_date TIMESTAMP WITH TIME ZONE NULL,
    is_suspended BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
-- RLS: profiles สามารถ SELECT ได้โดยเจ้าของ ID, UPDATE ได้โดยเจ้าของ ID
-- RLS: Admin สามารถ SELECT และ UPDATE profiles ทั้งหมด

-- ตารางหมวดหมู่นิยาย
CREATE TABLE categories (
    id SERIAL PRIMARY KEY,
    name TEXT UNIQUE NOT NULL,
    slug TEXT UNIQUE NOT NULL,
    description TEXT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
-- RLS: ทุกคน SELECT ได้
-- RLS: Admin สามารถ INSERT, UPDATE, DELETE

-- ตารางนิยาย
CREATE TABLE novels (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    title TEXT NOT NULL,
    slug TEXT UNIQUE NOT NULL,
    synopsis TEXT,
    category_id INTEGER REFERENCES categories(id) ON DELETE SET NULL,
    cover_image_url TEXT,
    original_language VARCHAR(10) NOT NULL, -- e.g., 'zh', 'en', 'th'
    visibility JSONB DEFAULT '["ทุกคน"]'::jsonb, -- e.g., ["ทุกคน"], ["เฉพาะสมาชิก"], ["เฉพาะนักแปล"]
    status TEXT NOT NULL DEFAULT 'ร่าง', -- e.g., 'เผยแพร่', 'ร่าง', 'จบแล้ว', 'ยุติการลง'
    created_by UUID NOT NULL REFERENCES profiles(id),
    default_chapter_status TEXT NOT NULL DEFAULT 'เผยแพร่', -- 'เผยแพร่', 'ร่าง'
    default_chapter_access_level JSONB DEFAULT '["ทุกคน"]'::jsonb, -- ["ทุกคน"], ["เฉพาะสมาชิก"], ["เฉพาะนักแปล"]
    total_chapters INTEGER DEFAULT 0,
    last_updated_chapter_at TIMESTAMP WITH TIME ZONE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
-- RLS: ทุกคน SELECT ได้ตาม visibility
-- RLS: Translator/Admin ที่เป็น created_by สามารถ UPDATE, DELETE
-- RLS: Admin สามารถ UPDATE, DELETE ทุกเรื่อง

-- ตารางตอนของนิยาย
CREATE TABLE chapters (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    novel_id UUID NOT NULL REFERENCES novels(id) ON DELETE CASCADE,
    chapter_number INTEGER NOT NULL,
    title TEXT NOT NULL,
    -- slug อาจจะไม่จำเป็นถ้าใช้ chapter_number ใน URL แต่ถ้ามีก็ดี
    -- slug TEXT NOT NULL,
    content_original TEXT, -- เนื้อหาต้นฉบับ (สำหรับโหมดแปล)
    content_translated TEXT, -- เนื้อหาที่แปลแล้ว (ถ้ามีระบบแปลในตัว หรือสำหรับแสดงผล)
    status TEXT NOT NULL, -- e.g., 'เผยแพร่', 'ร่าง' (ค่าเริ่มต้นจาก novels.default_chapter_status)
    access_level JSONB NOT NULL, -- e.g., ["ทุกคน"], ["เฉพาะสมาชิก"], ["เฉพาะนักแปล"] (ค่าเริ่มต้นจาก novels.default_chapter_access_level)
    created_by UUID NOT NULL REFERENCES profiles(id), -- ผู้สร้าง/นักแปลที่อัปโหลดตอนนี้
    word_count INTEGER DEFAULT 0,
    -- read_by_users JSONB, -- อาจจะซับซ้อน, พิจารณาตารางแยกถ้าต้องการ track การอ่านละเอียด
    published_at TIMESTAMP WITH TIME ZONE NULL, -- วันที่เผยแพร่จริง
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE (novel_id, chapter_number)
    -- UNIQUE (novel_id, slug) -- ถ้าใช้ slug สำหรับตอน
);
-- RLS: ทุกคน SELECT ได้ตาม access_level และ novel visibility
-- RLS: Translator/Admin ที่เป็น created_by ของ novel หรือ chapter สามารถ UPDATE, DELETE
-- RLS: Admin สามารถ UPDATE, DELETE ทุกตอน

-- ตารางบุ๊คมาร์คของผู้ใช้
CREATE TABLE user_bookmarks (
    id SERIAL PRIMARY KEY,
    user_id UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
    chapter_id UUID NOT NULL REFERENCES chapters(id) ON DELETE CASCADE,
    novel_id UUID NOT NULL REFERENCES novels(id) ON DELETE CASCADE, -- เพื่อ query ง่ายขึ้น
    bookmarked_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE (user_id, chapter_id)
);
-- RLS: เจ้าของ user_id เท่านั้นที่ CUD ข้อมูลของตัวเอง, SELECT ได้

-- ตารางบรรทัดที่มาร์คโดยนักแปล
CREATE TABLE mark_lines (
    id SERIAL PRIMARY KEY,
    chapter_id UUID NOT NULL REFERENCES chapters(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE, -- นักแปลที่มาร์ค
    marked_lines JSONB, -- e.g., [1, 5, 10] บรรทัดที่ถูกมาร์ค
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE (chapter_id, user_id)
);
-- RLS: เจ้าของ user_id (นักแปล) สามารถ CUD ข้อมูลของตัวเองสำหรับ chapter ที่ตนมีสิทธิ์
-- RLS: Admin สามารถ SELECT ข้อมูลทั้งหมด

-- ตารางประกาศ
CREATE TABLE announcements (
    id SERIAL PRIMARY KEY,
    title TEXT NOT NULL,
    content TEXT NOT NULL, -- รองรับ Markdown
    start_date TIMESTAMP WITH TIME ZONE NOT NULL,
    end_date TIMESTAMP WITH TIME ZONE NOT NULL,
    target_roles JSONB, -- e.g., ["member", "translator"] หรือ null/empty for all roles
    target_pages JSONB, -- e.g., ["home", "novel_detail_all", "novel_detail_specific:[novel_id]"]
    is_active BOOLEAN DEFAULT TRUE,
    created_by UUID REFERENCES profiles(id),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
-- RLS: ทุกคน SELECT ได้ (แอปจะกรองตาม target_roles, target_pages, is_active, date range)
-- RLS: Admin สามารถ CUD

-- ตารางการตั้งค่าเว็บไซต์
CREATE TABLE site_settings (
    key TEXT PRIMARY KEY,
    value JSONB,
    description TEXT, -- คำอธิบายว่า setting นี้คืออะไร
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_by UUID REFERENCES profiles(id)
);
-- Example Keys: 'site_name', 'site_description', 'logo_url', 'favicon_url', 'allow_registration', 'allow_google_login', 'default_pagination_count', 'novel_default_chapter_status', 'novel_default_chapter_access'
-- RLS: Admin สามารถ CUD, ทุกคนอาจจะ SELECT ได้บาง key (หรือให้ app logic ดึงผ่าน function)

-- ตาราง Changelog ของเว็บไซต์
CREATE TABLE site_changelogs (
    id SERIAL PRIMARY KEY,
    version_number TEXT NOT NULL,
    release_date DATE NOT NULL,
    summary TEXT, -- รองรับ Markdown
    is_published BOOLEAN DEFAULT FALSE,
    created_by UUID REFERENCES profiles(id),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
-- RLS: ทุกคน SELECT ได้ถ้า is_published = true
-- RLS: Admin สามารถ CUD

-- ตารางเวอร์ชันของข้อมูลนิยาย (Metadata)
CREATE TABLE novel_versions (
    id SERIAL PRIMARY KEY,
    novel_id UUID NOT NULL REFERENCES novels(id) ON DELETE CASCADE,
    version_number INTEGER NOT NULL,
    metadata_snapshot JSONB NOT NULL, -- เก็บข้อมูล snapshot ของ novel (title, synopsis, category, etc.)
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    created_by UUID REFERENCES profiles(id),
    notes TEXT, -- หมายเหตุการเปลี่ยนแปลง
    UNIQUE (novel_id, version_number)
);
-- RLS: Admin สามารถ SELECT
-- RLS: การ INSERT จะทำผ่าน trigger หรือ app logic เมื่อ novel มีการเปลี่ยนแปลงสำคัญ

-- ตารางเวอร์ชันของเนื้อหาตอน
CREATE TABLE chapter_versions (
    id SERIAL PRIMARY KEY,
    chapter_id UUID NOT NULL REFERENCES chapters(id) ON DELETE CASCADE,
    version_number INTEGER NOT NULL,
    content_original_snapshot TEXT,
    content_translated_snapshot TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    created_by UUID REFERENCES profiles(id),
    notes TEXT, -- หมายเหตุการเปลี่ยนแปลง
    UNIQUE (chapter_id, version_number)
);
-- RLS: Admin สามารถ SELECT
-- RLS: การ INSERT จะทำผ่าน trigger หรือ app logic เมื่อ chapter content มีการเปลี่ยนแปลง

-- Indexes (ตัวอย่าง)
CREATE INDEX idx_novels_slug ON novels(slug);
CREATE INDEX idx_novels_created_by ON novels(created_by);
CREATE INDEX idx_chapters_novel_id_chapter_number ON chapters(novel_id, chapter_number);
CREATE INDEX idx_user_bookmarks_user_id_novel_id ON user_bookmarks(user_id, novel_id);
CREATE INDEX idx_mark_lines_chapter_id_user_id ON mark_lines(chapter_id, user_id);

-- ฟังก์ชันสำหรับอัปเดต updated_at (ถ้าต้องการ)
CREATE OR REPLACE FUNCTION trigger_set_timestamp()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Trigger (ตัวอย่างสำหรับตาราง profiles)
CREATE TRIGGER set_profiles_updated_at
BEFORE UPDATE ON profiles
FOR EACH ROW
EXECUTE FUNCTION trigger_set_timestamp();

-- ทำเช่นเดียวกันสำหรับตารางอื่นๆ ที่มี updated_at:
-- categories, novels, chapters, mark_lines, announcements, site_settings, site_changelogs

-- Initial Data (ตัวอย่างสำหรับ roles)
INSERT INTO roles (name) VALUES ('member'), ('translator'), ('admin')
ON CONFLICT (name) DO NOTHING;

-- Initial Data (ตัวอย่างสำหรับ site_settings)
INSERT INTO site_settings (key, value, description) VALUES
    ('site_name', '"INKREALM.CO"'::jsonb, 'ชื่อเว็บไซต์หลัก'),
    ('site_description', '"แพลตฟอร์มสำหรับนักอ่านและนักแปลนิยาย"'::jsonb, 'คำอธิบายเว็บไซต์สำหรับ SEO'),
    ('allow_registration', 'true'::jsonb, 'เปิด/ปิดการลงทะเบียนใหม่'),
    ('allow_google_login', 'true'::jsonb, 'เปิด/ปิดการเข้าสู่ระบบด้วย Google'),
    ('default_pagination_count', '20'::jsonb, 'จำนวนรายการต่อหน้าเริ่มต้น'),
    ('novel_default_chapter_status', '"เผยแพร่"'::jsonb, 'สถานะเริ่มต้นของตอนใหม่สำหรับนิยาย (เผยแพร่/ร่าง)'),
    ('novel_default_chapter_access', '["ทุกคน"]'::jsonb, 'สิทธิ์การเข้าถึงตอนเริ่มต้นของตอนใหม่สำหรับนิยาย')
ON CONFLICT (key) DO UPDATE SET
    value = EXCLUDED.value,
    description = EXCLUDED.description,
    updated_at = NOW();

```

## Supabase Storage Buckets

*   `novel-covers`: รูปปกนิยาย (public)
*   `chapter-text-files`: ไฟล์ .txt สำหรับ Batch Upload (private, temporary, ลบหลังประมวลผล)
*   `avatars`: รูปโปรไฟล์ผู้ใช้ (public, หรือ private แล้วใช้ signed URL)
*   `site-assets`: โลโก้, favicon (public)

---

**หมายเหตุ**:
*   การแจ้งเตือนผู้ใช้ทั้งหมด (Toast notifications, error messages ในฟอร์ม) จะเป็นภาษาไทย
*   โครงสร้าง SQL ข้างต้นเป็นแบบละเอียดและครอบคลุมตามข้อกำหนด
*   Row-Level Security (RLS) Policies จะต้องถูกกำหนดอย่างละเอียดสำหรับแต่ละตารางเพื่อให้มั่นใจในความปลอดภัยของข้อมูลตามบทบาทผู้ใช้
*   พิจารณาการสร้าง Indexes เพิ่มเติมตามความจำเป็นเพื่อประสิทธิภาพในการ Query ข้อมูล
*   สำหรับ `content_original` และ `content_translated` ในตาราง `chapters` อาจจะต้องพิจารณาการเก็บข้อมูล HTML ที่ผ่านการ Sanitize แล้ว

หวังว่านี่จะเป็นประโยชน์ในการพัฒนา INKREALM.CO นะครับ!
```
