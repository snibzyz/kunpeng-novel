
# INKREALM.CO: ข้อกำหนดระบบและคุณสมบัติหลัก (V5.1) - ฉบับปรับปรุง README (Next.js & Supabase Stack with Hero UI & Font Awesome)

เอกสารนี้เป็นข้อกำหนดระบบและคุณสมบัติหลักสำหรับแพลตฟอร์ม INKREALM.CO (V5.1) โดยเน้นฟังก์ชันที่จำเป็นตามการปรับปรุงล่าสุด โดยอ้างอิง Tech Stack ใหม่ (Next.js (React), Supabase, **Hero UI**, **Font Awesome Icons**) และออกแบบมาเพื่อความสะดวกในการปรับใช้, ทดสอบ, และย้ายไปยังโดเมนต่างๆ



**ลำดับความสำคัญในการพิจารณาและพัฒนา (โดยรวม):**

1.  **ความปลอดภัยของระบบและข้อมูล (Security & Data Integrity):** การยืนยันตัวตน, การกำหนดสิทธิ์ (RLS), การป้องกันข้อมูลรั่วไหล, การตรวจสอบข้อมูลนำเข้า, การสำรองข้อมูล
2.  **โครงสร้างฐานข้อมูล (Database Structure):** ความถูกต้อง, ความครบถ้วน, ความสัมพันธ์ของข้อมูล, การรองรับการขยายตัว และการ query ที่มีประสิทธิภาพ (รวมถึงการ denormalize ที่จำเป็นอย่าง `mark_lines`)
3.  **ระบบหลัก (Core Systems):** ระบบยืนยันตัวตน, ระบบจัดการเนื้อหานิยาย (CMS), ระบบการอ่านและบุ๊คมาร์ค, ระบบเครื่องมือสำหรับนักแปล
4.  **ประสบการณ์ผู้ใช้ (User Experience - UX):** การใช้งานง่าย, ความต่อเนื่อง, การตอบสนองของระบบ, การจัดการข้อผิดพลาดที่เป็นมิตร
5.  **การออกแบบส่วนติดต่อผู้ใช้ (User Interface - UI):** ความสวยงาม, การอ่านง่าย, ความสอดคล้อง, Responsive Design
6.  **คุณสมบัติเสริมและส่วนอำนวยความสะดวก (Enhancements & Utilities):** ระบบประกาศ, แดชบอร์ด, การจัดการเว็บไซต์, การจัดการเวอร์ชัน

---

## 1. ปรัชญาการออกแบบโดยรวม

- **การแสดงผลหรูหราและอ่านง่าย:**
  - **โทนสีหลัก:** สีทอง (เช่น `#FFD700`, `#B8860B` หรือโทนใกล้เคียงที่ให้ความรู้สึกหรูหรา)
  - **สีรอง:** สีเทา (หลายเฉด เช่น `#F0F0F0`, `#CCCCCC`, `#808080`, `#333333`)
  - **โหมดสว่าง (Light Mode):** เน้นสีทอง, เทาอ่อน, และขาว เพื่อความสบายตา ดูสะอาด และหรูหรา
  - **โหมดมืด (Dark Mode):** เน้นสีทอง (อาจปรับให้สว่างขึ้นเล็กน้อยเพื่อคอนทราสต์), เทาเข้ม, และดำ เพื่อลดแสงสะท้อนและช่วยให้อ่านง่ายในที่มืด
  - **โลโก้:** พิจารณาใช้การไล่ระดับสี (gradient) ที่ผสมผสานสีทองและสีเทาอย่างลงตัว หรือสีทองล้วนบนพื้นหลังที่เหมาะสม เพื่อความโดดเด่นและทันสมัย
  - **ฟอนต์หลัก:** Sarabun สำหรับการแสดงผลภาษาไทยที่สวยงามและอ่านง่าย; พิจารณาฟอนต์สำรองสำหรับภาษาอังกฤษที่เข้ากัน (เช่น Noto Sans, Open Sans)
- **เครื่องมือสำหรับนักแปล:** มีฟังก์ชันสนับสนุนการทำงานของนักแปลโดยเฉพาะ เน้นความชัดเจนและประสิทธิภาพ
- **ใช้งานง่าย ตอบโจทย์ทุกอุปกรณ์:**
  - UI ใช้งานง่าย (Intuitive) และสอดคล้องกันทั้งระบบ
  - Responsive Design เพื่อประสบการณ์ที่ดีในทุกขนาดหน้าจอ (ใช้ Tailwind CSS utilities ร่วมกับ Hero UI)
  - หน้าอ่านนิยายไม่มี Horizontal Scroll โดยเด็ดขาด
- **ความยืดหยุ่นและการทดสอบ:** โครงสร้างที่เอื้อต่อการเปลี่ยนแปลงโดเมนและการทดสอบระบบที่ง่ายดาย
- **การเปลี่ยนธีม:**
  - การสลับระหว่างโหมดสว่างและโหมดมืดจะต้องราบรื่น (อาจมี transition CSS เบาๆ)
  - สีที่แสดงผลในแต่ละโหมดต้องไม่ผิดเพี้ยนจากที่ออกแบบไว้ และมีคอนทราสต์ที่เหมาะสมสำหรับการอ่าน
  - ควบคุมผ่านองค์ประกอบ UI ที่ชัดเจน (เช่น ไอคอนพระอาทิตย์/พระจันทร์ จาก **Font Awesome** ที่มีการเปลี่ยนแปลงสถานะชัดเจน เช่น `fa-sun` / `fa-moon`) และสถานะธีมต้องถูกบันทึกไว้ในการตั้งค่าของผู้ใช้ (localStorage และ/หรือ `profiles.user_settings` ใน Supabase) อาจใช้ Library เช่น `next-themes` เพื่อความสะดวก

## 1.1 ภาพรวมระบบหลัก (Core System Overview)

- **ระบบยืนยันตัวตนและจัดการผู้ใช้:** ลงทะเบียน, เข้าสู่ระบบ (อีเมล/รหัสผ่าน, Google OAuth ผ่าน Supabase Auth), ลืมรหัสผ่าน, จัดการโปรไฟล์ (รวมถึงการเปลี่ยนรูปโปรไฟล์, ชื่อที่แสดง, รหัสผ่าน), การจัดการบทบาทและสิทธิ์ (Admin ควบคุมผ่าน UI และ RLS ใน Supabase)
- **ระบบจัดการเนื้อหานิยาย (CMS):**
  - การสร้าง/แก้ไข/ลบนิยายและตอน (ดำเนินการผ่าน **Hero UI Modal** เป็นหลัก เพื่อ UX ที่ต่อเนื่อง)
  - การอัปโหลดเนื้อหา: รูปปกนิยาย (พร้อม crop ด้วย react-image-crop), ไฟล์ .txt สำหรับตอนนิยาย (Batch Upload ผ่าน Next.js API Route)
  - การจัดการหมวดหมู่นิยาย (สำหรับ Admin)
  - ระบบจัดการเวอร์ชันสำหรับข้อมูลเมตานิยายและเนื้อหาตอน
- **ระบบการอ่านและบุ๊คมาร์ค:**
  - หน้าอ่านที่ปรับแต่งการแสดงผลได้ (ขนาดฟอนต์, font-family เบื้องต้น, ระยะห่างบรรทัด)
  - ระบบบุ๊คมาร์คตอนที่ผู้ใช้อ่านค้างไว้
  - โหมดอ่านต่อเนื่อง (Infinite Scroll 구현ด้วย React Query หรือ SWR ร่วมกับ Intersection Observer API)
  - การสลับโหมดการอ่านสำหรับนักแปล (เปิด/ปิด "โหมดนักแปล" สำหรับนิยายนั้นๆ)
- **ระบบเครื่องมือสำหรับนักแปล:**
  - "โหมดนักแปล" (เมื่อเปิดใช้งานสำหรับนิยายที่กำหนด): แสดงเลขบรรทัด, checkbox สำหรับมาร์คบรรทัด
  - ระบบบรรทัดมาร์ค (Mark Lines): บันทึกบรรทัดที่นักแปลเลือก (**Real-time update** ไปยัง Supabase)
  - หน้าจัดการบรรทัดมาร์ค: ดูภาพรวม, คัดลอก, ดาวน์โหลดมาร์ค
- **ระบบแดชบอร์ด (สร้างด้วย Next.js Pages/Components):**
  - **สำหรับนักแปล:** ภาพรวมนิยายที่ตนเองจัดการ, ทางลัดไปหน้าจัดการบรรทัดมาร์ค
  - **สำหรับผู้ดูแลระบบ:** ภาพรวมระบบ, จัดการผู้ใช้, จัดการหมวดหมู่, จัดการประกาศ, จัดการเว็บไซต์ (การตั้งค่าทั่วไป), จัดการเวอร์ชัน (เว็บไซต์, นิยาย, ตอน)
- **ระบบฐานข้อมูลและไฟล์:** Supabase (PostgreSQL สำหรับฐานข้อมูล, Supabase Storage สำหรับไฟล์)
- **ระบบการแจ้งเตือนผู้ใช้ (User Feedback System):** การใช้ **Hero UI Alert** (สำหรับ Toast notifications) สำหรับยืนยันการกระทำ, แจ้งเตือนข้อผิดพลาด, หรือให้ข้อมูลสั้นๆ
- **ระบบประกาศ (Announcement System):** สำหรับ Admin ในการสร้างและเผยแพร่ประกาศไปยังกลุ่มผู้ใช้เป้าหมายและหน้าที่กำหนด

## 1.2 โครงสร้าง URL ที่เป็นมิตรและการจัดการโดเมน (User-Friendly URL Structure & Domain Management)

เพื่อให้ URL ของเว็บไซต์ INKREALM.CO อ่านง่าย, เป็นมิตรต่อผู้ใช้, ดีต่อ SEO, และง่ายต่อการย้ายโดเมน (ใช้ Next.js File-System Routing):

- **โครงสร้าง URL หลัก (ตัวอย่างการจัดวางใน `pages` directory ของ Next.js):**
  - หน้ารวมนิยาย (หน้าแรก): `/` (ไฟล์ `pages/index.tsx`)
  - หน้าข้อมูลนิยาย: `/novels/[novel_id]` (ไฟล์ `pages/novels/[novel_id].tsx`, `novel_id` คือ UUID)
  - หน้าอ่านตอนนิยาย: `/novels/[novel_id]/[chapter_number]` (ไฟล์ `pages/novels/[novel_id]/[chapter_number].tsx`, `chapter_number` คือหมายเลขตอน อาจมี padding เช่น `001`, `123`)
  - หน้าโปรไฟล์ผู้ใช้ (สำหรับผู้ใช้ปัจจุบัน): `/profile` (ไฟล์ `pages/profile.tsx`)
  - หน้าโปรไฟล์ผู้ใช้ (สำหรับ Admin ดูผู้ใช้อื่น): `/admin/dashboard/users/[user_id]` (ไฟล์ `pages/admin/dashboard/users/[user_id].tsx`)
  - หน้าบุ๊คมาร์ค: `/bookmarks` (ไฟล์ `pages/bookmarks.tsx`)
  - แดชบอร์ดนักแปล: `/translator/dashboard` (ไฟล์ `pages/translator/dashboard.tsx`)
  - หน้าจัดการบรรทัดมาร์ค (นักแปล): `/translator/manage-marks/[novel_id]` (ไฟล์ `pages/translator/manage-marks/[novel_id].tsx`)
  - แดชบอร์ดผู้ดูแลระบบ: `/admin/dashboard` (ไฟล์ `pages/admin/dashboard/index.tsx` หรือ `_layout.tsx` สำหรับ layout)
  - หน้าย่อยในแดชบอร์ดผู้ดูแลระบบ: `/admin/dashboard/users` (ไฟล์ `pages/admin/dashboard/users/index.tsx`), `/admin/dashboard/categories` (ไฟล์ `pages/admin/dashboard/categories.tsx`), etc.
- **การสร้าง Slug สำหรับนิยาย (`novel_slug`):**
  - `novel_slug` จะถูกเก็บในฐานข้อมูล `novels` table
  - ถูกสร้างขึ้นโดยอัตโนมัติเมื่อสร้างนิยายใหม่ โดยแปลงชื่อเรื่องเป็นตัวพิมพ์เล็ก, แทนที่ช่องว่างและอักขระพิเศษที่ไม่ปลอดภัยสำหรับ URL ด้วยขีดกลาง (`-`), และตัดอักขระพิเศษอื่นๆ ออก (Logic นี้สามารถทำใน Next.js API Route หรือ Client-side ก่อนส่ง)
  - ผู้สร้าง/Admin สามารถแก้ไข `novel_slug` นี้ได้ในหน้าแก้ไขข้อมูลนิยาย (ต้องมีการตรวจสอบความซ้ำซ้อนของ `novel_slug` ในระบบก่อนบันทึก ผ่าน API call)
  - แม้ URL หลักจะใช้ `novel_id` (UUID) เพื่อความ Unique และง่ายในการ Query, `novel_slug` ยังคงมีประโยชน์สำหรับ SEO (เช่น การสร้าง sitemap ที่มี URL อ่านง่าย) หรือการอ้างอิงภายในที่สื่อความหมายกว่า
- **สำหรับตอนนิยาย:** จะใช้ `chapter_number` (ที่อาจมี padding และเก็บเป็น TEXT ใน DB) โดยตรงใน URL
- **การจัดการโดเมนและ Base URL:**
  - **Environment Variables (Next.js):** การตั้งค่า Base URL ของเว็บไซต์ (เช่น `https://inkrealm.co` หรือ `http://localhost:3000` สำหรับการพัฒนา) จะถูกกำหนดผ่าน `NEXT_PUBLIC_BASE_URL` ในไฟล์ `.env.local` (หรือเทียบเท่าตาม Environment)
  - **การสร้างลิงก์ภายใน (Internal Linking):** ใช้ Component `<Link>` จาก `next/link` เป็นหลัก ซึ่งจะจัดการ Client-side navigation และ prefetching อย่างเหมาะสม ทุกลิงก์ภายในเว็บไซต์จะถูกสร้างโดยอ้างอิงจาก Relative Paths หรือ Pathnames ที่ตรงกับโครงสร้างใน `pages` directory
  - **การตั้งค่า Supabase:** `NEXT_PUBLIC_SUPABASE_URL` และ `NEXT_PUBLIC_SUPABASE_ANON_KEY` จะถูกเก็บไว้ใน Environment Variables (`.env.local`) และเข้าถึงได้ทั้ง Client-side และ Server-side (เช่น ใน `getServerSideProps` หรือ API Routes)

## 2. บทบาทผู้ใช้และสิทธิ์ (User Roles & Permissions)

ระบบแบ่งบทบาทผู้ใช้ออกเป็น: สมาชิก (Member), นักแปล (Translator), และ ผู้ดูแลระบบ (Admin) โดยแต่ละบทบาทมีสิทธิ์การเข้าถึงและจัดการข้อมูลแตกต่างกันอย่างชัดเจน (ควบคุมโดย Supabase Row-Level Security - RLS เป็นหลัก และเสริมด้วย logic ใน Next.js Components, `getServerSideProps`, หรือ API Routes ตามความเหมาะสม)

- **สมาชิก (Member):**
  - **ความสามารถหลัก:** อ่านนิยาย (ตามสิทธิ์การเข้าถึงของนิยาย/ตอนที่กำหนดใน Supabase และตรวจสอบผ่าน RLS), บุ๊คมาร์คตอน, แก้ไขโปรไฟล์ส่วนตัว (ชื่อที่แสดง, รูปโปรไฟล์, รหัสผ่าน)
  - **แถบเครื่องมือสำหรับอ่าน (Reading Toolbar Component):** สารบัญ (เปิด **Hero UI Modal** แสดงรายการตอน), ปรับแต่งการแสดงผล (เปิด **Hero UI Popover/Dropdown**), บุ๊คมาร์คตอนปัจจุบัน (สลับสถานะ), สลับธีมเว็บ (Light/Dark), เปิด/ปิดโหมดอ่านต่อเนื่อง, นำทางตอนก่อนหน้า/ถัดไป
- **นักแปล (Translator):**
  - **ความสามารถหลัก:**
    - สิทธิ์ทั้งหมดของสมาชิก
    - สามารถเปิด/ปิด "โหมดนักแปล" สำหรับนิยายที่ตนเองได้รับสิทธิ์แปล (สถานะนี้อาจเก็บใน localStorage หรือ `user_preferences` table และใช้ในการ render UI ที่ต่างกันในหน้าอ่าน)
    - เข้าถึง "โหมดแปล" (เมื่อเปิดใช้งานสำหรับนิยายนั้น): แสดงเลขบรรทัด, **Hero UI Checkbox**, ดับเบิลคลิกที่บรรทัดเนื้อหาเพื่อมาร์คและคัดลอก
    - ใช้งานระบบบรรทัดมาร์ค (Mark Lines) และบันทึกข้อมูลไปยัง Supabase ทันที (**Real-time update**)
    - สร้าง/แก้ไข/ลบนิยายและตอนของตนเอง (นิยายที่ `created_by` ตรงกับ `user_id` ของตน หรือได้รับสิทธิ์จัดการผ่านตาราง permissions แยกต่างหาก ถ้ามี) การดำเนินการผ่าน **Hero UI Modal**
    - เข้าถึงแดชบอร์ดนักแปล (`/translator/dashboard`) และหน้าจัดการบรรทัดมาร์คสำหรับนิยายของตน
  - **แถบเครื่องมือพิเศษในโหมดแปล (เมื่อเปิด "โหมดนักแปล" สำหรับนิยายนั้น และเข้าหน้าอ่านตอน):** เหมือนของ Member แต่เพิ่ม: ปุ่ม "คัดลอกมาร์คทั้งหมด" (Copy All Marks) สำหรับตอนปัจจุบัน (ใช้ **Font Awesome icon** เช่น `fa-copy`)
- **ผู้ดูแลระบบ (Admin):**
  - **ความสามารถหลัก:**
    - สิทธิ์ทั้งหมดของนักแปล (รวมถึงการสร้าง/จัดการนิยายและตอนใดๆ ในระบบ โดย RLS จะอนุญาตถ้า role เป็น Admin)
    - เข้าถึงแดชบอร์ดผู้ดูแลระบบ (`/admin/dashboard`) ซึ่งมี Layout และการป้องกัน Route เฉพาะ Admin
    - จัดการผู้ใช้ทั้งหมด: ดูรายการ, เข้าดู/แก้ไขโปรไฟล์ (ชื่อที่แสดง, รูปโปรไฟล์), เปลี่ยนบทบาท, กำหนดวันหมดอายุสิทธิ์, ระงับบัญชี (ผ่านหน้า User Management `/admin/dashboard/users` และหน้าย่อย `/admin/dashboard/users/[user_id]` หรือ **Hero UI Modal**)
    - ดูข้อมูลบรรทัดมาร์คทั้งหมดของนักแปลทุกคน (อาจมีหน้าเฉพาะสำหรับ Admin หรือส่วนขยายในหน้าจัดการบรรทัดมาร์ค)
    - การจัดการหมวดหมู่นิยาย: เพิ่ม/แก้ไข/ลบหมวดหมู่
    - การจัดการประกาศ: สร้าง/แก้ไข/ลบ/เผยแพร่ประกาศ
    - การจัดการเว็บไซต์: แก้ไขการตั้งค่าทั่วไปของเว็บไซต์ (ผ่านหน้า Site Management)
    - การจัดการเวอร์ชัน: ดูและกู้คืนเวอร์ชันของเว็บไซต์ (Changelog), นิยาย (metadata), และตอน (content)

## 3. การเข้าสู่ระบบ / ลงทะเบียน / ลืมรหัสผ่าน (Authentication Modals & Pages)

- **ความสามารถ:** ลงทะเบียนผู้ใช้ใหม่, เข้าสู่ระบบด้วยอีเมล/รหัสผ่าน หรือ Google OAuth (ใช้ Supabase Auth UI Helpers หรือสร้าง Form เอง), กระบวนการลืมและรีเซ็ตรหัสผ่าน (Supabase จัดการการส่งอีเมล)
- **การแสดงผล:**
  - แสดงผลผ่าน **Modal** (**Hero UI Modal**) เป็นหลักเมื่อผู้ใช้คลิกปุ่ม "เข้าสู่ระบบ" หรือ "ลงทะเบียน" จากภายใน Navbar หรือส่วนอื่นๆ ของเว็บที่ผู้ใช้ยังไม่ได้ล็อกอิน
  - มีหน้าเว็บเฉพาะ (เช่น `pages/login.tsx`, `pages/register.tsx`, `pages/forgot-password.tsx`, `pages/reset-password.tsx`) สำหรับการเข้าถึงโดยตรง (เช่น จากลิงก์ในอีเมลรีเซ็ตรหัสผ่าน หรือหากผู้ใช้พิมพ์ URL โดยตรง)
- **การพัฒนา:**
  - **Backend:** Supabase Auth (จัดการ user accounts, sessions, social logins, password reset emails)
  - **Frontend (Next.js):**
    - สร้าง Forms ด้วย React Components และ **Hero UI (Input, Button, etc.)**
    - จัดการ Form State และ Validation ด้วย **React Hook Form** และ **Zod**
    - เรียกใช้ Supabase Auth client methods (`signInWithPassword`, `signUp`, `signInWithOAuth`, `resetPasswordForEmail`) จาก Event Handlers ใน Components
    - จัดการสถานะการโหลด (Loading state) และแสดงข้อผิดพลาดจาก Supabase Auth
    - อาจใช้ React Context API หรือ Zustand เพื่อจัดการ Global Authentication State (เช่น ข้อมูลผู้ใช้, สถานะการล็อกอิน) เพื่อให้ Components อื่นๆ เข้าถึงได้ง่าย
- **การสร้าง/อัปเดตโปรไฟล์ผู้ใช้โดยอัตโนมัติ:**
  - **เมื่อผู้ใช้ลงทะเบียนใหม่ หรือเข้าสู่ระบบครั้งแรก และยังไม่มีข้อมูลในตาราง `profiles` ที่เชื่อมโยงกับ `auth.users.id` ของพวกเขา ระบบจะสร้าง record ใหม่ในตาราง `profiles` โดยอัตโนมัติ**
  - การดำเนินการนี้สามารถทำได้ผ่าน Supabase Database Trigger บนตาราง `auth.users` (ON INSERT) หรือผ่าน Supabase Edge Function ที่ subscribe กับ auth events (เช่น `onSignUp`, `onSignIn`)
  - Record ที่สร้างใหม่ใน `profiles` จะมีค่าเริ่มต้นสำหรับ `role_id` (เช่น 'Member') และ `id` ที่อ้างอิง `auth.users.id`
  - การทำเช่นนี้ช่วยให้ผู้ดูแลระบบสามารถจัดการบทบาท (role) ของผู้ใช้ได้ทันทีผ่าน Admin Dashboard หลังจากผู้ใช้ลงทะเบียน/เข้าสู่ระบบครั้งแรก โดยข้อมูล `profiles` จะถูกสร้างและพร้อมใช้งานในฐานข้อมูลแบบ real-time
- **การแจ้งเตือน (User Feedback):** แสดง **Hero UI Alert (Toast notification)** (เช่น "เข้าสู่ระบบสำเร็จ", "ลงทะเบียนเรียบร้อย กรุณายืนยันอีเมล", "คำขอรีเซ็ตรหัสผ่านถูกส่งไปยังอีเมลของคุณแล้ว", "รหัสผ่านถูกเปลี่ยนสำเร็จ กรุณาเข้าสู่ระบบใหม่")

## 4. โครงสร้างแถบนำทาง (Navbar Structure)

- **ความสามารถ:** แสดงเมนูแบบไดนามิกตามบทบาทของผู้ใช้ (Guest, Member, Translator, Admin), Responsive Design (ปรับการแสดงผลสำหรับ Mobile เช่น ใช้ **Hero UI Navbar component** หรือ **Hero UI Drawer** สำหรับเมนูแบบ Hamburger)
- **ตัวอย่างเมนู (สร้างเป็น React Component):**
  - **Guest:**
    - โลโก้ INKREALM.CO (ใช้ `<Link href="/">` จาก `next/link`)
    - (อาจมีลิงก์ "นิยายทั้งหมด" หรือ "หมวดหมู่")
    - ปุ่มสลับธีม (ไอคอน Sun/Moon จาก **Font Awesome** เช่น `fa-sun`/`fa-moon`)
    - ปุ่ม "เข้าสู่ระบบ" (**Hero UI Button**, เปิด Login Modal)
    - ปุ่ม "ลงทะเบียน" (**Hero UI Button**, เปิด Register Modal)
  - **Member:**
    - โลโก้ INKREALM.CO
    - `<Link href="/">หน้าแรก</Link>`
    - `<Link href="/bookmarks">บุ๊คมาร์ค</Link>`
    - ปุ่มสลับธีม
    - โปรไฟล์ผู้ใช้ (**Hero UI Dropdown** หรือ **Hero UI Avatar** + **Hero UI Popover** เมื่อคลิกที่ Avatar/ชื่อผู้ใช้):
      - แสดงชื่อที่แสดง (Display Name) และบทบาท (เช่น "สมาชิก")
      - `<Link href="/profile">หน้าโปรไฟล์</Link>`
      - ปุ่ม "ออกจากระบบ" (เรียก Supabase `signOut()`)
  - **Translator:** เหมือน Member แต่เพิ่มเมนู (ใน Navbar หรือใน Dropdown โปรไฟล์):
    - `<Link href="/translator/dashboard">แดชบอร์ดนักแปล</Link>`
  - **Admin:** เหมือน Translator แต่เพิ่มเมนู (ใน Navbar หรือใน Dropdown โปรไฟล์):
    - `<Link href="/admin/dashboard">แดชบอร์ดผู้ดูแลระบบ</Link>`
- **การพัฒนา:**
  - สร้าง Navbar เป็น React Component
  - ใช้ `<Link>` จาก `next/link` สำหรับการนำทาง
  - ใช้ **Hero UI Components (Navbar, Menu, Dropdown, Button, Avatar, Drawer)** หรือสร้าง UI เองด้วย Tailwind CSS
  - ดึงข้อมูลสถานะผู้ใช้และบทบาทจาก Global State (เช่น Zustand store หรือ React Context ที่อัปเดตหลังจาก Login/Logout) เพื่อแสดงเมนูที่ถูกต้อง

## 5. รายละเอียดหน้าเว็บไซต์, Modals, และการจัดการเนื้อหา

### 5.1 หน้าแรก (Home Page - `pages/index.tsx`)

- **ส่วนแสดงประกาศ (Announcements Display Area - React Component):**
  - ดึงข้อมูลประกาศจาก Supabase (อาจใช้ React Query หรือ `getServerSideProps` ใน Next.js)
  - กรองประกาศที่ `is_active = true`, อยู่ในช่วง `start_date` ถึง `end_date` (ถ้ามี), `target_pages` รวมถึง "หน้าแรก", และ `target_roles` ตรงกับบทบาทของผู้ใช้ปัจจุบัน (Logic การกรองอาจทำใน Client หรือ Server)
  - แสดงผลในบริเวณที่เห็นได้ชัดเจน (เช่น Banner ด้านบน) ด้วย **Hero UI Alert** หรือ **Hero UI Card**, มีปุ่ม "X" (**Font Awesome icon** เช่น `fa-times`) ให้ผู้ใช้ปิดประกาศ (สถานะการปิดอาจบันทึกใน `localStorage`)
- **ส่วนเครื่องมือหลัก (React Component):**
  - **ช่องค้นหานิยาย:** (**Hero UI Input**) สำหรับกรอกคำค้น (ชื่อเรื่อง, คำโปรย) การค้นหาอาจทำแบบ real-time (debounced) หรือ on-submit โดยเรียก API Route
  - **ฟิลเตอร์หมวดหมู่ (Category Filter):** (**Hero UI Select**) แสดงรายการหมวดหมู่ทั้งหมด (ดึงจากตาราง `categories` แบบไดนามิก, มีตัวเลือก "ทุกหมวดหมู่")
  - **ฟิลเตอร์สถานะนิยาย (Novel Status Filter):** (**Hero UI Select**) เลือกสถานะ ("ต่อเนื่อง", "จบแล้ว", "ยุติการลง", "ทุกสถานะ")
  - **ปุ่มสร้างนิยาย (Create Novel Button):** (สำหรับ Translator/Admin เท่านั้น) (**Hero UI Button**) ประกอบด้วย [**Font Awesome icon** เช่น `fa-plus-square` หรือ `fa-file-plus`] + ข้อความ "สร้างนิยาย". เมื่อคลิก เปิด Modal สร้างนิยาย (ดูข้อ 5.1.1)
- **การแสดงรายการนิยาย (React Component):**
  - รูปแบบ Grid Layout (ใช้ Tailwind CSS grid)
  - **การ์ดนิยาย (**Hero UI Card**):** แสดง: **รูปปก (อัตราส่วน 3:4, ใช้ `next/image` เพื่อ optimization), ชื่อเรื่อง, จำนวนตอนทั้งหมด**
  - Responsive: PC (เช่น 4-5 คอลัมน์), Tablet (3 คอลัมน์), Mobile (1-2 คอลัมน์)
  - ใช้ `<Link href={`/novels/${novel.id}`}>` ครอบการ์ดเพื่อนำทาง
- **ระบบแบ่งหน้า (Pagination):** (**Hero UI Pagination**) แสดงเมื่อผลลัพธ์การค้นหา/ฟิลเตอร์มีจำนวนมากกว่าที่กำหนดต่อหน้า (เช่น 20 รายการต่อหน้า) การเปลี่ยนหน้าจะอัปเดต Query Parameters และเรียกข้อมูลใหม่
- **การพัฒนา:**
  - **Data Fetching:** ใช้ React Query (TanStack Query) สำหรับ Client-side data fetching และ caching หรือ `getServerSideProps` / `getStaticProps` (ถ้าเนื้อหาไม่เปลี่ยนแปลงบ่อย) เพื่อดึงข้อมูลนิยายจาก Supabase
  - **UI Components:** **Hero UI (Input, Select, Button, Card, Pagination, Alert)**, Tailwind CSS
  - **State Management:** Zustand หรือ React Context สำหรับจัดการสถานะฟิลเตอร์, คำค้น, หน้าปัจจุบันของ Pagination

### 5.1.1 Modal สร้างนิยาย (Create Novel Modal)

- **การเรียกใช้งาน:** คลิกปุ่ม "[ไอคอน] สร้างนิยาย" บนหน้าแรก (สำหรับ Translator/Admin)
- **UI ภายใน Modal (**Hero UI Modal**):**
  - **หัวเรื่อง Modal:** "สร้างนิยายใหม่"
  - **ฟอร์ม (จัดการด้วย React Hook Form, Validation ด้วย Zod):**
    - **ชื่อเรื่อง (Title):** (**Hero UI Input**), จำเป็นต้องกรอก (Zod: `required`, `minlength`)
    - **คำโปรย (Synopsis):** **Rich Text Editor** (เช่น TipTap ผูกกับ Controller ของ React Hook Form), (Zod: `optional`, `maxlength`)
    - **หมวดหมู่ (Category):** (**Hero UI Select**) เลือกจากรายการหมวดหมู่ (ดึงจาก `categories` table), (Zod: `optional` หรือ `required`)
    - **รูปปกนิยาย (Cover Image):** Input type file, แสดง Preview, มีเครื่องมือ Crop รูปภาพให้เป็นอัตราส่วน 3:4 (เช่น ใช้ `react-image-crop`), จัดการขนาดไฟล์ Client-side (ไม่เกิน 3MB)
    - **ภาษาต้นฉบับ (Original Language):** (**Hero UI Select**) เลือก ("จีน (zh)", "อังกฤษ (en)", "ไทย (th)"), (Zod: `required`)
    - **Slug นิยาย (`novel_slug`):** (**Hero UI Input**), auto-generate จากชื่อเรื่อง (Client-side logic), ผู้ใช้สามารถแก้ไขได้ (ต้องมีการตรวจสอบความซ้ำซ้อนของ `novel_slug` ผ่าน API call ก่อน Submit)
    - **สถานะนิยาย (Novel Status):** (**Hero UI Select** หรือ **Hero UI Radio** group) เลือกจาก ("ต่อเนื่อง", "จบแล้ว", "ยุติการลง"), ค่าเริ่มต้น "ต่อเนื่อง"
    - **สิทธิ์ในการมองเห็นนิยาย (Novel Visibility):** (**Hero UI Select (multiple)** หรือ **Hero UI Checkbox** Group) เลือกได้หลายอัน ("ทุกคน", "เฉพาะสมาชิก", "เฉพาะนักแปล"), ค่าเริ่มต้น "ทุกคน" (Zod: `array`, `min(1)`)
    - **การตั้งค่าเริ่มต้นสำหรับตอนใหม่ของนิยายนี้ (Default Chapter Settings for this Novel):**
      - **สถานะเริ่มต้นของตอน:** (**Hero UI Radio** group) ("เผยแพร่" (ค่าเริ่มต้น), "ฉบับร่าง")
      - **สิทธิ์การเข้าถึงตอนเริ่มต้น:** (**Hero UI Select (multiple)** หรือ **Hero UI Checkbox** Group) ("ทุกคน" (ค่าเริ่มต้น), "เฉพาะสมาชิก", "เฉพาะนักแปล")
  - **ปุ่มดำเนินการ (**Hero UI Button**):**
    - "เผยแพร่นิยาย": Submit form, เรียก API Route เพื่อสร้างนิยายและตั้งสถานะ (เมื่อสำเร็จ แสดง **Hero UI Alert (Toast)** "สร้างนิยาย '[ชื่อนิยาย]' และเผยแพร่สำเร็จแล้ว")
    - "บันทึกเป็นฉบับร่าง": Submit form, เรียก API Route เพื่อสร้างนิยายและตั้งสถานะเป็น 'draft' (เมื่อสำเร็จ แสดง **Hero UI Alert (Toast)** "สร้างนิยาย '[ชื่อนิยาย]' เป็นฉบับร่างสำเร็จแล้ว")
    - "ยกเลิก" (ปิด Modal)
- **การพัฒนา:**
  - **UI:** **Hero UI (Modal, Input, Select, Radio, Checkbox, Button)**
  - **Form:** React Hook Form, Zod (Schema สำหรับ validation)
  - **Rich Text:** TipTap
  - **Image Crop:** `react-image-crop`
  - **Backend Logic:** สร้าง Next.js API Route (`pages/api/novels/create.ts` หรือคล้ายกัน) เพื่อรับข้อมูลจาก Form, validate อีกครั้งด้วย Zod, สร้าง record ใน `novels` table (Supabase), อัปโหลดไฟล์ปกไป Supabase Storage, และสร้าง `novel_versions` record แรก

### 5.2 หน้าข้อมูลนิยายและสารบัญ (Novel Detail Page - `pages/novels/[novel_id].tsx`)

- **Data Fetching:** ใช้ `getServerSideProps` หรือ `getStaticProps` (ถ้า `novel_id` ทำให้ generate static paths ได้) เพื่อดึงข้อมูลนิยาย (`novels` table) และรายการตอน (`chapters` table) จาก Supabase ตาม `novel_id` ที่ได้จาก URL parameter หรือใช้ React Query สำหรับ Client-side fetching
- **ส่วนแสดงประกาศ (Announcements Display Area):** (เหมือนหน้าแรก, กรองตามเงื่อนไข)
- **ส่วนข้อมูลนิยาย (Novel Information Section - React Component):**
  - **ข้อมูลที่แสดง:** รูปปก (`next/image`), ชื่อเรื่อง, หมวดหมู่, คำโปรย (Render HTML จาก Rich Text Editor ด้วย `dangerouslySetInnerHTML` หลัง Sanitize ด้วย DOMPurify หรือใช้ library ที่ render Markdown/HTML ปลอดภัย), จำนวนตอนทั้งหมด (จาก `chapters` data), สถานะนิยาย, ภาษาต้นฉบับ, `novel_slug` (แสดง, ไม่ใช่สำหรับแก้ไขตรงนี้)
  - **(สำหรับนักแปล/Admin ที่มีสิทธิ์ในนิยายเรื่องนี้) ปุ่มสลับ "โหมดนักแปล" (Translator Mode Toggle):**
    - (**Hero UI Toggle/Switch**) ชัดเจน เช่น "โหมดนักแปล: ปิด/เปิด"
    - สถานะของสวิตช์นี้ (เปิด/ปิด) จะถูกบันทึกไว้สำหรับนักแปลคนนั้นกับนิยายเรื่องนั้นโดยเฉพาะ (อาจเก็บใน `localStorage` หรือ `user_preferences` table ผ่าน API call)
- **ปุ่มจัดการนิยาย (สำหรับผู้สร้างนิยาย (`created_by`) / Admin - แสดงตามเงื่อนไข):**
  - ปุ่ม "แก้ไขข้อมูลนิยาย" (**Hero UI Button**): เปิด Modal แก้ไขข้อมูลนิยาย (ดูข้อ 5.2.1)
  - ปุ่ม "ลบนิยาย" (**Hero UI Button**, destructive variant): แสดง **Hero UI Modal (Alert variant)** ยืนยัน ก่อนเรียก API Route เพื่อลบ (เมื่อลบสำเร็จ แสดง **Hero UI Alert (Toast)** "ลบนิยาย '[ชื่อนิยาย]' เรียบร้อยแล้ว" และ redirect ด้วย `next/router`)
- **ส่วนเพิ่มตอน (Add Chapter Section - สำหรับผู้สร้างนิยาย / Admin - แสดงตามเงื่อนไข):**
  - ปุ่ม: "เพิ่มตอนด้วย .txt" (เปิด Modal เพิ่มตอนด้วย .txt - ดูข้อ 5.2.2)
  - ปุ่ม: "เพิ่มตอน" (เปิด Modal เพิ่ม/แก้ไขตอน - ดูข้อ 5.2.3, สำหรับสร้างตอนใหม่)
- **ส่วนสารบัญ (Table of Contents Section - React Component):**
  - **การแสดงผล:** (**Hero UI Collapse (Accordion)**) หรือ List แบบ Collapse/Expand เอง, ปรับจำนวนตอนต่อกลุ่มที่แสดงได้ (เช่น Dropdown เลือก 10, 20, 50, 100 ตอน)
  - **รายการตอน (แต่ละตอนใน Collapse Item หรือ List Item):**
    - **(สำหรับทุกคน):**
      - ไอคอนบุ๊คมาร์ค (**Font Awesome**, แสดงสถานะว่าบุ๊คมาร์คตอนนี้ไว้หรือไม่, เช่น `fa-bookmark` / `far fa-bookmark`)
      - หมายเลขตอน (`chapter_number`), ชื่อตอน (คลิก `<Link href={`/novels/${novelId}/${chapter.chapter_number}`}>` เพื่อไปหน้าอ่าน)
      - สถานะการอ่าน (ถ้าเคยอ่านแล้ว): แสดงไอคอนรูปตา (**Font Awesome** เช่น `fa-eye`) หรือ UI อื่นๆ (ข้อมูลจาก `user_chapter_reads`)
    - **(สำหรับผู้สร้างนิยาย / Admin เท่านั้น, เพิ่มเติม):**
      - ไอคอนแสดงสถานะตอน (เช่น "เผยแพร่", "ฉบับร่าง")
      - ปุ่ม "แก้ไขตอน" (ไอคอนดินสอ **Font Awesome** เช่น `fa-pencil-alt`): เปิด Modal เพิ่ม/แก้ไขตอน (ข้อ 5.2.3) โดยโหลดข้อมูลของตอนนี้มา
      - ปุ่ม "ลบตอน" (ไอคอนถังขยะ **Font Awesome** เช่น `fa-trash-alt`): แสดง Modal ยืนยัน, เรียก API Route เพื่อลบตอน, Toast, รีเฟรชสารบัญ (React Query invalidate query)
      - **ปุ่ม "คัดลอกมาร์คทั้งหมด" (Copy All Marks):** (ไอคอน Copy **Font Awesome** เช่น `fa-copy`) สำหรับตอนนี้ (หากนักแปลเป็นเจ้าของและมีมาร์คใน `mark_lines` สำหรับตอนนี้)
- **การพัฒนา:** Next.js (Dynamic Routes), **Hero UI (Toggle, Button, Collapse, Modal)**, **Font Awesome Icons**, Supabase, React Query (สำหรับ Client-side updates และ refetching), DOMPurify (ถ้า render HTML)

### 5.2.1 Modal แก้ไขข้อมูลนิยาย (Edit Novel Modal)

- **การเรียกใช้งาน:** คลิกปุ่ม "แก้ไขข้อมูลนิยาย" ในหน้าข้อมูลนิยาย
- **UI ภายใน Modal (**Hero UI Modal**):** ฟอร์ม (React Hook Form + Zod) จะถูกเติมด้วยข้อมูลปัจจุบันของนิยาย (ดึงข้อมูลนิยายเมื่อ Modal เปิด)
  - **หัวเรื่อง Modal:** "แก้ไขข้อมูลนิยาย: [ชื่อนิยายปัจจุบัน]"
  - **ฟิลด์ที่สามารถแก้ไขได้ (เหมือน 5.1.1 แต่สำหรับแก้ไข):**
    - ชื่อเรื่อง (Title)
    - คำโปรย (Synopsis - Rich Text Editor)
    - หมวดหมู่ (Category - **Hero UI Select**)
    - รูปปกนิยาย (Cover Image - อัปโหลดใหม่, Crop 3:4)
    - ภาษาต้นฉบับ (Original Language - **Hero UI Select**: zh, en, th)
    - **Slug นิยาย (`novel_slug`)**: (**Hero UI Input**) สามารถแก้ไขได้ (ต้องมี real-time validation check ความซ้ำซ้อนผ่าน API call หรือ on blur)
    - สถานะนิยาย (Novel Status - **Hero UI Select/Radio** group: "ต่อเนื่อง", "จบแล้ว", "ยุติการลง")
    - สิทธิ์ในการมองเห็นนิยาย (Novel Visibility - **Hero UI Select (multiple)/Checkbox** Group)
  - **ค่าเริ่มต้นสำหรับการสร้างตอนใหม่ของนิยายนี้ (Default Chapter Settings for this Novel - เหมือนตอนสร้าง):**
    - สถานะเริ่มต้นของตอน (Default Chapter Status - **Hero UI Radio** group)
    - สิทธิ์การเข้าถึงตอนเริ่มต้น (Default Chapter Access - **Hero UI Select (multiple)/Checkbox** Group)
  - **ปุ่มดำเนินการ (**Hero UI Button**):**
    - "บันทึกการเปลี่ยนแปลง": Submit form, เรียก API Route เพื่ออัปเดตข้อมูลใน `novels` table (เมื่อสำเร็จ แสดง **Hero UI Alert (Toast)** "บันทึกข้อมูลนิยาย '[ชื่อนิยาย]' สำเร็จแล้ว", และสร้างรายการใหม่ใน `novel_versions` หากมีการเปลี่ยนแปลงข้อมูลสำคัญ, ปิด Modal, React Query invalidate query เพื่อรีเฟรชข้อมูลหน้า)
    - "ยกเลิก" (ปิด Modal)
    - (อาจมี) ปุ่ม "ลบนิยาย" ภายใน Modal นี้ด้วย (**Hero UI Button**, destructive): แสดง **Hero UI Modal (Alert variant)** ยืนยัน, Toast, redirect
- **การพัฒนา:** เหมือน 5.1.1 แต่เป็น API Route สำหรับ update, และมีการสร้าง `novel_versions`

### 5.2.2 Modal เพิ่มตอนด้วย .txt (Batch Upload Modal)

- **การเรียกใช้งาน:** คลิกปุ่ม "เพิ่มตอนด้วย .txt" ในหน้าข้อมูลนิยาย
- **UI ภายใน Modal (**Hero UI Modal**):**
  - **หัวเรื่อง Modal:** "เพิ่มหลายตอนด้วยไฟล์ .txt สำหรับนิยาย: [ชื่อนิยาย]"
  - **พื้นที่ Drag & Drop ไฟล์ หรือ ปุ่ม "เลือกไฟล์" (**Hero UI Input type file**, multiple):**
    - รองรับการเลือกหลายไฟล์ .txt (จำกัดจำนวนสูงสุด เช่น 500 ไฟล์ต่อครั้ง Client-side และ Server-side)
    - แสดงรายการไฟล์ที่เลือก พร้อมชื่อไฟล์ (ผู้ใช้อาจจัดเรียงชื่อไฟล์ให้ถูกต้องก่อนเลือก)
  - **การจัดการ Chapter Number จากชื่อไฟล์:**
    - หากชื่อไฟล์เป็นรูปแบบ `[ส่วนที่เป็นชื่อตอน] [เลขตอนที่มี padding]` เช่น "Chapter 001.txt", "ตอนที่ 0004.txt", ระบบจะพยายามดึง `[เลขตอนที่มี padding]` (เช่น "001", "0004") มาใช้เป็น `chapters.chapter_number` (เก็บเป็น TEXT) และจะใช้ส่วนที่เหลือเป็นชื่อตอน (`chapters.title`)
    - หากนิยายเริ่มด้วยตอนที่สูงๆ เช่น ไฟล์ชื่อ "ตอนที่ 201.txt", ระบบจะใช้ "201" เป็น `chapter_number`
    - ลำดับการสร้างตอนจะขึ้นอยู่กับการเรียงลำดับชื่อไฟล์ที่ผู้ใช้อัปโหลด
  - **ตัวเลือกการจัดการไฟล์ชื่อซ้ำ (ถ้า `chapter_number` ที่ได้จากชื่อไฟล์ซ้ำกับที่มีอยู่):**
    - (**Hero UI Radio** group): "เขียนทับตอนที่มีอยู่ (Overwrite)" หรือ "ข้ามไฟล์นี้ (Skip)"
  - **Progress Bar (**Hero UI Progress**):** แสดงความคืบหน้าการอัปโหลดและประมวลผลไฟล์ (ถ้าทำได้แบบ real-time)
  - **ปุ่มดำเนินการ:** "เริ่มอัปโหลด", "ยกเลิก"
- **การทำงานเบื้องหลัง (Next.js API Route ที่รับ `multipart/form-data`):**
  - รับไฟล์ .txt ทั้งหมด
  - ประมวลผลแต่ละไฟล์:
    - อ่านเนื้อหาจาก .txt
    - แยก `chapter_number` และ `title` จากชื่อไฟล์ตาม Logic ที่กำหนด
    - ดึงค่า `default_chapter_status` และ `default_chapter_access` จาก `novels` table ของนิยายนี้
    - สร้าง record ใหม่ใน `chapters` table (เก็บเนื้อหาใน `content_original` เป็น plain text)
    - **สำคัญ:** สำหรับแต่ละตอนที่สร้างใหม่สำเร็จ, สร้าง record ว่างๆ ในตาราง `mark_lines` (สำหรับนักแปลเจ้าของนิยาย, `user_id` จาก `novels.created_by`) และสร้าง `chapter_versions` record แรกโดยอัตโนมัติ
- **การแจ้งเตือน (**Hero UI Alert (Toast)**) เมื่อสำเร็จ:** สรุปผลอย่างละเอียด เช่น "เพิ่มตอนใหม่ X ตอนสำเร็จ, เขียนทับ Y ตอน, ข้าม Z ตอน"
- **การพัฒนา:** **Hero UI (Modal, Input, Progress, Radio)**, Next.js API Route (ใช้ library เช่น `formidable` หรือ `multer` สำหรับ parsing `multipart/form-data` ถ้าจำเป็น), Supabase (batch insert `chapters`, `mark_lines`, `chapter_versions`)

### 5.2.3 Modal เพิ่ม/แก้ไขตอน (Create/Edit Chapter Modal)

- **การเรียกใช้งาน:**
  - **เพิ่มตอนใหม่:** คลิกปุ่ม "เพิ่มตอน" ในหน้าข้อมูลนิยาย
  - **แก้ไขตอน:** คลิกปุ่ม "แก้ไขตอน" (ไอคอนดินสอ) จากรายการตอนในส่วนสารบัญ
- **UI ภายใน Modal (**Hero UI Modal**):**
  - **หัวเรื่อง:** "เพิ่มตอนใหม่สำหรับ: [ชื่อนิยาย]" หรือ "แก้ไขตอน: [ชื่อตอนปัจจุบัน]"
  - **ฟอร์ม (React Hook Form + Zod):**
    - **ชื่อตอน (Chapter Title):** (**Hero UI Input**), จำเป็นต้องกรอก (Zod: `required`)
    - **หมายเลขตอน (`chapter_number` - เก็บเป็น TEXT):**
      - **เพิ่มตอนใหม่:** (**Hero UI Input**) แสดงค่า auto-increment จากตอนล่าสุด +1 เป็นค่าเริ่มต้น (เช่น ถ้าตอนล่าสุดคือ "005" ให้แสดง "006"). **ผู้ใช้สามารถแก้ไขได้** (ต้อง validate ไม่ให้ซ้ำกับ `chapter_number` อื่นในนิยายเดียวกัน และควรเป็นรูปแบบตัวเลขที่อาจมี padding).
      - **แก้ไขตอน:** (**Hero UI Input**) แสดงเลขปัจจุบัน, **ผู้ใช้สามารถแก้ไขได้** (ต้องระวังเรื่องลำดับ และ validate ความซ้ำซ้อน).
    - **เนื้อหา (`content_original`):** **Rich Text Editor** (เช่น TipTap) ที่ผูกกับ React Hook Form Controller.
      - รองรับการจัดรูปแบบพื้นฐาน, ใช้ฟอนต์ Sarabun, คัดลอก/วางจาก Word
    - **สถานะตอน (Chapter Status):** (**Hero UI Radio** group) ("เผยแพร่", "ฉบับร่าง")
    - **สิทธิ์การเข้าถึงตอน (Chapter Access Level):** (**Hero UI Select (multiple)** หรือ **Hero UI Checkbox** Group) ("ทุกคน", "เฉพาะสมาชิก", "เฉพาะนักแปล")
    - *หมายเหตุ:*
      - **สำหรับเพิ่มตอนใหม่:** ค่าเริ่มต้นของ "สถานะตอน" และ "สิทธิ์การเข้าถึงตอน" จะถูกดึงมาจาก "ค่าเริ่มต้นสำหรับการสร้างตอนใหม่" ที่ตั้งไว้ในระดับนิยาย (Novel settings)
      - ผู้ใช้สามารถปรับเปลี่ยนค่าเหล่านี้สำหรับตอนนี้โดยเฉพาะได้ใน Modal นี้
  - **ปุ่มดำเนินการ (**Hero UI Button**):**
    - **กรณีเพิ่มตอนใหม่:** "เผยแพร่ตอน", "บันทึกเป็นฉบับร่าง"
    - **กรณีแก้ไขตอน:** "บันทึกการเปลี่ยนแปลง"
    - **(ทั้งสองกรณี):** "ยกเลิก" (ปิด Modal)
- **การทำงานเบื้องหลัง (Next.js API Route):**
  - **เมื่อเพิ่มตอนใหม่สำเร็จ:** สร้าง record ใน `chapters`, สร้าง record ว่างๆ ใน `mark_lines` (สำหรับนักแปลเจ้าของนิยาย, `user_id` จาก `novels.created_by`), และสร้าง `chapter_versions` record แรก. แสดง **Hero UI Alert (Toast)** "เพิ่มตอน '[ชื่อตอน]' สำเร็จแล้ว". React Query invalidate query สารบัญ.
  - **เมื่อแก้ไขตอนสำเร็จ:** อัปเดต record ใน `chapters`. หากมีการเปลี่ยนแปลงเนื้อหา (`content_original`) หรือข้อมูลสำคัญอื่นๆ (title, status, access) ให้สร้างรายการใหม่ใน `chapter_versions`. แสดง **Hero UI Alert (Toast)** "บันทึกการเปลี่ยนแปลงตอน '[ชื่อตอน]' เรียบร้อยแล้ว". React Query invalidate query สารบัญ.
- **การพัฒนา:** **Hero UI (Modal, Input, Radio, Select, Checkbox, Button)**, React Hook Form + Zod, TipTap, Next.js API Route, Supabase

### 5.3 หน้าบุ๊คมาร์ค (Bookmarks Page - `pages/bookmarks.tsx`)

- **Data Fetching:** ใช้ React Query (TanStack Query) เพื่อดึงข้อมูลบุ๊คมาร์คของผู้ใช้ปัจจุบัน (`user_id`) จากตาราง `user_bookmarks` โดย JOIN กับ `novels` และ `chapters` เพื่อแสดงข้อมูลที่จำเป็น
- **ความสามารถ:**
  - แสดงรายการนิยายที่ผู้ใช้ปัจจุบันได้บุ๊คมาร์คตอนใดตอนหนึ่งไว้
  - เรียงลำดับรายการนิยายตามวันที่บุ๊คมาร์คล่าสุดของตอนในนิยายนั้นๆ (latest `bookmarked_at` for that novel, Logic การเรียงใน query หรือ client-side)
  - **สำหรับแต่ละนิยายในรายการ (แสดงเป็น **Hero UI Card** หรือ List Item):**
    - แสดงรูปปกนิยาย (`next/image`), ชื่อนิยาย
    - แสดงชื่อตอนล่าสุดที่บุ๊คมาร์คสำหรับนิยายเรื่องนั้น (เช่น "บุ๊คมาร์คล่าสุด: ตอนที่ X - ชื่อตอน")
    - ปุ่ม "ไปยังบุ๊คมาร์คล่าสุด" (**Hero UI Button** หรือ Link): นำทางผู้ใช้ไปยังหน้าอ่านของตอนล่าสุดที่บุ๊คมาร์คสำหรับนิยายเรื่องนั้น (ใช้ `<Link href={`/novels/${novelId}/${latestBookmarkedChapterNumber}`}>`)
    - ปุ่ม "จัดการบุ๊คมาร์คสำหรับเรื่องนี้" (**Hero UI Button**): เปิด Modal "จัดการบุ๊คมาร์คสำหรับเรื่อง: [ชื่อนิยาย]"
- **Modal "จัดการบุ๊คมาร์คสำหรับเรื่อง: [ชื่อนิยาย]" (**Hero UI Modal**):**
  - **หัวเรื่อง:** "จัดการบุ๊คมาร์คสำหรับ: [ชื่อนิยาย]"
  - **ตัวเลือกการเรียงลำดับภายใน Modal:** (**Hero UI Select**) (เรียงตามลำดับตอน (น้อยไปมาก/มากไปน้อย), เรียงตามวันที่บุ๊คมาร์ค (ล่าสุด/เก่าสุด))
  - **รายการบุ๊คมาร์คของเรื่องนั้น (แสดงเฉพาะตอนที่บุ๊คมาร์คไว้, เป็น List พร้อม **Hero UI Checkbox**):**
    - Checkbox หน้าแต่ละรายการ (สำหรับเลือกเพื่อลบ)
    - หมายเลขตอน, ชื่อตอน
    - วันที่/เวลาที่บุ๊คมาร์ค
  - **ปุ่มดำเนินการใน Modal (**Hero UI Button**):**
    - ปุ่ม "ลบรายการที่เลือก": เรียก API Route เพื่อลบบุ๊คมาร์คที่ผู้ใช้ติ๊กเลือก (เมื่อสำเร็จ แสดง **Hero UI Alert (Toast)** "ลบบุ๊คมาร์ค X รายการสำเร็จแล้ว" และ React Query invalidate query บุ๊คมาร์ค)
    - ปุ่ม "ล้างบุ๊คมาร์คทั้งหมดของเรื่องนี้":
      - แสดง **Hero UI Modal (Alert variant)** ยืนยัน
      - เมื่อยืนยัน, เรียก API Route เพื่อลบบุ๊คมาร์คทั้งหมดของนิยายเรื่องนั้นสำหรับผู้ใช้ปัจจุบัน (เมื่อสำเร็จ แสดง **Hero UI Alert (Toast)** "ลบบุ๊คมาร์คทั้งหมดของเรื่อง '[ชื่อนิยาย]' สำเร็จแล้ว", ปิด Modal, React Query invalidate)
    - ปุ่ม "ปิด" หรือ "ยกเลิก"
- **ปุ่ม "ลบบุ๊คมาร์คทั้งหมด (ทุกเรื่อง)":**
  - (**Hero UI Button**, destructive variant) วางในตำแหน่งที่เหมาะสม
  - เมื่อคลิก แสดง **Hero UI Modal (Alert variant)** ยืนยัน "คุณต้องการลบบุ๊คมาร์คทั้งหมดทุกเรื่องใช่หรือไม่?"
  - เมื่อยืนยัน, เรียก API Route เพื่อลบบุ๊คมาร์คทั้งหมดของผู้ใช้ปัจจุบัน (เมื่อสำเร็จ แสดง **Hero UI Alert (Toast)** "ลบบุ๊คมาร์คทั้งหมด (ทุกเรื่อง) เรียบร้อยแล้ว" และ React Query invalidate)
- **การพัฒนา:** Next.js Page, React Query, Supabase (API calls ผ่าน Next.js API Routes), **Hero UI (Card, List, Modal, Select, Checkbox, Button)**, **Font Awesome Icons**

### 5.4 แดชบอร์ดนักแปล (Translator Dashboard - `pages/translator/dashboard.tsx`)

- **Data Fetching:** ใช้ React Query เพื่อดึงรายการนิยายที่นักแปลคนปัจจุบันเป็นผู้สร้าง (`created_by`) หรือได้รับสิทธิ์ในการแปล/จัดการ (จาก `novels` table หรือ `translator_novel_permissions` table ถ้ามี)
- **ความสามารถ:**
  - แสดงรายการนิยายที่นักแปลจัดการ
  - **รูปแบบการแสดงผล: การ์ดนิยาย (**Hero UI Card**):**
    - รูปปก (`next/image`), ชื่อนิยาย
    - **(เพิ่มเติม)** ความคืบหน้าการมาร์คโดยรวมสำหรับนิยายนั้นๆ (เช่น "X% ของตอนทั้งหมดมีมาร์ค" หรือ "Y/Z ตอนมีมาร์ค") พร้อม (**Hero UI Progress Bar**) ที่แสดงเปอร์เซ็นต์นี้ (Logic การคำนวณอาจทำใน API Route หรือ Client-side จากข้อมูลตอนทั้งหมดและตอนที่มีมาร์ค)
  - **สำหรับแต่ละนิยายในการ์ด:**
    - ปุ่ม "จัดการบรรทัดมาร์ค" (**Hero UI Button**): นำทางไปยังหน้า `/translator/manage-marks/[novel_id]` (ใช้ `<Link>`)
    - ปุ่ม "ไปยังหน้าข้อมูลนิยาย" (**Hero UI Button**): นำทางไป `/novels/[novel_id]`
    - (อาจมี) ปุ่ม "เพิ่มตอนใหม่" (เปิด Modal 5.2.3)
- **การพัฒนา:** Next.js Page, React Query, Supabase (API calls ผ่าน Next.js API Routes), **Hero UI (Card, Progress Bar, Button)**

### 5.4.1 หน้าจัดการบรรทัดมาร์ค (Mark Line Management Page - `pages/translator/manage-marks/[novel_id].tsx`)

- **Data Fetching:** ใช้ `getServerSideProps` เพื่อดึงชื่อนิยาย (จาก `novels`) และ React Query เพื่อดึงรายการตอน (`chapters`) และข้อมูลมาร์ค (`mark_lines`) ของนิยายนั้นสำหรับนักแปลปัจจุบัน
- **หัวเรื่อง:** "จัดการบรรทัดมาร์คสำหรับนิยาย: [ชื่อนิยาย]"
- **ส่วนควบคุมหลัก:**
  - ปุ่ม "เลือกตอนเพื่อดำเนินการหลายรายการ" (**Hero UI Button** หรือ **Hero UI Toggle/Switch**) (Toggle Batch Mode): เมื่อเปิดโหมดนี้ จะแสดง **Hero UI Checkbox** หน้าแต่ละตอน และแสดง Batch Actions Toolbar
- **การแสดงผลสารบัญตอน (**Hero UI Collapse (Accordion)** หรือ List):**
  - ปรับจำนวนตอนต่อกลุ่ม (10, 20, 50, 100 ตอน)
  - หัวกลุ่มตอน: ชื่อกลุ่ม (เช่น "ตอนที่ 1-50"), (ถ้า Batch Mode) **Hero UI Checkbox** "เลือกทั้งหมดในกลุ่มนี้"
- **รายการตอน (สำหรับแต่ละตอนในสารบัญ):**
  - (ถ้า Batch Mode) (**Hero UI Checkbox**) สำหรับเลือกตอน
  - หมายเลขตอน, ชื่อตอน
  - จำนวนบรรทัดที่มาร์คในตอนนี้ (นับจาก `mark_lines.marked_line_numbers` JSONB array length)
  - สถานะการมาร์ค (เช่น ไอคอน `fa-check-circle`/`fa-circle` จาก **Font Awesome**)
  - ปุ่ม "คัดลอกมาร์คตอนนี้" (ไอคอน `fa-copy` จาก **Font Awesome**, **Hero UI Button**): คัดลอกรายการเลขบรรทัดที่มาร์คของตอนนี้ไปยัง Clipboard (ใช้ `navigator.clipboard.writeText`), แสดง **Hero UI Alert (Toast)**
  - ปุ่ม "ไปที่หน้าอ่าน/แปลตอนนี้": นำทางไปหน้าอ่านของตอนนี้ในโหมดนักแปล (ใช้ `<Link>`)
- **Batch Actions Toolbar (**Hero UI Component** เช่น `div` styled as Toolbar, แสดงเมื่อ Batch Mode และมีตอนที่เลือก):**
  - ข้อความแสดงจำนวนตอนที่เลือก
  - ปุ่ม "คัดลอกบรรทัดมาร์คของตอนที่เลือก": คัดลอกมาร์คของทุกตอนที่เลือก, แสดง **Hero UI Alert (Toast)**
  - ปุ่ม "ดาวน์โหลดไฟล์บรรทัดมาร์คของตอนที่เลือก":
    - เปิด **Hero UI Modal** ตัวเลือกการดาวน์โหลด:
      - **รูปแบบไฟล์:** (**Hero UI Radio** group) "รวมเป็นไฟล์ .txt เดียว" หรือ "แยกไฟล์ .zip"
      - **ชื่อไฟล์ใน .zip:** จะเป็นชื่อเดียวกับที่แสดงในหน้าเว็บหรือชื่อไฟล์ต้นฉบับที่อัปโหลด (เช่น `[chapter_number_with_padding]-[chapter_title].txt`). ให้ยึดตาม `chapters.title` และ `chapters.chapter_number` เป็นหลักในการสร้างชื่อไฟล์
    - เมื่อผู้ใช้เลือกและกดยืนยัน: Client-side logic (ใช้ JSZip สำหรับ .zip) สร้างและเริ่มดาวน์โหลดไฟล์, แสดง **Hero UI Alert (Toast)**
- **การพัฒนา:** Next.js Page (Dynamic Route), React Query, **Hero UI (Collapse, Checkbox, Button, Modal, Radio, Toggle/Switch)**, **Font Awesome Icons**, Supabase (API calls), JSZip (สำหรับสร้าง .zip client-side)

### 5.5 หน้าอ่านนิยายและโหมดแปล (Reading & Translator Mode UI - `pages/novels/[novel_id]/[chapter_number].tsx`)

- **Data Fetching:** ใช้ `getServerSideProps` เพื่อดึงข้อมูลตอนปัจจุบัน (`chapters.content_original`, `chapters.title`, etc.) และข้อมูลนิยาย (ชื่อนิยาย, ตอนก่อนหน้า/ถัดไป) จาก Supabase ตาม `novel_id` และ `chapter_number` หากเป็นโหมดนักแปล อาจดึงข้อมูล `mark_lines` ของตอนนี้มาด้วย
- **การแสดงผลเนื้อหา (Content Display - React Component):**
  - **ชื่อตอน (Chapter Title):** แสดงอย่างชัดเจนตรงกลางด้านบน
  - **เนื้อหานิยาย (`chapters.content_original` เท่านั้น):**
    - **การย่อหน้า (Indentation):** ใช้ CSS `text-indent`
    - **PC Layout:** เนื้อหาในคอนเทนเนอร์กว้างจำกัด, จัดกลาง
    - **Mobile Layout:** เนื้อหาเต็มความกว้าง (มี padding เล็กน้อย)
    - **No Horizontal Scroll**
- **การแสดงผลร่วม (ทั้ง PC และ Mobile):**
  - **ปุ่ม "Arrow Up" (กลับไปด้านบนสุด):** (ไอคอน `fa-arrow-up` จาก **Font Awesome**) แสดงเมื่อเลื่อนลง, กดแล้ว smooth scroll to top
- **การควบคุมด้วยคีย์บอร์ด (PC เท่านั้น) (Event Listeners ใน React `useEffect`):**
  - ลูกศรซ้าย/ขวา: ไปยังตอนก่อนหน้า/ถัดไป (เรียก `router.push`)
  - Space Bar: เลื่อนหน้าลง
- **โหมดการอ่าน (สำหรับ Member และ Translator เมื่อไม่ได้เปิด "โหมดนักแปล"):**
  - **แถบเครื่องมือหลักด้านบน (Top Reading Toolbar - React Component, อาจเป็น sticky):**
    - **PC Items:**
      - ปุ่ม "สารบัญ" (ไอคอน `fa-list`): เปิด **Hero UI Modal** แสดงรายการตอน
      - ปุ่ม "ปรับแต่งการแสดงผล (Aa)" (ไอคอน `fa-cog` หรือ `fa-font`): เปิด **Hero UI Popover/Dropdown** ตั้งค่า Font Size, Line Height (สถานะเก็บใน Zustand/Context + localStorage)
      - ปุ่ม "บุ๊คมาร์คตอนนี้" (ไอคอน `fa-bookmark`): สลับสถานะ (API call), แสดง **Hero UI Alert (Toast)**
      - ปุ่ม "สลับธีมเว็บไซต์" (ไอคอน `fa-sun`/`fa-moon`)
      - ปุ่ม "โหมดอ่านต่อเนื่อง" (ไอคอน `fa-infinity`): เปิด/ปิด (สถานะเก็บใน Zustand/Context + localStorage), **Hero UI Alert (Toast)**
      - ปุ่มนำทางตอน "< ตอนก่อนหน้า", "ตอนต่อไป >" (ถ้ามี)
    - **Mobile Items (อาจมีบางส่วนซ่อนในเมนู "เพิ่มเติม"):** คล้าย PC
  - **การนำทางตอน (Chapter Navigation - เฉพาะ Mobile, เพิ่มเติม):**
    - **แถบเครื่องมือด้านล่าง (Bottom Navigation Toolbar - อาจ auto-hide/show):** ปุ่ม "< ตอนก่อนหน้า", "ตอนต่อไป >"
  - **เมื่อเลื่อนถึงท้ายบท:**
    - **(ถ้า "โหมดอ่านต่อเนื่อง" ปิดอยู่):** แสดงปุ่มขนาดใหญ่ "ตอนต่อไป: [ชื่อตอนถัดไป]" (ถ้ามี) คลิกแล้ว `router.push`
    - **(ถ้า "โหมดอ่านต่อเนื่อง" เปิดอยู่):** เมื่อใกล้ท้ายบท, ใช้ React Query หรือ SWR ดึงเนื้อหาตอนถัดไปและ render ต่อท้าย (Infinite Scroll). อัปเดต URL ด้วย `router.replace` และอัปเดตชื่อตอนที่แสดง (ใช้ Intersection Observer API ตรวจจับ)
- **โหมดนักแปล (Translator Mode - แสดงผลเมื่อนักแปลเปิด "โหมดนักแปล" สำหรับนิยายนี้):**
  - **แถบเครื่องมือหลักด้านบน (Top Reading Toolbar):** เหมือนโหมดอ่าน แต่ **เพิ่มเติมด้วย**:
    - ปุ่ม "คัดลอกมาร์คทั้งหมด" (ไอคอน `fa-copy` อาจมี `fa-check` ประกอบ): สำหรับตอนปัจจุบัน, แสดง **Hero UI Alert (Toast)**
  - **การแสดงผลเนื้อหา (แตกต่างจากโหมดอ่าน):**
    - **แถบเลขบรรทัด (Line Number Gutter):** แสดงทางซ้าย, สีปรับตาม Theme
    - **เนื้อหาต้นฉบับ (`chapters.content_original`):** แสดงถัดจากเลขบรรทัด, ย่อหน้า. เนื้อหาจะถูก split เป็นบรรทัด (paragraphs) เพื่อแสดงเลขบรรทัดและ checkbox
    - **Checkbox สำหรับมาร์ค (**Hero UI Checkbox**):** แสดงทางขวาสุดของแต่ละบรรทัด
  - **MarkLine Interaction (หัวใจสำคัญ: Real-time):**
    - **คลิก Checkbox:** มาร์ค/ยกเลิกมาร์คบรรทัดนั้นๆ. **Real-time update:** เรียก API Route ทันทีเพื่ออัปเดต `mark_lines.marked_line_numbers` ใน Supabase (อาจใช้ `useMutation` จาก React Query). แสดง **Hero UI Alert (Toast)** "มาร์ค/ยกเลิกมาร์คบรรทัดที่ X".
    - **ดับเบิลคลิกที่บรรทัดเนื้อหา:**
      - ไฮไลท์บรรทัดชั่วครู่
      - คัดลอกข้อความต้นฉบับของบรรทัดนั้นไป Clipboard
      - "มาร์ค" บรรทัดนั้นอัตโนมัติ (เรียก API update)
      - แสดง **Hero UI Alert (Toast)** "คัดลอกและมาร์คบรรทัดที่ X แล้ว"
  - **เมื่อเลื่อนถึงท้ายบทในโหมดนักแปล:**
    - แสดงส่วนสรุปข้อมูลการมาร์ค: "มีมาร์คทั้งหมด: [จำนวน]", "บรรทัดที่มาร์ค: [เลขบรรทัด]..." (ปุ่ม "ดูทั้งหมด" เปิด Modal)
    - ปุ่ม "ล้างมาร์คทั้งหมดของตอนนี้" (**Hero UI Button**, destructive): แสดง **Hero UI Modal (Alert variant)** ยืนยัน, เรียก API update, **Hero UI Alert (Toast)**
    - ปุ่ม "คัดลอกมาร์คทั้งหมดของตอนนี้" (**Hero UI Button**)
- **การพัฒนา:**
  - Next.js Page (Dynamic Route, `getServerSideProps`)
  - Zustand หรือ React Context (สำหรับจัดการสถานะ UI เช่น font size, translator mode, continuous read, theme)
  - React Logic (จัดการ UI, scrolling, infinite scroll, line numbering, mark interactions, keyboard controls)
  - **Hero UI (Modal, Popover, Dropdown, Button, Checkbox, Toggle/Switch)**, **Font Awesome Icons**
  - Supabase (API calls ผ่าน Next.js API Routes หรือ Server Actions ถ้า Next.js เวอร์ชันใหม่รองรับดี), React Query (`useQuery`, `useMutation` - **สำคัญมากสำหรับการอัปเดต `mark_lines` แบบ real-time**)
  - Intersection Observer API (สำหรับ infinite scroll และอัปเดตชื่อตอน)
  - Logic สำหรับ split `content_original` เป็นบรรทัดเพื่อแสดงเลขและ checkbox (อาจทำตอน `getServerSideProps` หรือ Client-side)

### 5.6 แดชบอร์ดผู้ดูแลระบบ (Admin Dashboard - `pages/admin/dashboard/*`)

- **โครงสร้างหน้าแดชบอร์ด (Admin Layout Component - `components/layouts/AdminLayout.tsx`):**
  - **Sidebar นำทางด้านซ้าย (Admin Sidebar - สร้างด้วย **Hero UI Menu** หรือ List component ที่เหมาะสม):**
    - โลโก้/ชื่อเว็บไซต์
    - ลิงก์ (ใช้ `<Link>` จาก `next/link`):
      - "ภาพรวม" (ไปยัง `/admin/dashboard` หรือ `/admin/dashboard/overview`)
      - "จัดการผู้ใช้" (ไปยัง `/admin/dashboard/users`)
      - "จัดการหมวดหมู่นิยาย" (ไปยัง `/admin/dashboard/categories`)
      - "จัดการประกาศ" (ไปยัง `/admin/dashboard/announcements`)
      - "จัดการเว็บไซต์" (ไปยัง `/admin/dashboard/site-settings`)
      - "จัดการเวอร์ชัน" (ไปยัง `/admin/dashboard/versions`)
    - (อาจมี) ลิงก์ "กลับไปหน้าหลักเว็บ" (`<Link href="/">`)
  - **พื้นที่แสดงเนื้อหาหลัก (Main Content Area):** แสดงเนื้อหาของ Page Component ตาม Route ที่เลือกใน Sidebar
  - **การป้องกัน Route:** Admin Layout หรือแต่ละ Page ใน `/admin/*` ควรมีการตรวจสอบสิทธิ์ Admin (เช่น ใน `getServerSideProps` หรือ Client-side redirect ถ้า user ไม่ใช่ Admin)

### 5.6.1 หน้าภาพรวมแดชบอร์ด (Dashboard Overview Page - `pages/admin/dashboard/index.tsx` หรือ `pages/admin/dashboard/overview.tsx`)

- **หัวเรื่องหน้า:** "ภาพรวมแดชบอร์ดผู้ดูแลระบบ"
- **Data Fetching:** ใช้ React Query หรือ `getServerSideProps` เพื่อดึงข้อมูลสรุปจาก Supabase (Aggregate queries)
- **ความสามารถ:** แสดงข้อมูลสรุปในรูปแบบ (**Hero UI Card**) หรือกราฟเบื้องต้น (ถ้าต้องการ อาจใช้ `recharts` หรือ `nivo`):
  - จำนวนนิยายทั้งหมด (แบ่งตามสถานะ)
  - จำนวนตอนทั้งหมด
  - จำนวนผู้ใช้ทั้งหมด (แบ่งตามบทบาท)
  - จำนวนผู้ใช้ลงทะเบียนใหม่ (เช่น ใน 7 วัน/30 วัน)
- **การพัฒนา:** Next.js Page, **Hero UI Card**, Supabase (API calls)

### 5.6.2 หน้าจัดการผู้ใช้ (User Management Page - `pages/admin/dashboard/users/index.tsx` และ `pages/admin/dashboard/users/[user_id].tsx`)

- **หน้า `/admin/dashboard/users/index.tsx` (List Users):**
  - **หัวเรื่องหน้า:** "จัดการผู้ใช้ทั้งหมด"
  - **Data Fetching:** React Query (สำหรับ pagination, sorting, filtering)
  - **ส่วนควบคุม:**
    - ช่องค้นหาผู้ใช้ (**Hero UI Input**): ค้นหาตาม Email, ชื่อที่แสดง (กรอง real-time debounced หรือ on-submit)
    - ฟิลเตอร์ตามบทบาท (**Hero UI Select**)
    - แสดงจำนวนผู้ใช้ทั้งหมด/ตามผลค้นหา
  - **ตารางผู้ใช้ (User Table - สร้างด้วย **Hero UI Table**):**
    - **คอลัมน์:** `#`, Avatar (`next/image`), Email, ชื่อที่แสดง, สิทธิ์ (**Hero UI Badge** สีตามบทบาท), วันที่สมัคร, วันหมดอายุสิทธิ์, สถานะ (ปกติ/ถูกระงับ), ปุ่ม "จัดการ" (ไอคอน `fa-cog` หรือ `fa-edit` จาก **Font Awesome**, ลิงก์ไป `/admin/dashboard/users/[user_id]`)
    - หัวคอลัมน์ Sortable, มี Pagination (**Hero UI Pagination**)
- **หน้า `/admin/dashboard/users/[user_id].tsx` (Edit User Detail Page):**
  - **Data Fetching:** `getServerSideProps` หรือ React Query เพื่อดึงข้อมูล Profile ของ `user_id` นั้น
  - **หัวเรื่อง:** "จัดการผู้ใช้: [Email หรือ Display Name]"
  - **UI (Form จัดการด้วย React Hook Form + Zod):**
    - แสดง/แก้ไข Avatar (อัปโหลดใหม่ได้)
    - Email (Read-only)
    - ชื่อที่แสดง (Display Name - **Hero UI Input** สำหรับแก้ไข)
    - สิทธิ์ (Role - **Hero UI Select** สำหรับแก้ไข)
    - วันที่สมัคร (Read-only)
    - วันที่สิทธิ์หมดอายุ (Role Expiry Date - **Hero UI Input type="date"** หรือ custom DatePicker + **Hero UI Popover**, ปุ่มช่วย: +1/3/6 เดือน, ไม่มีหมดอายุ)
    - ระงับบัญชี (Suspend Account - **Hero UI Toggle/Switch**)
  - **ปุ่มดำเนินการ (**Hero UI Button**):**
    - "บันทึกการเปลี่ยนแปลง": เรียก API Route เพื่ออัปเดต `profiles` (**Hero UI Alert (Toast)**, React Query invalidate list)
    - "ยกเลิก" (กลับไปหน้ารายการผู้ใช้)
- **การพัฒนา:** Next.js Pages (Dynamic Routes), **Hero UI (Input, Table, Pagination, Select, DatePicker components, Toggle/Switch, Badge)**, React Hook Form + Zod, Supabase (API calls), React Query

### 5.6.3 หน้าจัดการหมวดหมู่นิยาย (Category Management Page - `pages/admin/dashboard/categories.tsx`)

- **หัวเรื่องหน้า:** "จัดการหมวดหมู่นิยาย"
- **Data Fetching:** React Query สำหรับรายการหมวดหมู่
- **ส่วนเพิ่ม/แก้ไขหมวดหมู่ (ใช้ **Hero UI Modal** + Form React Hook Form + Zod):**
  - ปุ่ม "เพิ่มหมวดหมู่ใหม่" (เปิด Modal)
  - **Modal เพิ่ม/แก้ไขหมวดหมู่:**
    - ฟอร์ม: ชื่อหมวดหมู่ (จำเป็น, unique), Slug (auto-generate, แก้ไขได้, unique), คำอธิบาย (optional)
    - ปุ่ม: "บันทึก", "ยกเลิก" (**Hero UI Alert (Toast)**, React Query invalidate)
- **รายการหมวดหมู่ (**Hero UI Table** หรือ List):**
  - แสดง: ชื่อ, Slug, จำนวนนิยายในหมวด
  - ปุ่ม "แก้ไข" (เปิด Modal แก้ไข)
  - ปุ่ม "ลบ" (**Hero UI Button**, destructive): แสดง **Hero UI Modal (Alert variant)** ยืนยัน, **ตัวเลือกจัดการนิยายในหมวดที่จะลบ** (ย้ายไปหมวดอื่น (**Hero UI Select**) หรือตั้งเป็น NULL), เรียก API Route (**Hero UI Alert (Toast)**, React Query invalidate)
- **การพัฒนา:** Next.js Page, **Hero UI (Modal, Table, Button, Select)**, React Hook Form + Zod, Supabase (API calls), React Query

### 5.6.4 หน้าจัดการประกาศ (Announcement Management Page - `pages/admin/dashboard/announcements.tsx`)

- **หัวเรื่องหน้า:** "จัดการประกาศ"
- **Data Fetching:** React Query สำหรับรายการประกาศ
- **ความสามารถ:**
  - ปุ่ม "สร้างประกาศใหม่" (เปิด Modal สร้าง/แก้ไขประกาศ)
  - **ตารางประกาศ (**Hero UI Table**):**
    - คอลัมน์: หัวข้อ, สถานะ (Active/Inactive - **Hero UI Badge**), หน้าที่แสดง, กลุ่มผู้รับ, วันที่เริ่ม/สิ้นสุด, ปุ่มดำเนินการ
    - ปุ่มดำเนินการ: "แก้ไข" (เปิด Modal), "ลบ" (**Hero UI Modal (Alert variant)**, Toast), "สลับสถานะ" (**Hero UI Toggle/Switch** หรือ Button, API call, Toast)
- **Modal สร้าง/แก้ไขประกาศ (**Hero UI Modal** + Form React Hook Form + Zod):**
  - ฟิลด์: หัวข้อ, เนื้อหา (Markdown - TipTap หรือ `react-markdown` editor/preview), วันที่เริ่ม/สิ้นสุด (DatePicker component), หน้าที่แสดง (Multi-select/Checkbox Group), กลุ่มผู้รับ (Multi-select/Checkbox Group), สถานะ "ใช้งาน" (**Hero UI Toggle/Switch**)
  - ปุ่ม: "บันทึก", "ยกเลิก" (**Hero UI Alert (Toast)**, React Query invalidate)
- **การพัฒนา:** Next.js Page, **Hero UI (Modal, Table, Button, Select, DatePicker components, Toggle/Switch, Badge)**, React Hook Form + Zod, TipTap/`react-markdown`, Supabase (API calls), React Query

### 5.6.5 หน้าจัดการเว็บไซต์ (Site Management Page - `pages/admin/dashboard/site-settings.tsx`)

- **หัวเรื่องหน้า:** "จัดการการตั้งค่าเว็บไซต์"
- **Data Fetching:** React Query เพื่อดึงการตั้งค่าปัจจุบันจาก `site_settings` table
- **Layout:** แบ่งเป็น Section หรือ Card (**Hero UI Card**)
- **UI (Form จัดการด้วย React Hook Form + Zod, อาจมีหลาย Form หรือ Form เดียวใหญ่):**
  - **ส่วนทั่วไป:** ชื่อเว็บ, คำอธิบายเว็บ (SEO), โลโก้ (อัปโหลด, Preview), Favicon (อัปโหลด)
  - **ส่วนลงทะเบียน/เข้าสู่ระบบ:** เปิด/ปิดการลงทะเบียน (**Hero UI Toggle/Switch**), เปิด/ปิด Google OAuth (**Hero UI Toggle/Switch**)
  - **ส่วนเนื้อหา:** จำนวนรายการต่อหน้าเริ่มต้น, ค่าเริ่มต้นสำหรับนิยายใหม่ (สถานะตอน, สิทธิ์เข้าถึงตอน)
- **ปุ่มดำเนินการ (**Hero UI Button**):** "บันทึกการตั้งค่า" (เรียก API Route เพื่ออัปเดต `site_settings`, **Hero UI Alert (Toast)**)
- **การพัฒนา:** Next.js Page, **Hero UI (Card, Input, Textarea, Toggle/Switch, File Input, Button)**, React Hook Form + Zod, Supabase (API calls), React Query

### 5.6.6 หน้าจัดการเวอร์ชัน (Version Management Page - `pages/admin/dashboard/versions.tsx`)

- **หัวเรื่องหน้า:** "จัดการเวอร์ชันและประวัติการเปลี่ยนแปลง"
- **Layout:** ใช้ (**Hero UI Tabs**) แยกประเภทเวอร์ชัน
- **Tab 1: เวอร์ชันเว็บไซต์ / Changelog:**
  - **Data Fetching:** React Query สำหรับ `site_changelogs`
  - แสดงรายการ Changelog (**Hero UI Table**): เลขเวอร์ชัน, วันที่, สรุป (render Markdown), สถานะ
  - ปุ่ม "เพิ่ม Changelog ใหม่" (เปิด Modal)
  - **Modal เพิ่ม/แก้ไข Changelog (Form React Hook Form + Zod):** เลขเวอร์ชัน, วันที่, สรุป (Markdown), สถานะ "เผยแพร่" (**Hero UI Toggle/Switch**)
  - ปุ่มดำเนินการต่อรายการ: "แก้ไข", "ลบ" (**Hero UI Modal (Alert variant)**)
- **Tab 2: เวอร์ชันนิยายและตอน:**
  - **Data Fetching:** React Query (สำหรับค้นหานิยาย, ดึง `novel_versions`, `chapter_versions`)
  - ส่วนค้นหานิยาย (**Hero UI Input** หรือ **Hero UI Select**)
  - **เมื่อเลือกนิยาย:**
    - **ประวัติเวอร์ชันข้อมูลเมตานิยาย (**Hero UI Table**):** แสดงรายการจาก `novel_versions`: เวอร์ชัน, วันที่, ผู้แก้ไข, หมายเหตุ. ปุ่ม "ดูรายละเอียด" (Modal แสดง Snapshot JSONB), ปุ่ม "กู้คืนเวอร์ชันนี้" (**Hero UI Modal (Alert variant)** ยืนยัน, เรียก API, **Hero UI Alert (Toast)**)
    - **ประวัติเวอร์ชันตอน:**
      - (**Hero UI Select**) เลือกตอนของนิยายนั้น
      - **เมื่อเลือกตอน (**Hero UI Table**):** แสดงรายการจาก `chapter_versions`: เวอร์ชัน, วันที่, ผู้แก้ไข, หมายเหตุ. ปุ่ม "ดูเนื้อหา" (Modal แสดง Snapshot), ปุ่ม "กู้คืนเวอร์ชันนี้" (**Hero UI Modal (Alert variant)**, API, **Hero UI Alert (Toast)**)
- **การพัฒนา:** Next.js Page, **Hero UI (Tabs, Table, Modal, Input, DatePicker components, Toggle/Switch, Select)**, React Hook Form + Zod, `react-markdown` (สำหรับแสดง Markdown), Supabase (API calls), React Query

### 5.7 Modal สร้างนิยาย (Create Novel Modal)
- (รายละเอียดตามข้อ 5.1.1 ทุกประการ, ใช้ **Hero UI Modal**, React Hook Form + Zod, TipTap, react-image-crop)

### 5.8 การจัดการตอน (Chapter Management)
- **5.8.1 Modal เพิ่มตอนด้วย .txt (Batch Upload Modal):** (รายละเอียดตามข้อ 5.2.2, ใช้ **Hero UI Modal**)
- **5.8.2 Modal เพิ่ม/แก้ไขตอน (Create/Edit Chapter Modal):** (รายละเอียดตามข้อ 5.2.3, ใช้ **Hero UI Modal**, React Hook Form + Zod, TipTap)

### 5.9 หน้าโปรไฟล์ (Profile Page - `pages/profile.tsx`)

- **การนำทาง:** จาก Dropdown ผู้ใช้ใน Navbar
- **หัวเรื่องหน้า:** "โปรไฟล์ของฉัน" หรือ "ตั้งค่าบัญชี"
- **Data Fetching:** React Query หรือ `getServerSideProps` เพื่อดึงข้อมูลโปรไฟล์ผู้ใช้ปัจจุบัน
- **Layout:** หน้าเพจเดียว, แบ่งเป็นส่วนๆ (**Hero UI Card**)
- **ความสามารถ (ใช้ **Hero UI Components** และ React Hook Form + Zod สำหรับฟอร์ม):**
  - **รูปโปรไฟล์ (Avatar):**
    - แสดง **Hero UI Avatar** ปัจจุบัน (`next/image` จาก `profiles.avatar_url`)
    - ปุ่ม "เปลี่ยนรูปโปรไฟล์": เปิด File Picker, ใช้ `react-image-crop` Crop 1:1, อัปโหลดผ่าน API Route (**Hero UI Alert (Toast)**)
  - **ชื่อที่แสดง (Display Name):**
    - แสดงชื่อปัจจุบัน
    - (**Hero UI Input**) สำหรับแก้ไข + ปุ่ม "บันทึกชื่อที่แสดง" (API call, **Hero UI Alert (Toast)**)
  - **สิทธิ์ผู้ใช้งาน (Role):**
    - แสดงบทบาทปัจจุบัน (**Hero UI Badge** สีตามบทบาท), Read-only
  - **อีเมล (Email):**
    - แสดงอีเมล (Read-only)
  - **เปลี่ยนรหัสผ่าน (Change Password):**
    - ปุ่ม "เปลี่ยนรหัสผ่าน" -> เปิด **Hero UI Modal**
    - **ภายใน Modal (Form React Hook Form + Zod):** รหัสผ่านปัจจุบัน, รหัสผ่านใหม่, ยืนยันรหัสผ่านใหม่
    - ปุ่ม: "บันทึกรหัสผ่านใหม่", "ยกเลิก" (เรียก Supabase Auth API, **Hero UI Alert (Toast)** "เปลี่ยนรหัสผ่านสำเร็จแล้ว", **บังคับ Logout และ Redirect ไปหน้า Login**)
  - **ออกจากระบบ (Logout):**
    - ปุ่ม "ออกจากระบบ" (**Hero UI Button**) -> เปิด **Hero UI Modal (Alert variant)** ยืนยัน (เรียก Supabase `signOut()`, **Hero UI Alert (Toast)** "ออกจากระบบแล้ว", Redirect ไปหน้าแรก)
- **การพัฒนา:** Next.js Page, **Hero UI (Card, Avatar, Input, Button, Modal, Badge)**, React Hook Form + Zod, Supabase (Auth API, DB API), `react-image-crop`

## 6. Key User Experience Flows & Enhancements

- **การล็อกอิน/ลงทะเบียน:** ราบรื่นผ่าน **Hero UI Modal**, มี Feedback ชัดเจน (**Hero UI Alert (Toast)**)
- **การนำทาง (Navigation):** โครงสร้าง **Hero UI Navbar** และ Admin Sidebar ชัดเจน, URL เป็นมิตร (Next.js Routing)
- **ประสบการณ์การอ่าน (Reading Experience):**
  - ปรับแต่งการแสดงผลได้ (ขนาดฟอนต์, etc. - จัดการ State ด้วย Zustand/Context)
  - โหมดอ่านต่อเนื่อง (Infinite Scroll) ทำงานได้ดี (ใช้ React Query/SWR + Intersection Observer)
  - บุ๊คมาร์คใช้งานง่าย
  - การสลับตอนราบรื่น (`next/link` และ `next/router`)
- **เครื่องมือช่วยแปล (Translator Tools):**
  - "โหมดนักแปล" เปิด/ปิดง่ายจากหน้าข้อมูลนิยาย
  - การมาร์คบรรทัด (คลิก checkbox, ดับเบิลคลิกบรรทัด) **ต้อง Real-time และตอบสนองทันที** (ใช้ `useMutation` จาก React Query ส่งการเปลี่ยนแปลงไปยัง API Route เพื่ออัปเดต `mark_lines` โดยอัตโนมัติพร้อม **Hero UI Alert (Toast)** feedback)
  - หน้าจัดการมาร์คมีประสิทธิภาพ
- **การตอบสนองของระบบ (System Responsiveness):**
  - **Empty States:** แสดงผลเหมาะสมเมื่อไม่มีข้อมูล (เช่น ไม่มีนิยาย, ไม่มีบุ๊คมาร์ค - ใช้ Conditional Rendering ใน React)
  - **Loading States:** แสดง Spinner (**Font Awesome icon** เช่น `fa-spinner fa-spin`) หรือ **Hero UI Skeleton** ขณะโหลดข้อมูล (จัดการผ่าน State ของ React Query หรือ SWR)
  - **Error Handling:** แสดงข้อความข้อผิดพลาดที่เป็นมิตร (**Hero UI Alert (Toast)** หรือใน UI) เมื่อเกิดปัญหา (จัดการผ่าน State ของ React Query หรือ `catch` block)
- **การออกแบบที่สวยงามและตอบสนองทุกอุปกรณ์:** ตามปรัชญาการออกแบบ (ใช้ Tailwind CSS และ **Hero UI**)
- **ปุ่ม Arrow Up:** สำหรับเลื่อนกลับไปบนสุดในหน้าที่มีการ scroll ยาวๆ (หน้าอ่าน, รายการยาวๆ)
- **การแจ้งเตือนการกระทำ (Action Confirmations):** ใช้ **Hero UI Alert (Toast notifications)** แสดงผลข้อความยืนยันการกระทำต่างๆ
- **การสร้าง `mark_lines` อัตโนมัติ:** เมื่อมีการสร้างตอนใหม่ (ทั้งแบบ Manual หรือ Batch Upload) ระบบ (API Route ที่สร้างตอน) จะต้องสร้าง record ว่างๆ (`marked_line_numbers: []`) ในตาราง `mark_lines` สำหรับตอนนั้นและนักแปลเจ้าของนิยาย (`created_by` ของนิยาย ซึ่งคือ `user_id` ของผู้สร้าง) โดยอัตโนมัติ
- **การคงสถานะหน้าเมื่อรีเฟรช (Persistent State on Refresh):** ทุกหน้าที่มี URL เฉพาะ (เช่น หน้าอ่านตอน, หน้าข้อมูลนิยาย, หน้าใน Admin Dashboard) เมื่อผู้ใช้รีเฟรชเบราว์เซอร์ (F5) จะต้องยังคงอยู่ที่หน้าเดิมและแสดงข้อมูลเดิม (Next.js Routing และ Data Fetching methods เช่น `getServerSideProps` หรือ React Query với caching จัดการส่วนนี้) สถานะ UI ที่ไม่ได้อยู่ใน URL (เช่น สถานะการเปิด/ปิดของ **Hero UI Collapse**, ค่าในฟิลเตอร์ที่ยังไม่ submit) อาจใช้ `localStorage` หรือ Global State (Zustand/Context) ที่ persist ข้อมูลบางส่วน

## 7. Database Structure (Supabase - PostgreSQL)

(โครงสร้างฐานข้อมูลอ้างอิงตามไฟล์ `data.csv` ที่ให้มา และการปรับปรุงก่อนหน้า)

- **`roles`** (id SERIAL PRIMARY KEY, name TEXT UNIQUE NOT NULL) -- *e.g., 'Member', 'Translator', 'Admin'*

- **`profiles`** (
  id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
  display_name TEXT,
  avatar_url TEXT,
  role_id INTEGER NOT NULL REFERENCES roles(id) DEFAULT (SELECT id FROM roles WHERE name='Member'), -- Default role can be set here or by trigger
  role_expiry_date TIMESTAMP WITH TIME ZONE NULL,
  is_suspended BOOLEAN DEFAULT FALSE,
  user_settings JSONB DEFAULT '{}'::jsonb, -- For user preferences like theme, reading settings
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
  )
  *   **หมายเหตุ `profiles`:** การสร้าง record อัตโนมัติเมื่อมี user ใหม่ใน `auth.users` (ผ่าน Trigger หรือ Edge Function) เป็นสิ่งสำคัญ เพื่อให้ Admin สามารถกำหนด `role_id` ได้ทันที

- **`categories`** (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL UNIQUE,
  slug TEXT UNIQUE NOT NULL,
  description TEXT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
  )

- **`novels`** (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  title TEXT NOT NULL,
  slug TEXT UNIQUE NOT NULL,
  synopsis TEXT, -- Rich text content (HTML from TipTap)
  cover_image_url TEXT, -- Path in Supabase Storage
  category_id INTEGER REFERENCES categories(id) ON DELETE SET NULL,
  original_language TEXT NOT NULL, -- 'zh', 'en', 'th'
  status TEXT DEFAULT 'ongoing', -- 'ongoing', 'completed', 'halted'
  visibility JSONB DEFAULT '["everyone"]'::jsonb,
  default_chapter_status TEXT DEFAULT 'published',
  default_chapter_access JSONB DEFAULT '["everyone"]'::jsonb,
  created_by UUID NOT NULL REFERENCES profiles(id), -- Stores user's UUID
  total_chapters INTEGER DEFAULT 0, -- Denormalized, update with trigger on chapters table
  last_chapter_updated_at TIMESTAMP WITH TIME ZONE NULL, -- Denormalized, update with trigger
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
  )

- **`chapters`** (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  novel_id UUID NOT NULL REFERENCES novels(id) ON DELETE CASCADE,
  chapter_number TEXT NOT NULL, -- Stores number as TEXT, potentially with padding (e.g., "001", "123")
  title TEXT NOT NULL,
  content_original TEXT, -- Original language content (HTML from TipTap or Plain Text from .txt)
  status TEXT DEFAULT 'published',
  access_level JSONB DEFAULT '["everyone"]'::jsonb,
  created_by UUID NOT NULL REFERENCES profiles(id),
  word_count INTEGER, -- Calculated on save
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(novel_id, chapter_number)
  )

- **`user_chapter_reads`** (
  user_id UUID REFERENCES profiles(id) ON DELETE CASCADE,
  chapter_id UUID REFERENCES chapters(id) ON DELETE CASCADE,
  read_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (user_id, chapter_id)
  )

- **`user_bookmarks`** (
  id SERIAL PRIMARY KEY,
  user_id UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  chapter_id UUID NOT NULL REFERENCES chapters(id) ON DELETE CASCADE,
  novel_id UUID NOT NULL REFERENCES novels(id) ON DELETE CASCADE,
  bookmarked_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(user_id, chapter_id)
  )

- **`mark_lines`** (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  chapter_id UUID NOT NULL REFERENCES chapters(id) ON DELETE CASCADE,
  -- Denormalized fields (อ้างอิงจาก data.csv และข้อกำหนดเดิม):
  user_email TEXT, -- From profiles (via auth.users)
  user_display_name TEXT, -- From profiles
  novel_id UUID NOT NULL REFERENCES novels(id) ON DELETE CASCADE, -- Redundant if chapter_id is present, but useful for queries
  novel_title TEXT, -- From novels
  chapter_number TEXT, -- From chapters
  chapter_title TEXT, -- From chapters
  -- Core data:
  marked_line_numbers JSONB NOT NULL DEFAULT '[]'::jsonb,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(user_id, chapter_id)
  )
  *   **หมายเหตุ `mark_lines`:**
      *   การ denormalize ข้อมูล (`user_email`, `user_display_name`, `novel_title`, `chapter_title`, `chapter_number`) ต้องมีกลไก (Database Triggers หรือ Application-level logic ใน Next.js API Route ที่เกี่ยวข้องกับการอัปเดตข้อมูลต้นทาง) ในการอัปเดตเมื่อข้อมูลต้นทางมีการเปลี่ยนแปลง เพื่อรักษาความสอดคล้องของข้อมูล หรือพิจารณา JOIN เมื่อ query ถ้า performance รับได้
      *   **การอัปเดต `marked_line_numbers` ต้องเป็นแบบ Real-time** จากฝั่ง Client ผ่าน API Route ที่มีการป้องกันสิทธิ์อย่างเหมาะสม

- **`announcements`** (
  id SERIAL PRIMARY KEY, title TEXT NOT NULL, content TEXT NOT NULL, -- Markdown content
  start_date TIMESTAMP WITH TIME ZONE, end_date TIMESTAMP WITH TIME ZONE,
  target_roles JSONB DEFAULT '["Guest", "Member", "Translator", "Admin"]'::jsonb,
  target_pages JSONB DEFAULT '["homepage"]'::jsonb,
  is_active BOOLEAN DEFAULT TRUE, created_by UUID REFERENCES profiles(id),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
  )

- **`site_settings`** (
  key TEXT PRIMARY KEY, value JSONB, description TEXT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
  )

- **`site_changelogs`** (
  id SERIAL PRIMARY KEY, version_number TEXT NOT NULL UNIQUE, release_date DATE NOT NULL,
  summary TEXT, -- Markdown content
  is_published BOOLEAN DEFAULT FALSE, created_by UUID REFERENCES profiles(id),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
  )

- **`novel_versions`** (
  id SERIAL PRIMARY KEY, novel_id UUID NOT NULL REFERENCES novels(id) ON DELETE CASCADE,
  version_number INTEGER NOT NULL, metadata_snapshot JSONB NOT NULL,
  created_by UUID REFERENCES profiles(id), notes TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(novel_id, version_number)
  )

- **`chapter_versions`** (
  id SERIAL PRIMARY KEY, chapter_id UUID NOT NULL REFERENCES chapters(id) ON DELETE CASCADE,
  version_number INTEGER NOT NULL, content_original_snapshot TEXT,
  created_by UUID REFERENCES profiles(id), notes TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(chapter_id, version_number)
  )

- **การรักษาความปลอดภัย (Row-Level Security - RLS):**
  - RLS จะถูกนำมาใช้อย่างเข้มงวดใน **ทุกตาราง**
  - Policies จะอ้างอิง `auth.uid()`, `auth.role()`, และข้อมูลจาก `profiles` (เช่น `profiles.role_id`)
  - เมื่อแสดงผลข้อมูล `created_by` (UUID) ใน UI จะต้อง JOIN กับ `profiles` table เพื่อดึง `display_name` มาแสดง (ทำใน query ของ Supabase ที่เรียกจาก API Route หรือ `getServerSideProps`)

## 8. Supabase Storage Buckets

(เหมือนเดิมกับที่ระบุไว้ก่อนหน้า)
- **`novel-covers`:** (Public: true) สำหรับเก็บรูปปกนิยาย
- **`chapter-text-files`:** (Public: false, restricted access) สำหรับ Batch Upload ไฟล์ .txt (อาจเป็น temporary storage และมี policy ลบไฟล์หลังประมวลผล หรือหลังผ่านไประยะหนึ่ง)
- **`avatars`:** (Public: true) สำหรับเก็บรูปโปรไฟล์ผู้ใช้
- **`site_assets`:** (Public: true) สำหรับเก็บโลโก้เว็บไซต์, favicon, และไฟล์ static อื่นๆ ที่ใช้แสดงผลบนเว็บไซต์ (ถ้าไม่ deploy ผ่าน CDN หรือ `public` folder ของ Next.js โดยตรง)

## 9. สรุป Tech Stack (Tech Stack Summary)

- **Frontend Framework:** **Next.js (React)**
- **Styling:** **Tailwind CSS**
- **UI Component Library:** **Hero UI**
- **Form Handling & Validation:** **React Hook Form** และ **Zod**
- **Icon Library:** **Font Awesome Icons**
- **State Management (Optional, สำหรับ Global State ที่ซับซ้อน):** **Zustand** (หรือ React Context API + `useReducer` / Redux Toolkit หากทีมคุ้นเคย)
- **Data Fetching & Server State Management:** **React Query (TanStack Query)** (หรือ SWR หากต้องการตัวเลือกที่เบากว่าเล็กน้อยและพัฒนาโดย Vercel; หรือใช้ Next.js built-in data fetching methods (`getServerSideProps`, `getStaticProps`, client-side `useEffect` + `fetch`/`axios`) สำหรับกรณีที่ไม่ซับซ้อน)
- **Backend & Database Platform:** **Supabase**
  - **Database:** PostgreSQL
  - **Authentication:** Supabase Auth
  - **File Storage:** Supabase Storage
  - **Serverless Functions:** Supabase Edge Functions (สำหรับ Logic ฝั่ง Server ที่ซับซ้อน, การประมวลผลไฟล์, tasks ที่ต้องการ `service_role` key เช่น การจัดการ RLS, Triggers สร้าง `profiles` record, หรือการอัปเดตข้อมูล denormalized) หรือ Next.js API Routes สำหรับ Logic ส่วนใหญ่
- **ภาษา:** **TypeScript** (สำหรับทั้ง Next.js frontend/backend (API Routes) และ Supabase Edge Functions ถ้าใช้)
- **Rich Text Editor:** **TipTap** (integrated with React Hook Form)
- **Image Cropping:** **`react-image-crop`** (หรือ library อื่นที่เข้ากับ React)
- **ZIP file generation (Client-side):** **JSZip**

## 10. การจัดการข้อผิดพลาดเบื้องต้น (Basic Error Handling)

- **การตรวจสอบข้อมูลฝั่งผู้ใช้ (Client-Side Validation):**
  - ใช้ Zod schemas ร่วมกับ React Hook Form เพื่อตรวจสอบข้อมูลในฟอร์มต่างๆ ก่อนส่ง
  - แสดงข้อความข้อผิดพลาดที่ชัดเจนใต้ฟิลด์ที่มีปัญหา (React Hook Form จัดการส่วนนี้ได้ดี)
- **การตรวจสอบข้อมูลฝั่งเซิร์ฟเวอร์ (Server-Side Validation):**
  - ใน Next.js API Routes หรือ Supabase Edge Functions, **ต้องมีการตรวจสอบข้อมูลที่รับเข้ามาอีกครั้งเสมอ** ด้วย Zod schemas
- **การแสดงข้อความข้อผิดพลาดแก่ผู้ใช้ (User-Facing Error Messages):**
  - เมื่อเกิดข้อผิดพลาดจาก Server, แสดง **Hero UI Alert (Toast notification)** หรือข้อความใน UI ที่เป็นมิตร
  - ใช้ React Error Boundaries (Component `componentDidCatch` หรือ `getDerivedStateFromError` ใน Class Components, หรือ custom hook สำหรับ Function Components) เพื่อจัดการ Uncaught errors ใน UI และแสดง Fallback UI
- **การจัดการข้อผิดพลาดในโค้ด (Code-Level Error Handling):**
  - ใน Next.js API Routes และ `getServerSideProps`/`getStaticProps`: ใช้ `try...catch` blocks และคืน HTTP status code ที่เหมาะสมพร้อม error message
  - ในการเรียก Supabase API: ตรวจสอบ `{ data, error }` object ที่ Supabase client คืนค่ามา
  - React Query และ SWR มีกลไกจัดการ error state ในตัว
- **การบันทึกข้อผิดพลาด (Error Logging):**
  - **ระหว่างการพัฒนา:** `console.log`, `console.warn`, `console.error`
  - **บน Production:** Supabase Logs, พิจารณาใช้บริการ Error Tracking ภายนอก (เช่น Sentry, Logtail) โดย integrate กับ Next.js

## 11. มาตรการความปลอดภัยเบื้องต้น (Basic Security Measures)

- **การยืนยันตัวตน (Authentication):**
  - ใช้ Supabase Auth อย่างเต็มรูปแบบ (secure password hashing, session management)
  - เปิดใช้งาน Email Verification สำหรับการลงทะเบียนใหม่
  - พิจารณา Multi-Factor Authentication (MFA/2FA) สำหรับบัญชี Admin (Supabase Auth รองรับ TOTP)
- **การกำหนดสิทธิ์ (Authorization) - Row-Level Security (RLS):**
  - **หัวใจสำคัญที่สุดในการป้องกันการเข้าถึง/แก้ไข/ลบข้อมูลโดยไม่ได้รับอนุญาต**
  - กำหนด RLS policies ใน **ทุกตาราง** ที่มีข้อมูลสำคัญ โดยอ้างอิง `auth.uid()` และ `profiles.role_id`
- **การตรวจสอบข้อมูลนำเข้า (Input Validation):**
  - ใช้ Zod schemas ตรวจสอบข้อมูลทุกชนิด **ทั้งฝั่ง Client และ Server**
  - **Sanitize HTML output:** หากมีการใช้ `dangerouslySetInnerHTML` กับข้อมูลที่มาจากผู้ใช้ (เช่น คำโปรย, เนื้อหานิยายจาก Rich Text Editor), ต้อง Sanitize ด้วย DOMPurify หรือ library ที่คล้ายกันก่อน render เพื่อป้องกัน XSS
- **การใช้ HTTPS:** ให้บริการผ่าน HTTPS เสมอ (Deployment platforms ส่วนใหญ่จัดการส่วนนี้)
- **การจัดการ Environment Variables:**
  - เก็บ Supabase URL, Anon Key, และ **Service Role Key** (`SUPABASE_SERVICE_ROLE_KEY`) ไว้ใน Environment Variables (`.env.local` สำหรับ Next.js, และตั้งค่าใน deployment platform)
  - **`NEXT_PUBLIC_...` variables:** ปลอดภัยที่จะใช้ใน client-side code
  - **`SERVICE_ROLE_KEY`:** **ห้ามใช้ใน client-side code โดยเด็ดขาด** ใช้เฉพาะใน Server-side code ที่ปลอดภัย (Next.js API Routes, `getServerSideProps`, Supabase Edge Functions)
- **การป้องกัน CSRF (Cross-Site Request Forgery):**
  - Next.js API Routes (เมื่อใช้กับ Form submissions) ควรมีการป้องกัน CSRF เช่น การตรวจสอบ `Origin` header, การใช้ SameSite cookies, หรือการใช้ CSRF tokens (อาจต้อง implement เองหรือใช้ library)
- **การตั้งค่า HTTP Headers เพื่อความปลอดภัย:**
  - (เช่น Content Security Policy - CSP, X-Content-Type-Options, X-Frame-Options, Strict-Transport-Security) สามารถตั้งค่าผ่าน `next.config.js` (headers function) หรือ reverse proxy
- **การสำรองข้อมูล (Data Backups):**
  - Supabase มีระบบสำรองข้อมูลอัตโนมัติ ตรวจสอบนโยบายและตั้งค่าตาม plan
  - พิจารณาการทำ Manual Backups (เช่น `pg_dump`) เป็นระยะ
- **การจำกัดสิทธิ์การเข้าถึง (Principle of Least Privilege):**
  - ผู้ใช้แต่ละบทบาทควรมีสิทธิ์เท่าที่จำเป็น
  - บัญชี Admin ควรใช้สำหรับการดูแลระบบเท่านั้น
- **การอัปเดต Software และ Dependencies:** หมั่นตรวจสอบและอัปเดต Next.js, Supabase client libraries, Node.js, และ dependencies อื่นๆ (ใช้ `npm audit` หรือ `yarn audit`)

---

หวังว่าการปรับปรุงนี้จะตรงตามความต้องการของคุณอย่างครบถ้วนนะครับ!
