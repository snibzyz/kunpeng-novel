ยอดเยี่ยมครับ! ผมได้ดำเนินการปรับปรุงเอกสาร README.md สำหรับ INKREALM.CO (V5.1) ตามข้อเสนอแนะและการแก้ไขล่าสุดทั้งหมด โดยยังคงความละเอียดของข้อมูลและโครงสร้างเดิมไว้ พร้อมจัดลำดับความสำคัญของเนื้อหาตามที่ควรจะเป็น

นี่คือฉบับปรับปรุงที่สมบูรณ์:


# INKREALM.CO: ข้อกำหนดระบบและคุณสมบัติหลัก (V5.1) - ฉบับปรับปรุง README

เอกสารนี้เป็นข้อกำหนดระบบและคุณสมบัติหลักสำหรับแพลตฟอร์ม INKREALM.CO (V5.1) โดยเน้นฟังก์ชันที่จำเป็นตามการปรับปรุงล่าสุด โดยยังคงอ้างอิง Tech Stack (SvelteKit & Supabase Stack) ที่กำหนดไว้ และออกแบบมาเพื่อความสะดวกในการปรับใช้, ทดสอบ, และย้ายไปยังโดเมนต่างๆ

---

**ลำดับความสำคัญในการพิจารณาและพัฒนา (โดยรวม):**

1.  **ความปลอดภัยของระบบและข้อมูล (Security & Data Integrity):** การยืนยันตัวตน, การกำหนดสิทธิ์ (RLS), การป้องกันข้อมูลรั่วไหล, การตรวจสอบข้อมูลนำเข้า, การสำรองข้อมูล
2.  **โครงสร้างฐานข้อมูล (Database Structure):** ความถูกต้อง, ความครบถ้วน, ความสัมพันธ์ของข้อมูล, การรองรับการขยายตัว และการ query ที่มีประสิทธิภาพ (รวมถึงการ denormalize ที่จำเป็นอย่าง `mark_lines`)
3.  **ระบบหลัก (Core Systems):** ระบบยืนยันตัวตน, ระบบจัดการเนื้อหานิยาย (CMS), ระบบการอ่านและบุ๊คมาร์ค, ระบบเครื่องมือสำหรับนักแปล
4.  **ประสบการณ์ผู้ใช้ (User Experience - UX):** การใช้งานง่าย, ความต่อเนื่อง, การตอบสนองของระบบ, การจัดการข้อผิดพลาดที่เป็นมิตร
5.  **การออกแบบส่วนติดต่อผู้ใช้ (User Interface - UI):** ความสวยงาม, การอ่านง่าย, ความสอดคล้อง, Responsive Design
6.  **คุณสมบัติเสริมและส่วนอำนวยความสะดวก (Enhancements & Utilities):** ระบบประกาศ, แดชบอร์ด, การจัดการเว็บไซต์, การจัดการเวอร์ชัน

---

## 1. ปรัชญาการออกแบบโดยรวม

*   **การแสดงผลหรูหราและอ่านง่าย:**
    *   **โทนสีหลัก:** สีทอง (เช่น `#FFD700`, `#B8860B` หรือโทนใกล้เคียงที่ให้ความรู้สึกหรูหรา)
    *   **สีรอง:** สีเทา (หลายเฉด เช่น `#F0F0F0`, `#CCCCCC`, `#808080`, `#333333`)
    *   **โหมดสว่าง (Light Mode):** เน้นสีทอง, เทาอ่อน, และขาว เพื่อความสบายตา ดูสะอาด และหรูหรา
    *   **โหมดมืด (Dark Mode):** เน้นสีทอง (อาจปรับให้สว่างขึ้นเล็กน้อยเพื่อคอนทราสต์), เทาเข้ม, และดำ เพื่อลดแสงสะท้อนและช่วยให้อ่านง่ายในที่มืด
    *   **โลโก้:** พิจารณาใช้การไล่ระดับสี (gradient) ที่ผสมผสานสีทองและสีเทาอย่างลงตัว หรือสีทองล้วนบนพื้นหลังที่เหมาะสม เพื่อความโดดเด่นและทันสมัย
    *   **ฟอนต์หลัก:** Sarabun สำหรับการแสดงผลภาษาไทยที่สวยงามและอ่านง่าย; พิจารณาฟอนต์สำรองสำหรับภาษาอังกฤษที่เข้ากัน (เช่น Noto Sans, Open Sans)
*   **เครื่องมือสำหรับนักแปล:** มีฟังก์ชันสนับสนุนการทำงานของนักแปลโดยเฉพาะ เน้นความชัดเจนและประสิทธิภาพ
*   **ใช้งานง่าย ตอบโจทย์ทุกอุปกรณ์:**
    *   UI ใช้งานง่าย (Intuitive) และสอดคล้องกันทั้งระบบ
    *   Responsive Design เพื่อประสบการณ์ที่ดีในทุกขนาดหน้าจอ
    *   หน้าอ่านนิยายไม่มี Horizontal Scroll โดยเด็ดขาด
*   **ความยืดหยุ่นและการทดสอบ:** โครงสร้างที่เอื้อต่อการเปลี่ยนแปลงโดเมนและการทดสอบระบบที่ง่ายดาย
*   **การเปลี่ยนธีม:**
    *   การสลับระหว่างโหมดสว่างและโหมดมืดจะต้องราบรื่น (อาจมี transition เบาๆ)
    *   สีที่แสดงผลในแต่ละโหมดต้องไม่ผิดเพี้ยนจากที่ออกแบบไว้ และมีคอนทราสต์ที่เหมาะสมสำหรับการอ่าน
    *   ควบคุมผ่านองค์ประกอบ UI ที่ชัดเจน (เช่น ไอคอนพระอาทิตย์/พระจันทร์ ที่มีการเปลี่ยนแปลงสถานะชัดเจน) และสถานะธีมต้องถูกบันทึกไว้ในการตั้งค่าของผู้ใช้ (localStorage และ/หรือ profile)

## 1.1 ภาพรวมระบบหลัก (Core System Overview)

*   **ระบบยืนยันตัวตนและจัดการผู้ใช้:** ลงทะเบียน, เข้าสู่ระบบ (อีเมล/รหัสผ่าน, Google OAuth), ลืมรหัสผ่าน, จัดการโปรไฟล์ (รวมถึงการเปลี่ยนรูปโปรไฟล์, ชื่อที่แสดง, รหัสผ่าน), การจัดการบทบาทและสิทธิ์ (Admin ควบคุม)
*   **ระบบจัดการเนื้อหานิยาย (CMS):**
    *   การสร้าง/แก้ไข/ลบนิยายและตอน (ดำเนินการผ่าน Modals เป็นหลัก เพื่อ UX ที่ต่อเนื่อง)
    *   การอัปโหลดเนื้อหา: รูปปกนิยาย (พร้อม crop), ไฟล์ .txt สำหรับตอนนิยาย (Batch Upload)
    *   การจัดการหมวดหมู่นิยาย (สำหรับ Admin)
    *   ระบบจัดการเวอร์ชันสำหรับข้อมูลเมตานิยายและเนื้อหาตอน
*   **ระบบการอ่านและบุ๊คมาร์ค:**
    *   หน้าอ่านที่ปรับแต่งการแสดงผลได้ (ขนาดฟอนต์, font-family เบื้องต้น, ระยะห่างบรรทัด)
    *   ระบบบุ๊คมาร์คตอนที่ผู้ใช้อ่านค้างไว้
    *   โหมดอ่านต่อเนื่อง (Infinite Scroll)
    *   การสลับโหมดการอ่านสำหรับนักแปล (เปิด/ปิด "โหมดนักแปล" สำหรับนิยายนั้นๆ)
*   **ระบบเครื่องมือสำหรับนักแปล:**
    *   "โหมดนักแปล" (เมื่อเปิดใช้งานสำหรับนิยายที่กำหนด): แสดงเลขบรรทัด, checkbox สำหรับมาร์คบรรทัด
    *   ระบบบรรทัดมาร์ค (Mark Lines): บันทึกบรรทัดที่นักแปลเลือก
    *   หน้าจัดการบรรทัดมาร์ค: ดูภาพรวม, คัดลอก, ดาวน์โหลดมาร์ค
*   **ระบบแดชบอร์ด:**
    *   **สำหรับนักแปล:** ภาพรวมนิยายที่ตนเองจัดการ, ทางลัดไปหน้าจัดการบรรทัดมาร์ค
    *   **สำหรับผู้ดูแลระบบ:** ภาพรวมระบบ, จัดการผู้ใช้, จัดการหมวดหมู่, จัดการประกาศ, จัดการเว็บไซต์ (การตั้งค่าทั่วไป), จัดการเวอร์ชัน (เว็บไซต์, นิยาย, ตอน)
*   **ระบบฐานข้อมูลและไฟล์:** Supabase (PostgreSQL สำหรับฐานข้อมูล, Supabase Storage สำหรับไฟล์)
*   **ระบบการแจ้งเตือนผู้ใช้ (User Feedback System):** การใช้ Toast notifications (เช่น ของ Skeleton UI) สำหรับยืนยันการกระทำ, แจ้งเตือนข้อผิดพลาด, หรือให้ข้อมูลสั้นๆ
*   **ระบบประกาศ (Announcement System):** สำหรับ Admin ในการสร้างและเผยแพร่ประกาศไปยังกลุ่มผู้ใช้เป้าหมายและหน้าที่กำหนด

## 1.2 โครงสร้าง URL ที่เป็นมิตรและการจัดการโดเมน (User-Friendly URL Structure & Domain Management)

เพื่อให้ URL ของเว็บไซต์ INKREALM.CO อ่านง่าย, เป็นมิตรต่อผู้ใช้, ดีต่อ SEO, และง่ายต่อการย้ายโดเมน:

*   **โครงสร้าง URL หลัก:**
    *   หน้ารวมนิยาย (หน้าแรก): `/`
    *   หน้าข้อมูลนิยาย: `/novels/[novel_id]` (โดย `novel_id` คือ UUID ของนิยาย)
    *   หน้าอ่านตอนนิยาย: `/novels/[novel_id]/[chapter_number]` (โดย `chapter_number` คือหมายเลขตอน อาจมี padding เช่น `001`, `123`)
    *   หน้าโปรไฟล์ผู้ใช้ (สำหรับผู้ใช้ปัจจุบัน): `/profile`
    *   หน้าโปรไฟล์ผู้ใช้ (สำหรับ Admin ดูผู้ใช้อื่น): `/admin/dashboard/users/[user_id]` (ซึ่งจะแสดงรายละเอียดและฟอร์มแก้ไขของผู้ใช้นั้น)
    *   หน้าบุ๊คมาร์ค: `/bookmarks`
    *   แดชบอร์ดนักแปล: `/translator/dashboard`
    *   หน้าจัดการบรรทัดมาร์ค (นักแปล): `/translator/manage-marks/[novel_id]` (ใช้ `novel_id` แทน `novel_slug`)
    *   แดชบอร์ดผู้ดูแลระบบ: `/admin/dashboard`
    *   หน้าย่อยในแดชบอร์ดผู้ดูแลระบบ: `/admin/dashboard/users`, `/admin/dashboard/categories`, `/admin/dashboard/announcements`, `/admin/dashboard/site-settings`, `/admin/dashboard/versions`
*   **การสร้าง Slug สำหรับนิยาย (`novel_slug`):**
    *   `novel_slug` จะถูกเก็บในฐานข้อมูล `novels` table
    *   ถูกสร้างขึ้นโดยอัตโนมัติเมื่อสร้างนิยายใหม่ โดยแปลงชื่อเรื่องเป็นตัวพิมพ์เล็ก, แทนที่ช่องว่างและอักขระพิเศษที่ไม่ปลอดภัยสำหรับ URL ด้วยขีดกลาง (`-`), และตัดอักขระพิเศษอื่นๆ ออก
    *   ผู้สร้าง/Admin สามารถแก้ไข `novel_slug` นี้ได้ในหน้าแก้ไขข้อมูลนิยาย (ต้องมีการตรวจสอบความซ้ำซ้อนของ `novel_slug` ในระบบก่อนบันทึก)
    *   แม้ URL หลักจะใช้ `novel_id` แต่ `novel_slug` ยังคงมีประโยชน์สำหรับ SEO (เช่น การสร้าง sitemap) หรือการอ้างอิงภายในที่อ่านง่ายกว่า
*   **สำหรับตอนนิยาย:** จะใช้ `chapter_number` (ที่อาจมี padding) โดยตรงใน URL
*   **การจัดการโดเมนและ Base URL:**
    *   **Environment Variables:** การตั้งค่า Base URL ของเว็บไซต์ (เช่น `https://inkrealm.co` หรือ `http://localhost:3000` สำหรับการพัฒนา) จะถูกกำหนดผ่าน Environment Variable ของ SvelteKit (เช่น `PUBLIC_BASE_URL`). การย้ายโดเมนทำได้โดยแก้ไขค่านี้เท่านั้น
    *   **การสร้างลิงก์ภายใน (Internal Linking):** ทุกลิงก์ภายในเว็บไซต์จะถูกสร้างขึ้นโดยอ้างอิงจาก Base URL (ถ้าจำเป็น) หรือใช้ Relative Paths เป็นหลัก เพื่อให้ทำงานได้อย่างถูกต้องไม่ว่าจะอยู่บนโดเมนใด
    *   **การตั้งค่า Supabase:** `PUBLIC_SUPABASE_URL` และ `PUBLIC_SUPABASE_ANON_KEY` จะถูกเก็บไว้ใน Environment Variables

## 2. บทบาทผู้ใช้และสิทธิ์ (User Roles & Permissions)

ระบบแบ่งบทบาทผู้ใช้ออกเป็น: สมาชิก (Member), นักแปล (Translator), และ ผู้ดูแลระบบ (Admin) โดยแต่ละบทบาทมีสิทธิ์การเข้าถึงและจัดการข้อมูลแตกต่างกันอย่างชัดเจน (ควบคุมโดย Supabase Row-Level Security - RLS เป็นหลัก และเสริมด้วย logic ใน SvelteKit ตามความเหมาะสม)

*   **สมาชิก (Member):**
    *   **ความสามารถหลัก:** อ่านนิยาย (ตามสิทธิ์การเข้าถึงของนิยาย/ตอน), บุ๊คมาร์คตอน, แก้ไขโปรไฟล์ส่วนตัว (ชื่อที่แสดง, รูปโปรไฟล์, รหัสผ่าน)
    *   **แถบเครื่องมือสำหรับอ่าน (Reading Toolbar):** สารบัญ (เปิด Modal), ปรับแต่งการแสดงผล (เปิด Popover), บุ๊คมาร์คตอนปัจจุบัน (สลับสถานะ), สลับธีมเว็บ (Light/Dark), เปิด/ปิดโหมดอ่านต่อเนื่อง, นำทางตอนก่อนหน้า/ถัดไป
*   **นักแปล (Translator):**
    *   **ความสามารถหลัก:**
        *   สิทธิ์ทั้งหมดของสมาชิก
        *   สามารถเปิด/ปิด "โหมดนักแปล" สำหรับนิยายที่ตนเองได้รับสิทธิ์แปล (ตั้งค่าในหน้าข้อมูลนิยายของเรื่องนั้นๆ)
        *   เข้าถึง "โหมดแปล" (เมื่อเปิดใช้งานสำหรับนิยายนั้น): แสดงเลขบรรทัด, checkbox, ดับเบิลคลิกเพื่อมาร์คและคัดลอกบรรทัด
        *   ใช้งานระบบบรรทัดมาร์ค (Mark Lines)
        *   สร้าง/แก้ไข/ลบนิยายและตอนของตนเอง (นิยายที่ตน `created_by` หรือได้รับสิทธิ์จัดการ) การดำเนินการผ่าน Modal
        *   เข้าถึงแดชบอร์ดนักแปลและหน้าจัดการบรรทัดมาร์คสำหรับนิยายของตน
    *   **แถบเครื่องมือพิเศษในโหมดแปล (เมื่อเปิด "โหมดนักแปล" สำหรับนิยายนั้น และเข้าหน้าอ่านตอน):** เหมือนของ Member แต่เพิ่ม: ปุ่ม "คัดลอกมาร์คทั้งหมด" (Copy All Marks) สำหรับตอนปัจจุบัน
*   **ผู้ดูแลระบบ (Admin):**
    *   **ความสามารถหลัก:**
        *   สิทธิ์ทั้งหมดของนักแปล (รวมถึงการสร้าง/จัดการนิยายและตอนใดๆ ในระบบ)
        *   เข้าถึงแดชบอร์ดผู้ดูแลระบบ
        *   จัดการผู้ใช้ทั้งหมด: ดู, แก้ไขโปรไฟล์ (ชื่อที่แสดง, รูปโปรไฟล์), เปลี่ยนบทบาท, กำหนดวันหมดอายุสิทธิ์, ระงับบัญชี (ผ่านหน้า User Management และ Modal หรือหน้าเฉพาะ `/admin/dashboard/users/[user_id]`)
        *   ดูข้อมูลบรรทัดมาร์คทั้งหมดของนักแปลทุกคน
        *   การจัดการหมวดหมู่นิยาย: เพิ่ม/แก้ไข/ลบหมวดหมู่
        *   การจัดการประกาศ: สร้าง/แก้ไข/ลบ/เผยแพร่ประกาศ
        *   การจัดการเว็บไซต์: แก้ไขการตั้งค่าทั่วไปของเว็บไซต์
        *   การจัดการเวอร์ชัน: ดูและกู้คืนเวอร์ชันของเว็บไซต์, นิยาย, และตอน

## 3. การเข้าสู่ระบบ / ลงทะเบียน / ลืมรหัสผ่าน (Authentication Modals & Pages)

*   **ความสามารถ:** ลงทะเบียนผู้ใช้ใหม่, เข้าสู่ระบบด้วยอีเมล/รหัสผ่าน หรือ Google OAuth, กระบวนการลืมและรีเซ็ตรหัสผ่าน
*   **การแสดงผล:**
    *   แสดงผลผ่าน **Modal** เป็นหลักเมื่อผู้ใช้คลิกปุ่ม "เข้าสู่ระบบ" หรือ "ลงทะเบียน" จากภายใน Navbar หรือส่วนอื่นๆ ของเว็บที่ผู้ใช้ยังไม่ได้ล็อกอิน
    *   มีหน้าเว็บเฉพาะ (เช่น `/login`, `/register`, `/forgot-password`, `/reset-password`) สำหรับการเข้าถึงโดยตรง
*   **การพัฒนา:** Supabase Auth, Felte + Zod, Skeleton UI
*   **การแจ้งเตือน (User Feedback):** แสดง Toast notification (เช่น "เข้าสู่ระบบสำเร็จ", "ลงทะเบียนเรียบร้อย กรุณายืนยันอีเมล", "คำขอรีเซ็ตรหัสผ่านถูกส่งแล้ว")

## 4. โครงสร้างแถบนำทาง (Navbar Structure)

*   **ความสามารถ:** แสดงเมนูแบบไดนามิกตามบทบาทของผู้ใช้, Responsive Design
*   **ตัวอย่างเมนู:**
    *   **Guest:** โลโก้, ปุ่มสลับธีม, ปุ่ม "เข้าสู่ระบบ", ปุ่ม "ลงทะเบียน"
    *   **Member:** โลโก้, "หน้าแรก", "บุ๊คมาร์ค", ปุ่มสลับธีม, โปรไฟล์ (Dropdown: ชื่อ, สิทธิ์, "หน้าโปรไฟล์", "ออกจากระบบ")
    *   **Translator:** เหมือน Member เพิ่ม "แดชบอร์ดนักแปล"
    *   **Admin:** เหมือน Translator เพิ่ม "แดชบอร์ดผู้ดูแลระบบ"
*   **การพัฒนา:** SvelteKit, Skeleton UI, TailwindCSS

## 5. รายละเอียดหน้าเว็บไซต์, Modals, และการจัดการเนื้อหา

### 5.1 หน้าแรก (Home Page - `/`)

*   **ส่วนแสดงประกาศ (Announcements Display Area):** แสดงตามเงื่อนไข (Active, Date, Target Page, Target Role) มีปุ่มปิด
*   **ส่วนเครื่องมือหลัก:**
    *   ช่องค้นหานิยาย
    *   ฟิลเตอร์หมวดหมู่ (Dropdown)
    *   ฟิลเตอร์สถานะนิยาย (Dropdown: ต่อเนื่อง, จบแล้ว, ยุติ)
    *   **ปุ่มสร้างนิยาย:** [ไอคอน Lucide-Svelte เช่น `PlusSquare` หรือ `FilePlus2`] + ข้อความ "สร้างนิยาย" (สำหรับ Translator/Admin) เมื่อคลิก เปิด Modal สร้างนิยาย (ดูข้อ 5.1.1)
*   **การแสดงรายการนิยาย:**
    *   Grid Layout, การ์ดนิยายแสดง: **รูปปก (3:4), ชื่อเรื่อง, จำนวนตอนทั้งหมด**
    *   Responsive: PC (4-5 คอลัมน์), Tablet (3 คอลัมน์), Mobile (1-2 คอลัมน์)
*   **ระบบแบ่งหน้า (Pagination):** แสดงเมื่อผลลัพธ์มีมากกว่าหนึ่งหน้า
*   **การพัฒนา:** Supabase (query), Skeleton UI

### 5.1.1 Modal สร้างนิยาย (Create Novel Modal)

*   **การเรียกใช้งาน:** คลิกปุ่ม "[ไอคอน] สร้างนิยาย" บนหน้าแรก (สำหรับ Translator/Admin)
*   **UI ภายใน Modal:**
    *   **หัวเรื่อง Modal:** "สร้างนิยายใหม่"
    *   **ฟอร์ม (ใช้ Felte + Zod):**
        *   **ชื่อเรื่อง (Title):** Input text, จำเป็น
        *   **คำโปรย (Synopsis):** **Rich Text Editor** (เช่น TipTap), (Zod: optional, maxlength)
        *   **หมวดหมู่ (Category):** Dropdown เลือกจาก `categories` table
        *   **รูปปกนิยาย (Cover Image):** อัปโหลด, Crop 3:4, Preview, จัดการขนาดไฟล์ (ไม่เกิน 3MB)
        *   **ภาษาต้นฉบับ (Original Language):** Dropdown เลือก ("จีน (zh)", "อังกฤษ (en)", "ไทย (th)"), จำเป็น
        *   **Slug นิยาย (`novel_slug`):** Input text, auto-generate จากชื่อเรื่อง, ผู้ใช้สามารถแก้ไขได้ (ตรวจสอบความซ้ำซ้อน)
        *   **สถานะนิยาย (Novel Status):** Dropdown/Radio Group ("ต่อเนื่อง", "จบแล้ว", "ยุติการลง"), ค่าเริ่มต้น "ต่อเนื่อง"
        *   **สิทธิ์ในการมองเห็นนิยาย (Novel Visibility):** Multi-select ("ทุกคน", "เฉพาะสมาชิก", "เฉพาะนักแปล"), ค่าเริ่มต้น "ทุกคน"
        *   **การตั้งค่าเริ่มต้นสำหรับตอนใหม่ของนิยายนี้:**
            *   **สถานะเริ่มต้นของตอน:** Radio Group ("เผยแพร่" (ค่าเริ่มต้น), "ฉบับร่าง")
            *   **สิทธิ์การเข้าถึงตอนเริ่มต้น:** Multi-select ("ทุกคน" (ค่าเริ่มต้น), "เฉพาะสมาชิก", "เฉพาะนักแปล")
    *   **ปุ่มดำเนินการ:** "เผยแพร่นิยาย", "บันทึกเป็นฉบับร่าง", "ยกเลิก" (Toast feedback)
*   **การพัฒนา:** Skeleton UI Modal, Felte + Zod, TipTap, Cropper.js, Supabase (สร้าง `novels`, อัปโหลด Storage, สร้าง `novel_versions`)

### 5.2 หน้าข้อมูลนิยายและสารบัญ (Novel Detail Page - `/novels/[novel_id]`)

*   **ส่วนแสดงประกาศ:** แสดงตามเงื่อนไข
*   **ส่วนข้อมูลนิยาย:** รูปปก, ชื่อเรื่อง, หมวดหมู่, คำโปรย (แสดงผล Rich Text), จำนวนตอน, สถานะนิยาย ("ต่อเนื่อง", "จบแล้ว", "ยุติการลง"), ภาษาต้นฉบับ, `novel_slug` (แสดง, ไม่แก้ไขตรงนี้)
    *   (สำหรับนักแปล/Admin เจ้าของ) ปุ่มสลับ "โหมดนักแปล" (Toggle Switch) บันทึกสถานะเฉพาะนักแปล+นิยาย
*   **ปุ่มจัดการนิยาย (สำหรับผู้สร้าง/Admin):**
    *   ปุ่ม "แก้ไขข้อมูลนิยาย": เปิด Modal แก้ไขข้อมูลนิยาย (ดูข้อ 5.2.1)
    *   ปุ่ม "ลบนิยาย": Modal ยืนยัน, Toast
*   **ส่วนเพิ่มตอน (สำหรับผู้สร้าง/Admin):**
    *   ปุ่ม "เพิ่มตอนด้วย .txt" (เปิด Modal 5.2.2)
    *   ปุ่ม "เพิ่มตอน" (เปิด Modal 5.2.3)
*   **ส่วนสารบัญ:**
    *   Collapse/Accordion, ปรับจำนวนตอนต่อกลุ่ม
    *   **รายการตอน:**
        *   (ทุกคน): ไอคอนบุ๊คมาร์ค, หมายเลขตอน, ชื่อตอน, สถานะการอ่าน
        *   (ผู้สร้าง/Admin): เพิ่มไอคอนสถานะตอน, ปุ่ม "แก้ไขตอน", ปุ่ม "ลบตอน", **ปุ่ม "คัดลอกมาร์คทั้งหมด"** (หากนักแปลเป็นเจ้าของและมีมาร์คสำหรับตอนนี้)
*   **การพัฒนา:** Skeleton UI, Supabase, Svelte logic

### 5.2.1 Modal แก้ไขข้อมูลนิยาย (Edit Novel Modal)

*   **การเรียกใช้งาน:** คลิกปุ่ม "แก้ไขข้อมูลนิยาย"
*   **UI ภายใน Modal:** ฟอร์มเติมด้วยข้อมูลปัจจุบัน, เหมือน 5.1.1 แต่สำหรับแก้ไข:
    *   หัวเรื่อง: "แก้ไขข้อมูลนิยาย: [ชื่อนิยายปัจจุบัน]"
    *   ฟิลด์ที่แก้ไขได้: ชื่อเรื่อง, คำโปรย (Rich Text), หมวดหมู่, รูปปก, ภาษาต้นฉบับ (zh, en, th), **Slug นิยาย (`novel_slug`)** (แก้ไขได้, ตรวจสอบความซ้ำซ้อน), สถานะนิยาย ("ต่อเนื่อง", "จบแล้ว", "ยุติการลง"), สิทธิ์มองเห็นนิยาย, ค่าเริ่มต้นสำหรับตอนใหม่
    *   ปุ่มดำเนินการ: "บันทึกการเปลี่ยนแปลง" (Toast, สร้าง `novel_versions`), "ยกเลิก", (อาจมี) "ลบนิยาย"
*   **การพัฒนา:** เหมือน 5.1.1 แต่เป็น action update, สร้าง `novel_versions`

### 5.2.2 Modal เพิ่มตอนด้วย .txt (Batch Upload Modal)

*   **การเรียกใช้งาน:** คลิก "เพิ่มตอนด้วย .txt"
*   **UI:** Drag & Drop/เลือกไฟล์ .txt (Max 500), Sort ชื่อไฟล์, ใช้ชื่อไฟล์เป็นชื่อตอน (หรือส่วนหนึ่งของชื่อไฟล์)
    *   **การจัดการ Chapter Number จากชื่อไฟล์:** หากชื่อไฟล์เป็นรูปแบบ `[ส่วนที่เป็นชื่อตอน] [เลขตอนที่มี padding]` เช่น "Chapter 001", "ตอนที่ 0004", ระบบจะดึง `[เลขตอนที่มี padding]` (เช่น "001", "0004") มาใช้เป็น `chapters.chapter_number` (เก็บเป็น TEXT) และจะใช้ส่วนที่เหลือเป็นชื่อตอน หากนิยายเริ่มด้วยตอนที่สูงๆ เช่น 201 ระบบจะเริ่มนับและแสดงจากตอนนั้นๆ
*   **ความสามารถ:** จัดการไฟล์ซ้ำ (Overwrite/Skip), Progress Bar, Toast สรุปผล
*   **การทำงานเบื้องหลัง:** สร้าง `chapters` (เก็บ `chapter_number` พร้อม padding ถ้ามี), `mark_lines` ว่างๆ, `chapter_versions` เริ่มต้น
*   **การพัฒนา:** Skeleton UI, Supabase

### 5.2.3 Modal เพิ่ม/แก้ไขตอน (Create/Edit Chapter Modal)

*   **การเรียกใช้งาน:** ปุ่ม "เพิ่มตอน" หรือ "แก้ไขตอน"
*   **UI ภายใน Modal:**
    *   หัวเรื่อง: "เพิ่มตอนใหม่" / "แก้ไขตอน: [ชื่อตอน]"
    *   **ฟอร์ม:**
        *   ชื่อตอน (จำเป็น)
        *   **หมายเลขตอน (`chapter_number`):**
            *   **เพิ่มตอนใหม่:** Auto-increment จากตอนล่าสุด +1 (แสดงเป็นค่าเริ่มต้น), **ผู้ใช้สามารถแก้ไขได้** (ต้อง validate ไม่ซ้ำ และเป็นตัวเลข หากต้องการ padding ให้ logic จัดการตอนบันทึก หรือให้ผู้ใช้ใส่เอง)
            *   **แก้ไขตอน:** แสดงเลขปัจจุบัน, **ผู้ใช้สามารถแก้ไขได้** (ต้องระวังเรื่องลำดับและ validate ความซ้ำซ้อน)
            *   *หมายเหตุ:* `chapter_number` จะถูกเก็บเป็น TEXT ในฐานข้อมูล
        *   เนื้อหา (`content_original`): Rich Text Editor (TipTap)
        *   สถานะตอน ("เผยแพร่", "ฉบับร่าง")
        *   สิทธิ์การเข้าถึงตอน ("ทุกคน", "เฉพาะสมาชิก", "เฉพาะนักแปล")
        *   *ค่าเริ่มต้นสำหรับตอนใหม่ ดึงจาก Novel settings, แก้ไขเฉพาะตอนนี้ได้*
    *   ปุ่มดำเนินการ: "เผยแพร่ตอน", "บันทึกเป็นฉบับร่าง" (สำหรับเพิ่มใหม่); "บันทึกการเปลี่ยนแปลง" (สำหรับแก้ไข, สร้าง `chapter_versions`); "ยกเลิก"
*   **การทำงานเบื้องหลัง:** สร้าง/อัปเดต `chapters`, `mark_lines` (สำหรับสร้างใหม่), `chapter_versions`
*   **การพัฒนา:** Skeleton UI Modal, Felte + Zod, TipTap, Supabase

### 5.3 หน้าบุ๊คมาร์ค (Bookmarks Page - `/bookmarks`)

*   แสดงรายการนิยายที่บุ๊คมาร์ค, เรียงตามวันที่บุ๊คมาร์คล่าสุดของนิยาย
*   แต่ละนิยาย: ปก, ชื่อ, ตอนล่าสุดที่บุ๊คมาร์ค, ปุ่ม "ไปยังบุ๊คมาร์คล่าสุด"
*   ปุ่ม "จัดการบุ๊คมาร์ค" (ต่อเรื่อง) -> Modal จัดการ (เรียง, เลือก, ลบที่เลือก, ล้างทั้งหมดของเรื่อง, Toast)
*   ปุ่ม "ลบบุ๊คมาร์คทั้งหมด (ทุกเรื่อง)" (Modal ยืนยัน, Toast)
*   **การพัฒนา:** Supabase, Skeleton UI, Lucide-Svelte

### 5.4 แดชบอร์ดนักแปล (Translator Dashboard - `/translator/dashboard`)

*   แสดงรายการนิยายที่นักแปลจัดการ (การ์ด: ปก, ชื่อ, % ความคืบหน้ามาร์ค + Progress Bar)
*   ปุ่ม "จัดการบรรทัดมาร์ค" (นำทางไป `/translator/manage-marks/[novel_id]`)
*   **การพัฒนา:** Supabase, Skeleton UI

### 5.4.1 หน้าจัดการบรรทัดมาร์ค (Mark Line Management Page - `/translator/manage-marks/[novel_id]`)

*   หัวเรื่อง: "จัดการบรรทัดมาร์คสำหรับ: [ชื่อนิยาย]"
*   ส่วนควบคุม: ปุ่ม "เลือกตอนเพื่อคัดลอก/ดาวน์โหลด" (Toggle โหมดเลือก)
*   สารบัญตอน: Collapse/Accordion, ปรับจำนวนตอน, รายการตอน (เลขตอน, ชื่อ, จำนวนมาร์ค, สถานะมาร์ค, ปุ่ม "คัดลอกมาร์คตอนนี้", Checkbox เลือกตอน)
*   Batch Actions Toolbar (เมื่อเลือกตอน): ปุ่ม "คัดลอกบรรทัดมาร์ค", ปุ่ม "ดาวน์โหลดไฟล์บรรทัดมาร์ค"
    *   **Modal ดาวน์โหลดไฟล์:**
        *   ตัวเลือก: "รวมไฟล์ .txt" / "แยกไฟล์ .zip"
        *   **ชื่อไฟล์ใน .zip:** จะเป็นชื่อเดียวกับที่แสดงในหน้าเว็บหรือชื่อไฟล์ต้นฉบับที่อัปโหลด (เช่น `[chapter_number_with_padding]-[chapter_title].txt` หรือ `[original_filename_if_available].txt`). ให้ยึดตาม `chapters.title` และ `chapters.chapter_number` เป็นหลักในการสร้างชื่อไฟล์
        *   Toast feedback
*   **การพัฒนา:** SvelteKit, Skeleton UI, Lucide-Svelte, Supabase, JSZip

### 5.5 หน้าอ่านนิยายและโหมดแปล (Reading & Translator Mode UI - `/novels/[novel_id]/[chapter_number]`)

*   **การแสดงผลเนื้อหา:**
    *   ชื่อตอน (ตรงกลางบน)
    *   เนื้อหานิยาย (`chapters.content_original` เท่านั้น): ย่อหน้า, PC (Constrained Width), Mobile (Edge-to-edge), No Horizontal Scroll
    *   ปุ่ม "Arrow Up"
    *   Keyboard Controls (PC: Left/Right Arrow, Space Bar)
*   **โหมดการอ่าน (Member/Translator - ไม่ใช่โหมดนักแปล):**
    *   **Top Reading Toolbar (PC & Mobile):**
        *   PC: สารบัญ, ปรับแต่งการแสดงผล, บุ๊คมาร์ค, สลับธีม, โหมดอ่านต่อเนื่อง, นำทางตอน
        *   Mobile: เหมือน PC (อาจมีบางส่วนในเมนู "เพิ่มเติม")
    *   **Bottom Navigation Toolbar (Mobile - Tap to show/hide):** นำทางตอน
    *   **ท้ายบท:** ปุ่ม "ตอนต่อไป" (ถ้าโหมดอ่านต่อเนื่องปิด); Infinite Scroll (ถ้าเปิด)
*   **โหมดนักแปล (Translator Mode - เมื่อเปิดใช้งานสำหรับนิยายนี้):**
    *   **Top Reading Toolbar (PC & Mobile):** เหมือนโหมดอ่าน + ปุ่ม "คัดลอกมาร์คทั้งหมด" (สำหรับตอนปัจจุบัน)
    *   **การแสดงผลเนื้อหา:**
        *   แถบเลขบรรทัด (ซ้าย, สีปรับตาม Theme)
        *   เนื้อหาต้นฉบับ (`content_original`) (ถัดจากเลขบรรทัด, ย่อหน้า)
        *   Checkbox สำหรับมาร์ค (ขวาสุดของแต่ละบรรทัด)
    *   **MarkLine Interaction:**
        *   คลิก Checkbox: มาร์ค/ยกเลิกมาร์ค (Real-time update `mark_lines`, Toast)
        *   ดับเบิลคลิกบรรทัด: ไฮไลท์, คัดลอกข้อความต้นฉบับ, มาร์คอัตโนมัติ (Real-time update, Toast)
    *   **ท้ายบทในโหมดนักแปล:** สรุปข้อมูลมาร์ค, ปุ่ม "ล้างมาร์คทั้งหมดของตอนนี้", ปุ่ม "คัดลอกมาร์คทั้งหมดของตอนนี้"
*   **การพัฒนา:** Svelte store, JS/Svelte logic, Skeleton UI, Supabase

### 5.6 แดชบอร์ดผู้ดูแลระบบ (Admin Dashboard - `/admin/dashboard` และหน้าย่อย)

*   **โครงสร้าง:** Sidebar นำทางซ้าย, พื้นที่เนื้อหาหลัก
    *   Sidebar: ภาพรวม, จัดการผู้ใช้, จัดการหมวดหมู่, จัดการประกาศ, จัดการเว็บไซต์, จัดการเวอร์ชัน
*   **5.6.1 หน้าภาพรวมแดชบอร์ด:** แสดงข้อมูลสรุป (จำนวนนิยาย, ผู้ใช้)
*   **5.6.2 หน้าจัดการผู้ใช้ (`/admin/dashboard/users`):**
    *   ค้นหาผู้ใช้, ฟิลเตอร์
    *   ตารางผู้ใช้: #, Avatar, Email, ชื่อที่แสดง, สิทธิ์, วันที่สมัคร, วันหมดอายุสิทธิ์, ปุ่ม "จัดการ" (อาจนำไป `/admin/dashboard/users/[user_id]` หรือเปิด Modal)
    *   **Modal/Page จัดการผู้ใช้ (`/admin/dashboard/users/[user_id]`):** แก้ไข Avatar, ชื่อที่แสดง, สิทธิ์, วันหมดอายุสิทธิ์, ระงับบัญชี (Toast)
*   **5.6.3 หน้าจัดการหมวดหมู่นิยาย:** เพิ่ม/แก้ไข/ลบหมวดหมู่ (Modal), จัดการนิยายในหมวดเมื่อลบ
*   **5.6.4 หน้าจัดการประกาศ:** สร้าง/แก้ไข/ลบ/สลับสถานะประกาศ (Modal, ตาราง)
*   **5.6.5 หน้าจัดการเว็บไซต์ (`/admin/dashboard/site-settings`):**
    *   ฟอร์มตั้งค่า: ชื่อเว็บ, คำอธิบาย, โลโก้, Favicon, การลงทะเบียน, Google OAuth, จำนวนรายการต่อหน้า, ค่าเริ่มต้นสำหรับนิยายใหม่ (Toast)
*   **5.6.6 หน้าจัดการเวอร์ชัน (`/admin/dashboard/versions`):**
    *   Tab 1: เวอร์ชันเว็บไซต์/Changelog (เพิ่ม/แก้ไข/ลบ, Modal)
    *   Tab 2: เวอร์ชันนิยายและตอน (ค้นหานิยาย, ดูประวัติเวอร์ชันนิยาย/ตอน, ดูรายละเอียด, กู้คืน, Modal, Toast)
*   **การพัฒนา:** SvelteKit, Skeleton UI, Supabase

### 5.7 Modal สร้างนิยาย (Create Novel Modal)
*   (ตามข้อ 5.1.1 ทุกประการ)

### 5.8 การจัดการตอน (Chapter Management)
*   **5.8.1 Modal เพิ่มตอนด้วย .txt (Batch Upload Modal):** (ตามข้อ 5.2.2)
*   **5.8.2 Modal เพิ่ม/แก้ไขตอน (Create/Edit Chapter Modal):** (ตามข้อ 5.2.3)

### 5.9 หน้าโปรไฟล์ (Profile Page - `/profile`)

*   **การนำทาง:** จาก Dropdown ผู้ใช้ใน Navbar
*   **หัวเรื่อง:** "โปรไฟล์ของฉัน"
*   **ความสามารถ:**
    *   รูปโปรไฟล์ (Avatar): แสดง, เปลี่ยน (อัปโหลด, Crop 1:1, Toast)
    *   ชื่อที่แสดง (Display Name): แสดง, แก้ไข (Input, บันทึก, Toast)
    *   สิทธิ์ผู้ใช้งาน (Role): แสดง (Badge สีตามบทบาท, Read-only)
    *   อีเมล (Email): แสดง (Read-only)
    *   เปลี่ยนรหัสผ่าน: ปุ่ม -> Modal (รหัสผ่านปัจจุบัน, ใหม่, ยืนยันใหม่, บันทึก, Toast, บังคับ Logout)
    *   ออกจากระบบ: ปุ่ม -> Modal ยืนยัน (Toast, Redirect)
*   **การพัฒนา:** SvelteKit, Skeleton UI, Felte + Zod, Supabase, Cropper.js

## 6. Key User Experience Flows & Enhancements

*   การล็อกอิน, นำทาง, ประสบการณ์อ่าน, เครื่องมือช่วยแปล: ราบรื่น, ตอบสนองดี
*   การตอบสนองของระบบ: Empty/Loading States, Error Handling (Toast)
*   ออกแบบสวยงาม, Responsive, ปุ่ม Arrow Up
*   แจ้งเตือนการกระทำ (Toast)
*   สร้าง `mark_lines` อัตโนมัติเมื่อสร้างตอนใหม่
*   ทุกหน้าที่มี URL เฉพาะ เมื่อรีเฟรช (F5) ต้องอยู่หน้าเดิม แสดงข้อมูลเดิม

## 7. Database Structure (Supabase - PostgreSQL)

ตารางหลัก (ปรับปรุงตามข้อเสนอแนะ):

*   `roles` (id SERIAL PRIMARY KEY, name TEXT UNIQUE NOT NULL)

*   `profiles` (
    id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
    display_name TEXT,
    avatar_url TEXT,
    role_id INTEGER NOT NULL REFERENCES roles(id) DEFAULT (SELECT id FROM roles WHERE name='Member'),
    role_expiry_date TIMESTAMP WITH TIME ZONE NULL,
    is_suspended BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
    )

*   `categories` (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL UNIQUE,
    slug TEXT UNIQUE NOT NULL,
    description TEXT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
    )

*   `novels` (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    title TEXT NOT NULL,
    slug TEXT UNIQUE NOT NULL, -- Editable slug
    synopsis TEXT, -- Rich text content
    cover_image_url TEXT,
    category_id INTEGER REFERENCES categories(id) ON DELETE SET NULL,
    original_language TEXT NOT NULL, -- 'zh', 'en', 'th'
    status TEXT DEFAULT 'ongoing', -- 'ongoing', 'completed', 'halted'
    visibility JSONB DEFAULT '["everyone"]'::jsonb,
    default_chapter_status TEXT DEFAULT 'published',
    default_chapter_access JSONB DEFAULT '["everyone"]'::jsonb,
    created_by UUID NOT NULL REFERENCES profiles(id), -- Stores user's UUID
    total_chapters INTEGER DEFAULT 0, -- Denormalized
    last_chapter_updated_at TIMESTAMP WITH TIME ZONE, -- Denormalized
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
    )

*   `chapters` (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    novel_id UUID NOT NULL REFERENCES novels(id) ON DELETE CASCADE,
    chapter_number TEXT NOT NULL, -- Stores number as TEXT, potentially with padding (e.g., "001", "123")
    title TEXT NOT NULL,
    -- slug TEXT, -- Removed as per "ใช้แค่ chapter_number"
    content_original TEXT, -- Original language content (Rich Text or Plain Text from .txt)
    -- content_translated TEXT NULL, -- Removed as per "เอาแค่ที่อัพโหลดครับ ไม่มีเลือกอะไร"
    status TEXT DEFAULT 'published',
    access_level JSONB DEFAULT '["everyone"]'::jsonb,
    created_by UUID NOT NULL REFERENCES profiles(id),
    word_count INTEGER,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(novel_id, chapter_number)
    -- UNIQUE(novel_id, slug) -- Removed
    )
    -- CHECK constraint for chapter_number if specific format is required, e.g., CHECK (chapter_number ~ '^[0-9]+$')

*   `user_chapter_reads` (user_id UUID, chapter_id UUID, read_at TIMESTAMP, PRIMARY KEY (user_id, chapter_id))

*   `user_bookmarks` (id SERIAL, user_id UUID, chapter_id UUID, novel_id UUID, bookmarked_at TIMESTAMP, UNIQUE(user_id, chapter_id))

*   `mark_lines` (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
    chapter_id UUID NOT NULL REFERENCES chapters(id) ON DELETE CASCADE,
    -- Denormalized fields for easier readability and querying:
    user_email TEXT,
    user_display_name TEXT,
    novel_id UUID NOT NULL REFERENCES novels(id) ON DELETE CASCADE,
    novel_title TEXT,
    chapter_number TEXT, -- To match chapters.chapter_number type (TEXT)
    chapter_title TEXT,
    -- Core data:
    marked_line_numbers JSONB NOT NULL DEFAULT '[]'::jsonb,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(user_id, chapter_id)
    )
    *   **หมายเหตุ `mark_lines`:** การ denormalize ข้อมูล (`user_email`, `user_display_name`, `novel_title`, `chapter_title`, `chapter_number`) ต้องมีกลไก (Database Triggers หรือ Application-level logic) ในการอัปเดตเมื่อข้อมูลต้นทางเปลี่ยน เพื่อรักษาความสอดคล้อง

*   `announcements` (id SERIAL, title TEXT, content TEXT, start_date TIMESTAMP, end_date TIMESTAMP, target_roles JSONB, target_pages JSONB, is_active BOOLEAN, created_by UUID, created_at TIMESTAMP, updated_at TIMESTAMP)

*   `site_settings` (key TEXT PRIMARY KEY, value JSONB, description TEXT, created_at TIMESTAMP, updated_at TIMESTAMP)

*   `site_changelogs` (id SERIAL, version_number TEXT UNIQUE, release_date DATE, summary TEXT, is_published BOOLEAN, created_by UUID, created_at TIMESTAMP, updated_at TIMESTAMP)

*   `novel_versions` (id SERIAL, novel_id UUID, version_number INTEGER, metadata_snapshot JSONB, created_by UUID, notes TEXT, created_at TIMESTAMP, UNIQUE(novel_id, version_number))

*   `chapter_versions` (id SERIAL, chapter_id UUID, version_number INTEGER, content_original_snapshot TEXT, created_by UUID, notes TEXT, created_at TIMESTAMP, UNIQUE(chapter_id, version_number))

*   **การรักษาความปลอดภัย (RLS):** ใช้ RLS อย่างเข้มงวดในทุกตารางที่เกี่ยวข้องกับข้อมูลผู้ใช้และเนื้อหา
    *   เมื่อแสดงผลข้อมูล `created_by` (UUID) จะต้อง JOIN กับ `profiles` table เพื่อดึง `display_name` มาแสดง ไม่ใช่เก็บ `display_name` โดยตรงใน `novels` หรือ `chapters` (ยกเว้นส่วน denormalized ใน `mark_lines` ที่ต้องมีกลไกซิงค์)

## 8. Supabase Storage Buckets

*   `novel-covers`: (Public: true) รูปปกนิยาย
*   `chapter-text-files`: (Public: false, restricted) ไฟล์ .txt (temporary, ลบหลังประมวลผล)
*   `avatars`: (Public: true) รูปโปรไฟล์ผู้ใช้
*   `site_assets`: (Public: true) โลโก้เว็บ, favicon

## 9. สรุป Tech Stack (Tech Stack Summary)

*   **Framework:** SvelteKit
*   **Styling:** TailwindCSS
*   **UI Library:** Skeleton UI
*   **Form Handling & Validation:** Felte และ Zod
*   **Icon Library:** Lucide-Svelte
*   **Backend & Authentication:** Supabase Platform (PostgreSQL, Supabase Auth, Supabase Storage, Supabase Edge Functions)
*   **ภาษา:** TypeScript
*   **Rich Text Editor:** TipTap
*   **Image Cropping:** Cropper.js (หรือ Svelte-Cropper)
*   **ZIP file generation:** JSZip

## 10. การจัดการข้อผิดพลาดเบื้องต้น (Basic Error Handling)

*   Client-Side Validation (Zod + Felte)
*   Server-Side Validation (Zod ใน SvelteKit actions/Edge Functions)
*   User-Facing Error Messages (Toast, UI messages)
*   Code-Level Error Handling (try...catch, Supabase error objects)
*   Error Logging (console, Supabase logs, พิจารณา Sentry/Logtail)

## 11. มาตรการความปลอดภัยเบื้องต้น (Basic Security Measures)

*   **Authentication:** Supabase Auth, Email Verification, (พิจารณา MFA for Admin)
*   **Authorization (RLS):** **หัวใจสำคัญ**, กำหนด RLS policies เข้มงวดทุกตาราง, อ้างอิง `auth.uid()`, `auth.role()`, `profiles.role_id`
*   **Input Validation:** Zod (Client & Server), Sanitize HTML output (DOMPurify ถ้าใช้ `{@html ...}`)
*   **HTTPS:** เสมอ
*   **Environment Variables:** `PUBLIC_...` สำหรับ Client, `SERVICE_ROLE_KEY` **เฉพาะ Server-side และระมัดระวังสูงสุด**
*   **CSRF Protection:** SvelteKit มีให้
*   **HTTP Security Headers:** (CSP, etc.)
*   **Data Backups:** Supabase auto-backups, พิจารณา manual backups
*   **Principle of Least Privilege:** สิทธิ์เท่าที่จำเป็น, Admin ใช้บัญชีแยก
*   **Software Updates:** หมั่นอัปเดต dependencies

เอกสารนี้ได้รวมการแก้ไขทั้งหมดตามที่แจ้งมาแล้วครับ หากมีส่วนใดต้องการปรับเพิ่มเติมหรือมีคำถาม สามารถแจ้งได้เลยครับ!
