โอเคครับ! จากการปรับแก้และข้อกำหนดเพิ่มเติมทั้งหมด ผมได้รวบรวมและปรับปรุงเป็นเอกสาร README.md ฉบับสมบูรณ์สำหรับ INKREALM.CO (V5.1) ตามที่คุณต้องการ โดยเน้นความละเอียด ความถูกต้อง และการนำไปใช้งานจริง

ผมได้ใส่ "Key Changes in this Revision" ไว้ด้านบนเพื่อให้เห็นภาพรวมของการเปลี่ยนแปลงจากเวอร์ชันก่อนหน้าได้ง่าย

```markdown
# INKREALM.CO: ข้อกำหนดระบบและคุณสมบัติหลัก (V5.1) - ฉบับปรับปรุง

เอกสารนี้เป็นข้อกำหนดระบบและคุณสมบัติหลักสำหรับแพลตฟอร์ม INKREALM.CO (V5.1) โดยเน้นฟังก์ชันที่จำเป็นตามการปรับปรุงล่าสุด โดยยังคงอ้างอิง Tech Stack (SvelteKit & Supabase Stack) ที่กำหนดไว้ และออกแบบมาเพื่อความสะดวกในการปรับใช้, ทดสอบ, และย้ายไปยังโดเมนต่างๆ

**Key Changes in this Revision (สรุปการเปลี่ยนแปลงสำคัญในฉบับนี้):**

*   **URL Structure:**
    *   User Profiles: `/users/[user_id]/profile` (Admin viewable).
    *   Novels: `/novel/[novel_id]` (ใช้ UUID ของนิยาย).
    *   Chapters: `/novel/[novel_id]/[padded_chapter_number]` (เช่น `/novel/uuid-abc/001`, `/novel/uuid-xyz/0201`). `chapter_number` จะถูกเก็บเป็น TEXT ใน database เพื่อรองรับ padding.
*   **Development Port:** เปลี่ยนเป็น `http://localhost:3000`.
*   **Novel Card Display:** แสดง: รูปปก (3:4), ชื่อเรื่อง, จำนวนตอนทั้งหมด.
*   **Create Novel Button:** ใช้ไอคอน + ข้อความ "สร้างนิยาย".
*   **Synopsis Field:** เป็น Rich Text Editor.
*   **Original Languages:** จำกัดเฉพาะ จีน (zh), อังกฤษ (en), ไทย (th).
*   **Slug Management:** Novel slug สามารถแก้ไขได้, Chapter slug ไม่ใช้ (ใช้ `padded_chapter_number` แทน).
*   **Novel Statuses:** จำกัดเฉพาะ "ต่อเนื่อง (Ongoing)", "จบแล้ว (Completed)", "ยุติการลง (Dropped)".
*   **Table of Contents (Translator/Admin):** เพิ่มปุ่ม "คัดลอกมาร์คทั้งหมด" ต่อตอน.
*   **Create/Edit Novel Modal:** เพิ่ม/ปรับปรุงฟิลด์สถานะนิยาย.
*   **Chapter Numbering:**
    *   `chapter_number` ใน database เป็น TEXT (รองรับ padding เช่น "001", "0025").
    *   Manual Add: Auto-increment (พร้อม padding) และสามารถแก้ไขได้.
    *   Batch .txt Upload:
        *   รองรับชื่อไฟล์รูปแบบ `[ชื่อตอน] [เลขตอนแบบมี padding]` (เช่น "บทนำ 001", "ตอนพิเศษ 0005").
        *   เลขตอน (พร้อม padding) จะถูกดึงมาใช้เป็น `chapter_number`.
        *   ระบบรองรับการเริ่มลงตอนที่ไม่ใช่ตอนที่ 1 (เช่น เริ่มที่ "0201").
*   **Download Marks ZIP:** ชื่อไฟล์ .txt ภายใน .zip จะตรงกับชื่อตอนที่แสดงบนเว็บ (หรือตามชื่อไฟล์ต้นฉบับที่อัปโหลด).
*   **Content:** เน้น `chapters.content_original` เท่านั้น ไม่มีการจัดการ `content_translated`.
*   **Database Pruning:** ตัดฟิลด์ที่ไม่จำเป็นออก (เช่น `novels.translator_id`, `novels.author_name`).
*   **`novels.created_by`:** เก็บเป็น UUID ของผู้ใช้, เวลาแสดงผลจะดึง `display_name` จาก `profiles`.

---

## 1. ปรัชญาการออกแบบโดยรวม

*   **การแสดงผลหรูหราและอ่านง่าย:**
    *   **โทนสีหลัก:** สีทอง
    *   **สีรอง:** สีเทา
    *   **โหมดสว่าง (Light Mode):** เน้นสีทอง, เทา, และขาว เพื่อความสบายตาและดูสะอาด
    *   **โหมดมืด (Dark Mode):** เน้นสีทอง, เทาเข้ม, และดำ เพื่อลดแสงสะท้อนและช่วยให้อ่านง่ายในที่มืด
    *   **โลโก้:** อาจพิจารณาใช้การไล่ระดับสี (gradient) ที่ผสมผสานสีทองและสีเทาเพื่อความโดดเด่นและทันสมัย
    *   **ฟอนต์หลัก:** Sarabun เพื่อการแสดงผลภาษาไทยที่สวยงามและอ่านง่าย
*   **เครื่องมือสำหรับนักแปล:** มีฟังก์ชันสนับสนุนการทำงานของนักแปลโดยเฉพาะ
*   **ใช้งานง่าย ตอบโจทย์ทุกอุปกรณ์:** UI ใช้งานง่าย (Intuitive), Responsive Design, หน้าอ่านไม่มี Horizontal Scroll
*   **ความยืดหยุ่นและการทดสอบ:** โครงสร้างที่เอื้อต่อการเปลี่ยนแปลงโดเมนและการทดสอบระบบที่ง่าย
*   **การเปลี่ยนธีม:** การสลับระหว่างโหมดสว่างและโหมดมืดจะต้องราบรื่น สีที่แสดงผลต้องไม่ผิดเพี้ยน และสามารถควบคุมผ่านองค์ประกอบ UI ที่ชัดเจน (เช่น ไอคอนพระอาทิตย์/พระจันทร์)

## 1.1 ภาพรวมระบบหลัก (Core System Overview)

*   **ระบบยืนยันตัวตนและจัดการผู้ใช้:** ลงทะเบียน, เข้าสู่ระบบ (อีเมล/รหัสผ่าน, Google), ลืมรหัสผ่าน, จัดการโปรไฟล์, บทบาทและสิทธิ์
*   **ระบบจัดการเนื้อหานิยาย (CMS):** การสร้าง/แก้ไข/ลบนิยายและตอน (ผ่าน Modals เป็นหลัก), อัปโหลดเนื้อหา (ปก, .txt), การจัดการหมวดหมู่นิยาย (สำหรับ Admin), ระบบจัดการเวอร์ชันสำหรับนิยายและตอน
*   **ระบบการอ่านและบุ๊คมาร์ค:** หน้าอ่านที่ปรับแต่งได้, ระบบบุ๊คมาร์คตอน, โหมดอ่านต่อเนื่อง, การสลับโหมดการอ่านสำหรับนักแปล
*   **ระบบเครื่องมือสำหรับนักแปล:** โหมดแปล (เมื่อเปิดใช้งาน), ระบบบรรทัดมาร์ค, หน้าจัดการบรรทัดมาร์ค
*   **ระบบแดชบอร์ด:** สำหรับนักแปลและผู้ดูแลระบบ (รวมถึงหน้าจัดการผู้ใช้, จัดการหมวดหมู่, จัดการประกาศ, หน้าจัดการเว็บไซต์, และ หน้าจัดการเวอร์ชัน)
*   **ระบบฐานข้อมูลและไฟล์:** Supabase (PostgreSQL, Storage)
*   **ระบบการแจ้งเตือนผู้ใช้ (User Feedback System):** การใช้ Toast notifications สำหรับการยืนยันการกระทำต่างๆ
*   **ระบบประกาศ (Announcement System):** สำหรับ Admin ในการสร้างและเผยแพร่ประกาศไปยังผู้ใช้

## 1.2 โครงสร้าง URL ที่เป็นมิตรและการจัดการโดเมน (User-Friendly URL Structure & Domain Management)

เพื่อให้ URL ของเว็บไซต์ INKREALM.CO อ่านง่าย, เป็นมิตรต่อผู้ใช้, ดีต่อ SEO, และง่ายต่อการย้ายโดเมน:

*   **โครงสร้าง URL หลัก:**
    *   หน้ารวมนิยาย (หน้าแรก): `/`
    *   หน้าข้อมูลนิยาย: `/novel/[novel_id]` (โดย `[novel_id]` คือ UUID ของนิยาย)
    *   หน้าอ่านตอนนิยาย: `/novel/[novel_id]/[padded_chapter_number]` (เช่น `/novel/a1b2c3d4-e5f6-7890-1234-567890abcdef/001`)
    *   หน้าโปรไฟล์ผู้ใช้: `/users/[user_id]/profile` (โดย `[user_id]` คือ UUID ของผู้ใช้, ทำให้ Admin สามารถดูโปรไฟล์ของผู้ใช้อื่นได้)
    *   หน้าบุ๊คมาร์ค (ของผู้ใช้ปัจจุบัน): `/bookmarks`
    *   แดชบอร์ดนักแปล: `/translator/dashboard`
    *   หน้าจัดการบรรทัดมาร์ค (นักแปล): `/translator/manage-marks/[novel_id]`
    *   แดชบอร์ดผู้ดูแลระบบ: `/admin/dashboard` (และหน้ารายละเอียดภายใน เช่น `/admin/dashboard/users`)

*   **การสร้าง Slug และ Identifier:**
    *   **Slug สำหรับนิยาย (`novels.slug`):** จะถูกสร้างขึ้นโดยอัตโนมัติเมื่อสร้างนิยายใหม่ โดยแปลงชื่อเรื่องเป็นตัวพิมพ์เล็ก, แทนที่ช่องว่างด้วยขีดกลาง (`-`), และตัดอักขระพิเศษที่ไม่ปลอดภัยสำหรับ URL ออก ผู้สร้าง/Admin สามารถแก้ไข Slug นี้ได้ (พร้อมการตรวจสอบความซ้ำซ้อน) **หมายเหตุ:** ถึงแม้จะมี `novel_slug` แต่ URL หลักจะใช้ `novel_id` (UUID) เพื่อความแน่นอนและ unique. `novel_slug` สามารถใช้สำหรับ internal linking หรือ SEO alternatives ได้หากต้องการ.
    *   **สำหรับตอนนิยาย:** จะใช้ `padded_chapter_number` (เช่น "001", "025", "1003") โดยตรงใน URL ซึ่งค่านี้จะถูกเก็บใน `chapters.chapter_number` เป็น TEXT

*   **การจัดการโดเมนและ Base URL:**
    *   **Environment Variables:** การตั้งค่า Base URL ของเว็บไซต์ (เช่น `https://inkrealm.co` หรือ `http://localhost:3000` สำหรับการพัฒนา) จะถูกกำหนดผ่าน Environment Variable (เช่น `PUBLIC_BASE_URL` ใน SvelteKit) เพื่อให้การย้ายไปยังโดเมนใหม่ทำได้ง่ายโดยการแก้ไขค่าใน Environment Variable เท่านั้น โดยไม่ต้องแก้ไขโค้ด
    *   **การสร้างลิงก์ภายใน (Internal Linking):** ทุกลิงก์ภายในเว็บไซต์จะถูกสร้างขึ้นโดยอ้างอิงจาก Base URL ที่กำหนดใน Environment Variable หรือใช้ Relative Paths เพื่อให้ทำงานได้อย่างถูกต้องไม่ว่าจะอยู่บนโดเมนใด
    *   **การตั้งค่า Supabase:** URL ของ Supabase และ Anon Key จะถูกเก็บไว้ใน Environment Variables (`PUBLIC_SUPABASE_URL`, `PUBLIC_SUPABASE_ANON_KEY`)

## 2. บทบาทผู้ใช้และสิทธิ์ (User Roles & Permissions)

ระบบแบ่งบทบาทผู้ใช้ออกเป็น: สมาชิก (Member), นักแปล (Translator), และ ผู้ดูแลระบบ (Admin) โดยแต่ละบทบาทมีสิทธิ์การเข้าถึงและจัดการข้อมูลแตกต่างกันอย่างชัดเจน (ควบคุมโดย Supabase Row-Level Security - RLS)

*   **สมาชิก (Member):**
    *   **ความสามารถหลัก:** อ่านนิยาย, บุ๊คมาร์คตอน, แก้ไขโปรไฟล์ส่วนตัว
    *   **แถบเครื่องมือสำหรับอ่าน:** สารบัญ, ปรับแต่งการแสดงผล, บุ๊คมาร์ค, สลับธีมเว็บ, เปิด/ปิดโหมดอ่านต่อเนื่อง, นำทางตอน
*   **นักแปล (Translator):**
    *   **ความสามารถหลัก:** สิทธิ์ทั้งหมดของสมาชิก, สามารถเปิด/ปิด "โหมดนักแปล" สำหรับนิยายที่ตนเองมีสิทธิ์แปล, เข้าถึงโหมดแปล, ใช้งานบรรทัดมาร์ค, สร้าง/จัดการนิยายและตอนของตนเอง, เข้าถึงแดชบอร์ดนักแปลและหน้าจัดการบรรทัดมาร์ค
    *   **แถบเครื่องมือพิเศษในโหมดแปล:** สารบัญ, ปรับแต่งการแสดงผล, บุ๊คมาร์ค, สลับธีมเว็บ, เปิด/ปิดโหมดอ่านต่อเนื่อง, คัดลอกมาร์คทั้งหมด (สำหรับตอนปัจจุบัน), นำทางตอน
*   **ผู้ดูแลระบบ (Admin):**
    *   **ความสามารถหลัก:** สิทธิ์ทั้งหมดของนักแปล, เข้าถึงแดชบอร์ดผู้ดูแลระบบ, จัดการผู้ใช้, ดูข้อมูลมาร์คทั้งหมด, เข้าถึงและแก้ไขหน้าจัดการเว็บไซต์, การจัดการหมวดหมู่นิยาย, การจัดการประกาศ, การจัดการเวอร์ชัน

## 3. การเข้าสู่ระบบ / ลงทะเบียน / ลืมรหัสผ่าน (Authentication Modals & Pages)

*   **ความสามารถ:** ลงทะเบียน, เข้าสู่ระบบ, ลืมรหัสผ่าน
*   **การแสดงผล:** แสดงผลผ่าน Modal เป็นหลัก เมื่อเรียกจากภายในเว็บ; มีหน้าเว็บเฉพาะ (`/login`, `/register`, `/forgot-password`) สำหรับการเข้าถึงโดยตรง
*   **การพัฒนา:** Supabase Auth, Felte + Zod, Skeleton UI
*   **การแจ้งเตือน:** แสดง Toast notification

## 4. โครงสร้างแถบนำทาง (Navbar Structure)

*   **ความสามารถ:** แสดงเมนูตามบทบาท (Dynamic Menu), Responsive
*   **ตัวอย่างเมนู:**
    *   **Guest:** โลโก้, ปุ่มสลับธีม, ปุ่ม "เข้าสู่ระบบ" (เปิด Login Modal)
    *   **Member:** โลโก้, "หน้าแรก", "บุ๊คมาร์ค", ปุ่มสลับธีม, โปรไฟล์ (Dropdown: แสดงชื่อ และสิทธิ์, ลิงก์ไปยัง `/users/[user_id]/profile` ของตนเอง, ออกจากระบบ)
    *   **Translator:** เหมือน Member เพิ่ม "แดชบอร์ดนักแปล"
    *   **Admin:** เหมือน Translator เพิ่ม "แดชบอร์ดผู้ดูแลระบบ"
*   **การพัฒนา:** SvelteKit, Skeleton UI, TailwindCSS

## 5. รายละเอียดหน้าเว็บไซต์, Modals, และการจัดการเนื้อหา

### 5.1 หน้าแรก (Home Page - `/`)

*   **ส่วนแสดงประกาศ (Announcements Display Area):** แสดงประกาศที่ Active และตรงตามเงื่อนไข
*   **ส่วนเครื่องมือหลัก:**
    *   ช่องค้นหานิยาย
    *   ฟิลเตอร์หมวดหมู่
    *   ฟิลเตอร์สถานะนิยาย
    *   **ปุ่มสร้างนิยาย (Create Novel Button):** (สำหรับนักแปล/Admin) ปุ่มที่มี [ไอคอน Lucide-Svelte เช่น `PlusSquare` หรือ `FilePlus2`] + ข้อความ "สร้างนิยาย", เมื่อคลิก เปิด Modal สร้างนิยาย (ดูข้อ 5.1.1)
*   **การแสดงรายการนิยาย:** Grid Layout, **การ์ดนิยายแสดง: รูปปก (อัตราส่วน 3:4), ชื่อเรื่อง, จำนวนตอนทั้งหมด**. Mobile 1-2 คอลัมน์.
*   **ระบบแบ่งหน้า (Pagination)**
*   **การพัฒนา:** Supabase (query), Skeleton UI

### 5.1.1 Modal สร้างนิยาย (Create Novel Modal)

*   **การเรียกใช้งาน:** คลิกปุ่ม "[ไอคอน] สร้างนิยาย" บนหน้าแรก
*   **UI ภายใน Modal:**
    *   หัวเรื่อง Modal: "สร้างนิยายใหม่"
    *   **ฟอร์ม:**
        *   ชื่อเรื่อง (Title): Input text, จำเป็น
        *   Slug นิยาย (`novel_slug`): Input text, auto-generate จากชื่อเรื่อง, ผู้ใช้สามารถแก้ไขได้ (ตรวจสอบความซ้ำซ้อน)
        *   คำโปรย (Synopsis): **Rich Text Editor** (เช่น TipTap)
        *   หมวดหมู่ (Category): Dropdown
        *   รูปปกนิยาย (Cover Image): อัปโหลด (Crop 3:4, ย่อขนาดไม่เกิน 3MB)
        *   ภาษาต้นฉบับ (Original Language): Dropdown เลือก ("จีน (zh)", "อังกฤษ (en)", "ไทย (th)")
        *   **สถานะนิยาย (Novel Status):** Radio Group ("ต่อเนื่อง (Ongoing)" (ค่าเริ่มต้น), "จบแล้ว (Completed)", "ยุติการลง (Dropped)")
        *   สิทธิ์ในการมองเห็นนิยาย (Novel Visibility): Checkbox ("ทุกคน", "เฉพาะสมาชิก", "เฉพาะนักแปล")
        *   การตั้งค่าเริ่มต้นสำหรับตอนใหม่ของนิยายนี้:
            *   สถานะเริ่มต้นของตอน: Radio Group ("เผยแพร่", "ฉบับร่าง")
            *   สิทธิ์การเข้าถึงตอนเริ่มต้น: Checkbox ("ทุกคน", "เฉพาะสมาชิก", "เฉพาะนักแปล")
    *   **ปุ่มดำเนินการ:** "เผยแพร่นิยาย", "บันทึกเป็นฉบับร่าง", "ยกเลิก"
*   **การพัฒนา:** Skeleton UI Modal, Felte + Zod, Supabase, Rich Text Editor library, Image Cropper library

### 5.2 หน้าข้อมูลนิยายและสารบัญ (Novel Detail Page - `/novel/[novel_id]`)

*   **ส่วนแสดงประกาศ:** แสดงตามเงื่อนไข
*   **ส่วนข้อมูลนิยาย:** รูปปก, ชื่อเรื่อง, ผู้สร้าง (แสดง `display_name`), หมวดหมู่, คำโปรย (แสดงผล Rich Text), จำนวนตอนทั้งหมด, สถานะนิยาย, ภาษาต้นฉบับ.
*   **(สำหรับนักแปล/Admin ที่มีสิทธิ์) ปุ่มสลับ "โหมดนักแปล"**
*   **ปุ่มจัดการนิยาย (สำหรับผู้สร้าง/Admin):**
    *   "แก้ไขข้อมูลนิยาย": เปิด Modal แก้ไขข้อมูลนิยาย (ดูข้อ 5.2.1)
    *   "ลบนิยาย": Modal ยืนยัน
*   **ส่วนเพิ่มตอน (สำหรับผู้สร้าง/Admin):**
    *   ปุ่ม: "เพิ่มตอนด้วย .txt" (เปิด Modal 5.2.2), "เพิ่มตอน" (เปิด Modal 5.2.3)
*   **ส่วนสารบัญ:**
    *   Collapse/Accordion, ปรับจำนวนตอนต่อกลุ่ม
    *   **รายการตอน:**
        *   (ทุกคน): ไอคอนบุ๊คมาร์ค, **เลขตอน (แบบมี padding)**, ชื่อตอน, สถานะการอ่าน
        *   (นักแปล/Admin): ไอคอนสถานะตอน, ปุ่ม "แก้ไขตอน", ปุ่ม "ลบตอน", **ปุ่ม "คัดลอกมาร์คทั้งหมด" (สำหรับตอนปัจจุบัน, หากนักแปลเป็นเจ้าของและมีมาร์ค)**
*   **การพัฒนา:** Skeleton UI, Supabase, Svelte logic

### 5.2.1 Modal แก้ไขข้อมูลนิยาย (Edit Novel Modal)

*   **การเรียกใช้งาน:** คลิก "แก้ไขข้อมูลนิยาย"
*   **UI ภายใน Modal:** ฟอร์มเติมด้วยข้อมูลปัจจุบัน, ฟิลด์เหมือน Modal สร้างนิยาย (ข้อ 5.1.1) แต่สำหรับแก้ไข รวมถึง:
    *   ชื่อเรื่อง, Slug นิยาย (แก้ไขได้, ตรวจสอบความซ้ำซ้อน), คำโปรย (Rich Text Editor), หมวดหมู่, รูปปก, ภาษาต้นฉบับ, **สถานะนิยาย**, สิทธิ์ในการมองเห็น, ค่าเริ่มต้นสำหรับตอนใหม่
*   **ปุ่มดำเนินการ:** "บันทึกการเปลี่ยนแปลง", "ยกเลิก", (อาจมี) "ลบนิยาย"
*   **การพัฒนา:** เหมือน 5.1.1 แต่เป็น action update

### 5.2.2 Modal เพิ่มตอนด้วย .txt (Batch Upload Modal)

*   **การเรียกใช้งาน:** คลิก "เพิ่มตอนด้วย .txt"
*   **UI ภายใน Modal:**
    *   หัวเรื่อง, Drag & Drop/เลือกไฟล์ (.txt, Max 500 ไฟล์)
    *   แสดงรายการไฟล์, **ชื่อไฟล์ควรเป็นรูปแบบ `[ชื่อตอน] [เลขตอนแบบมี padding]` เช่น "บทที่หนึ่ง 001", "การผจญภัยครั้งใหม่ 0002".**
    *   ตัวเลือกจัดการชื่อไฟล์ซ้ำ (Overwrite/Skip)
    *   Progress Bar
*   **การทำงานเบื้องหลัง:**
    *   Server ประมวลผลไฟล์:
        *   แยก "ชื่อตอน" และ "เลขตอนแบบมี padding" (เช่น "001") จากชื่อไฟล์
        *   ใช้ "เลขตอนแบบมี padding" นี้เป็น `chapters.chapter_number` (TEXT)
        *   ระบบรองรับการเริ่มลงตอนที่ไม่ใช่ "001" (เช่น เริ่มที่ "0201")
        *   สร้าง `mark_lines` และ `chapter_versions` อัตโนมัติ
*   **การแจ้งเตือน (Toast) เมื่อสำเร็จ:** สรุปผลละเอียด
*   **การพัฒนา:** Skeleton UI, Supabase, Logic การ parsing ชื่อไฟล์

### 5.2.3 Modal เพิ่ม/แก้ไขตอน (Create/Edit Chapter Modal)

*   **การเรียกใช้งาน:** "เพิ่มตอน" หรือ "แก้ไขตอน"
*   **UI ภายใน Modal:**
    *   หัวเรื่อง
    *   **ฟอร์ม:**
        *   ชื่อตอน (Chapter Title): Input text, จำเป็น
        *   **หมายเลขตอน (Chapter Number - `padded_chapter_number`):**
            *   **เพิ่มตอนใหม่:** Input text, **auto-increment จากตอนล่าสุด +1 พร้อม padding อัตโนมัติ** (เช่น ถ้าล่าสุดคือ "009" ตอนใหม่จะเป็น "010"). ผู้ใช้สามารถแก้ไขเลขตอนนี้ได้ (ต้อง validate รูปแบบ padding และความซ้ำซ้อน).
            *   **แก้ไขตอน:** แสดงเป็น Input text ที่แก้ไขได้ (validate รูปแบบ padding และความซ้ำซ้อน).
        *   เนื้อหา (Content - `content_original`): Rich Text Editor
        *   สถานะตอน: Radio Group ("เผยแพร่", "ฉบับร่าง")
        *   สิทธิ์การเข้าถึงตอน: Checkbox ("ทุกคน", "เฉพาะสมาชิก", "เฉพาะนักแปล")
    *   **ปุ่มดำเนินการ:** (เพิ่ม) "เผยแพร่ตอน", "บันทึกเป็นฉบับร่าง"; (แก้ไข) "บันทึกการเปลี่ยนแปลง"; "ยกเลิก"
*   **การสร้าง `mark_lines` และ `chapter_versions` อัตโนมัติ (เฉพาะตอนสร้างใหม่)**
*   **การพัฒนา:** Skeleton UI Modal, Felte + Zod, Supabase, TipTap

### 5.3 หน้าบุ๊คมาร์ค (Bookmarks Page - `/bookmarks`)

*   **ความสามารถ:** แสดงรายการนิยายที่บุ๊คมาร์ค, ปุ่ม "ไปยังบุ๊คมาร์คล่าสุด", Modal "จัดการบุ๊คมาร์คสำหรับเรื่อง" (ลบที่เลือก, ล้างทั้งหมดของเรื่อง), ปุ่ม "ลบบุ๊คมาร์คทั้งหมด (ทุกเรื่อง)"
*   **การพัฒนา:** Supabase, Skeleton UI, Lucide-Svelte

### 5.4 แดชบอร์ดนักแปล (Translator Dashboard - `/translator/dashboard`)

*   **ความสามารถ:** แสดงรายการนิยายที่นักแปลจัดการ (การ์ด: ปก, ชื่อ, ความคืบหน้ามาร์ค %), ปุ่ม "จัดการบรรทัดมาร์ค"
*   **การพัฒนา:** Supabase, Skeleton UI

### 5.4.1 หน้าจัดการบรรทัดมาร์ค (Mark Line Management Page - `/translator/manage-marks/[novel_id]`)

*   หัวเรื่อง: "จัดการบรรทัดมาร์คสำหรับ: [ชื่อนิยาย]"
*   ส่วนควบคุม: ปุ่ม "เลือกตอนเพื่อคัดลอก/ดาวน์โหลด"
*   สารบัญตอน: Collapse/Accordion, **เลขตอน (แบบมี padding)**, ชื่อตอน, จำนวนมาร์ค, สถานะมาร์ค, ปุ่ม "คัดลอกมาร์คตอนนี้", Checkbox เลือกตอน (โหมดเลือก)
*   Batch Actions Toolbar:
    *   "คัดลอกบรรทัดมาร์ค"
    *   "ดาวน์โหลดไฟล์บรรทัดมาร์ค": Modal ตัวเลือก
        *   รวมไฟล์ .txt / แยกไฟล์ .zip (ชื่อไฟล์ .txt ภายใน .zip **ควรเป็นชื่อตอนตามที่แสดงบนเว็บ หรือชื่อไฟล์ต้นฉบับ หากเก็บไว้**)
*   **การพัฒนา:** SvelteKit, Skeleton UI, Lucide-Svelte, Supabase, Logic สร้างไฟล์/ZIP

### 5.5 หน้าอ่านนิยายและโหมดแปล (Reading & Translator Mode UI - `/novel/[novel_id]/[padded_chapter_number]`)

*   **การแสดงผลเนื้อหา (Content Display - `chapters.content_original` เท่านั้น):**
    *   ชื่อตอน, เนื้อหานิยาย (ย่อหน้า, constrained width PC, edge-to-edge mobile, no horizontal scroll)
    *   ปุ่ม "Arrow Up"
    *   Keyboard Controls (PC)
*   **โหมดการอ่าน (Member/Translator - non-translator mode):**
    *   Top Reading Toolbar (PC/Mobile): สารบัญ, ปรับแต่ง, บุ๊คมาร์ค, สลับธีม, โหมดอ่านต่อเนื่อง, นำทางตอน
    *   Mobile Bottom Navigation Toolbar: "< ตอนก่อนหน้า", "ตอนต่อไป >" (เมื่อแตะเนื้อหา)
    *   ท้ายบท: ปุ่ม "ตอนต่อไป" หรือ Infinite Scroll (ถ้าโหมดอ่านต่อเนื่องเปิด)
*   **โหมดนักแปล (Translator Mode):**
    *   Top Reading Toolbar: เหมือนโหมดอ่าน + ปุ่ม "คัดลอกมาร์คทั้งหมด"
    *   การแสดงผลเนื้อหา: แถบเลขบรรทัด, เนื้อหาต้นฉบับ, Checkbox มาร์ค
    *   MarkLine Interaction: คลิก Checkbox (real-time save), ดับเบิลคลิกบรรทัด (ไฮไลท์, คัดลอก, มาร์ค)
    *   ท้ายบท (โหมดนักแปล): สรุปมาร์ค, ปุ่ม "ล้างมาร์คตอนนี้", "คัดลอกมาร์คตอนนี้"
*   **การพัฒนา:** Svelte store, JavaScript/Svelte logic, Skeleton UI, Supabase

### 5.6 แดชบอร์ดผู้ดูแลระบบ (Admin Dashboard - `/admin/dashboard` และหน้าย่อย)

*   **โครงสร้าง:** Sidebar นำทาง, พื้นที่แสดงเนื้อหาหลัก
*   **5.6.1 หน้าภาพรวมแดชบอร์ด:** สถิติ (จำนวนนิยาย, ผู้ใช้)
*   **5.6.2 หน้าจัดการผู้ใช้ (`/admin/dashboard/users`):** ค้นหา, ตารางผู้ใช้ (Avatar, Email, Display Name, Role, วันที่สมัคร, ปุ่ม "จัดการ" -> เปิด Modal)
    *   **Modal จัดการผู้ใช้:** แก้ไข Display Name, Role, วันที่สิทธิ์หมดอายุ, ระงับบัญชี
*   **5.6.3 หน้าจัดการหมวดหมู่นิยาย:** เพิ่ม/แก้ไข/ลบหมวดหมู่ (Modal), ย้ายนิยายเมื่อลบหมวดหมู่
*   **5.6.4 หน้าจัดการประกาศ:** สร้าง/แก้ไข/ลบประกาศ (Modal), ตารางประกาศ
*   **5.6.5 หน้าจัดการเว็บไซต์ (`/admin/dashboard/site-settings`):** ชื่อเว็บ, description, โลโก้, favicon, ตั้งค่าการลงทะเบียน, ตั้งค่าเนื้อหาเริ่มต้น
*   **5.6.6 หน้าจัดการเวอร์ชัน:**
    *   Tab 1: เวอร์ชันเว็บไซต์ / Changelog (เพิ่ม/แก้ไข/ลบ)
    *   Tab 2: เวอร์ชันนิยายและตอน (ค้นหานิยาย, ดู/กู้คืนประวัติเวอร์ชันนิยาย, เลือกตอน, ดู/กู้คืนประวัติเวอร์ชันตอน)

### 5.7 Modal สร้างนิยาย (Create Novel Modal)

*   (ตามข้อ 5.1.1)

### 5.8 การจัดการตอน (Chapter Management)

*   **5.8.1 Modal เพิ่มตอนด้วย .txt (Batch Upload Modal):** (ตามข้อ 5.2.2)
*   **5.8.2 Modal เพิ่ม/แก้ไขตอน (Create/Edit Chapter Modal):** (ตามข้อ 5.2.3)

### 5.9 หน้าโปรไฟล์ (Profile Page - `/users/[user_id]/profile`)

*   **การนำทาง:** จาก Navbar (สำหรับโปรไฟล์ตัวเอง) หรือ Admin Dashboard (สำหรับดู/แก้ไขโปรไฟล์ผู้อื่น)
*   **หัวเรื่อง:** "โปรไฟล์ของ [Display Name]" (ถ้าดูของคนอื่น) หรือ "โปรไฟล์ของฉัน"
*   **ความสามารถ (สำหรับเจ้าของโปรไฟล์):**
    *   เปลี่ยนรูปโปรไฟล์ (Avatar + Crop 1:1)
    *   แก้ไขชื่อที่แสดง (Display Name)
    *   สิทธิ์ผู้ใช้งาน (Role - Read-only Badge)
    *   อีเมล (Read-only)
    *   เปลี่ยนรหัสผ่าน (Modal)
    *   ออกจากระบบ
*   **ความสามารถ (สำหรับ Admin ที่ดูโปรไฟล์ผู้อื่น):** (อาจจะรวมอยู่ใน Modal จัดการผู้ใช้จาก 5.6.2 เพื่อความสอดคล้อง)
    *   ดูข้อมูล, แก้ไขชื่อที่แสดง, Role, วันหมดอายุสิทธิ์, ระงับบัญชี
*   **การพัฒนา:** SvelteKit, Skeleton UI, Felte + Zod, Supabase, Image Cropper library

## 6. Key User Experience Flows & Enhancements

*   การล็อกอิน, การนำทาง, ประสบการณ์การอ่าน, เครื่องมือช่วยแปล, การตอบสนองของระบบ (Empty/Loading/Error States), การออกแบบที่สวยงามและตอบสนองทุกอุปกรณ์, ปุ่ม Arrow Up
*   การแจ้งเตือนการกระทำ (Toast notifications)
*   การสร้าง `mark_lines` อัตโนมัติ
*   การคงสถานะหน้าเมื่อรีเฟรช

## 7. Database Structure (Supabase - PostgreSQL)

*   `roles` (id, name)
*   `profiles` (id, display_name, avatar_url, role_id, role_expiry_date, is_suspended, created_at, updated_at)
*   `categories` (id, name, slug, description, created_at, updated_at)
*   `novels` (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title TEXT NOT NULL,
    slug TEXT UNIQUE NOT NULL, -- Editable by user
    synopsis TEXT, -- Stored as HTML from Rich Text Editor
    cover_image_url TEXT,
    category_id INTEGER REFERENCES categories(id) ON DELETE SET NULL,
    original_language TEXT NOT NULL, -- 'zh', 'en', 'th'
    status TEXT DEFAULT 'ongoing', -- 'ongoing', 'completed', 'dropped'
    visibility JSONB DEFAULT '["everyone"]'::jsonb,
    default_chapter_status TEXT DEFAULT 'published',
    default_chapter_access JSONB DEFAULT '["everyone"]'::jsonb,
    created_by UUID NOT NULL REFERENCES profiles(id), -- When displaying, join with profiles to get display_name
    total_chapters INTEGER DEFAULT 0, -- Denormalized, update with trigger
    last_chapter_updated_at TIMESTAMP WITH TIME ZONE, -- Denormalized, update with trigger
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
    )
*   `chapters` (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    novel_id UUID NOT NULL REFERENCES novels(id) ON DELETE CASCADE,
    **chapter_number TEXT NOT NULL,** -- Padded number, e.g., "001", "0201". UNIQUE with novel_id.
    title TEXT NOT NULL,
    content_original TEXT, -- Stored as HTML from Rich Text Editor if used for synopsis, or plain text if direct txt upload. Assume plain text/markdown from .txt for content, can be rendered.
    status TEXT DEFAULT 'published',
    access_level JSONB DEFAULT '["everyone"]'::jsonb,
    created_by UUID NOT NULL REFERENCES profiles(id),
    word_count INTEGER,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(novel_id, chapter_number)
    )
*   `user_chapter_reads` (user_id, chapter_id, read_at)
*   `user_bookmarks` (id, user_id, chapter_id, novel_id, bookmarked_at)
*   `mark_lines` (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES profiles(id),
    chapter_id UUID NOT NULL REFERENCES chapters(id),
    user_email TEXT,
    user_display_name TEXT,
    novel_id UUID NOT NULL REFERENCES novels(id),
    novel_title TEXT,
    **chapter_number TEXT,** -- Padded chapter number
    chapter_title TEXT,
    marked_line_numbers JSONB NOT NULL DEFAULT '[]'::jsonb,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(user_id, chapter_id)
    )
*   `announcements` (id, title, content, start_date, end_date, target_roles, target_pages, is_active, created_by, created_at, updated_at)
*   `site_settings` (key, value, description, created_at, updated_at)
*   `site_changelogs` (id, version_number, release_date, summary, is_published, created_by, created_at, updated_at)
*   `novel_versions` (id, novel_id, version_number, metadata_snapshot, created_by, notes, created_at)
*   `chapter_versions` (id, chapter_id, version_number, content_original_snapshot, created_by, notes, created_at)

*   **Row-Level Security (RLS):** เข้มงวดในทุกตารางที่เกี่ยวข้อง
*   **Database Triggers:** สำหรับ `updated_at`, denormalized fields (`total_chapters`, `last_chapter_updated_at`), และอัปเดต `mark_lines` denormalized fields เมื่อข้อมูลต้นทางเปลี่ยน

## 8. Supabase Storage Buckets

*   `novel-covers` (Public: true)
*   `chapter-text-files` (Public: false, restricted)
*   `avatars` (Public: true)
*   `site_assets` (Public: true)

## 9. สรุป Tech Stack (Tech Stack Summary)

*   Framework: SvelteKit
*   Styling: TailwindCSS
*   UI Library: Skeleton UI
*   Form Handling & Validation: Felte และ Zod
*   Icon Library: Lucide-Svelte
*   Rich Text Editor: TipTap (สำหรับ Synopsis และ Content ตอนที่เพิ่ม/แก้ไขผ่าน UI)
*   Image Cropping: Cropper.js หรือ Svelte-Cropper
*   Backend & Auth: Supabase Platform (PostgreSQL, Supabase Auth, Supabase Storage, Supabase Edge Functions)
*   ภาษา: TypeScript

## 10. การจัดการข้อผิดพลาดเบื้องต้น (Basic Error Handling)

*   Client-Side Validation (Zod + Felte)
*   Server-Side Validation (Zod in SvelteKit actions/Edge Functions)
*   User-Facing Error Messages (Toast, UI messages)
*   Code-Level Error Handling (try...catch, Supabase error checking)
*   Error Logging (console, Supabase logs, consider Sentry/Logtail for production)

## 11. มาตรการความปลอดภัยเบื้องต้น (Basic Security Measures)

*   Authentication (Supabase Auth, Email Verification)
*   Authorization (RLS in all critical tables)
*   Input Validation (Zod client/server, HTML sanitization for Rich Text output)
*   HTTPS
*   Environment Variables (Service Role Key only server-side)
*   CSRF Protection (SvelteKit built-in)
*   HTTP Security Headers
*   Data Backups (Supabase + manual considerations)
*   Principle of Least Privilege
*   Software & Dependency Updates

```

**หมายเหตุสำคัญเพิ่มเติม:**

1.  **`chapters.content_original`:** ถ้า Synopsis ใช้ Rich Text Editor (เก็บเป็น HTML) และตอนนิยายอัปโหลดเป็น `.txt` (plain text) แล้วมีการแก้ไขผ่าน Rich Text Editor ใน Modal เพิ่ม/แก้ไขตอน, เนื้อหาของ `content_original` จะกลายเป็น HTML เช่นกัน ต้องวางแผนการแสดงผลให้สอดคล้องกัน (เช่น ใช้ `{@html ...}` อย่างระมัดระวัง หรือมี parser/renderer) หรือถ้าต้องการให้เนื้อหาตอนเป็น Markdown/Plain Text เสมอ แม้จะแก้ผ่าน Rich Text Editor ก็ต้องมี logic แปลงกลับ
2.  **Padding `chapter_number`:** การจัดการ padding (เช่น ทำให้เป็น 3 หรือ 4 หลักเสมอ) ต้องทำอย่างสม่ำเสมอทั้งตอนสร้าง, แก้ไข, และ batch upload เพื่อให้การเรียงลำดับและการ query ทำงานถูกต้อง
3.  **Denormalized Data in `mark_lines`:** ย้ำอีกครั้งว่าการมี `user_email`, `user_display_name`, `novel_title`, `chapter_title`, `chapter_number` ใน `mark_lines` นั้นช่วยให้อ่านง่าย แต่ **จำเป็นต้องมี Database Triggers หรือ Application-Level Logic ที่แข็งแรง** เพื่ออัปเดตฟิลด์เหล่านี้เมื่อข้อมูลในตาราง `profiles`, `novels`, หรือ `chapters` มีการเปลี่ยนแปลง มิฉะนั้นข้อมูลจะไม่ตรงกัน

เอกสารนี้ควรจะครอบคลุมความต้องการล่าสุดของคุณแล้วครับ!
