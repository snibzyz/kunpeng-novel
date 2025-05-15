# INKREALM.CO: ข้อกำหนดระบบและคุณสมบัติหลัก (V5.1) - ฉบับปรับปรุง README (Next.js & Supabase Stack with Chakra UI)

เอกสารนี้เป็นข้อกำหนดระบบและคุณสมบัติหลักสำหรับแพลตฟอร์ม INKREALM.CO (V5.1) โดยเน้นฟังก์ชันที่จำเป็นตามการปรับปรุงล่าสุด โดยอ้างอิง Tech Stack ใหม่ (Next.js (React), Supabase, **Chakra UI**, และ **Font Awesome Icon**) และออกแบบมาเพื่อความสะดวกในการปรับใช้, ทดสอบ, และย้ายไปยังโดเมนต่างๆ

---

**ลำดับความสำคัญในการพิจารณาและพัฒนา (โดยรวม):**

1.  **ความปลอดภัยของระบบและข้อมูล (Security & Data Integrity):** การยืนยันตัวตน, การกำหนดสิทธิ์ (RLS), การป้องกันข้อมูลรั่วไหล, การตรวจสอบข้อมูลนำเข้า, การสำรองข้อมูล
2.  **โครงสร้างฐานข้อมูล (Database Structure):** ความถูกต้อง, ความครบถ้วน, ความสัมพันธ์ของข้อมูล, การรองรับการขยายตัว และการ query ที่มีประสิทธิภาพ (รวมถึงการ denormalize ที่จำเป็นอย่าง `mark_lines`)
3.  **ระบบหลัก (Core Systems):** ระบบยืนยันตัวตน, ระบบจัดการเนื้อหานิยาย (CMS), ระบบการอ่านและบุ๊คมาร์ค, ระบบเครื่องมือสำหรับนักแปล (เน้น Real-time update สำหรับ `mark_lines`)
4.  **ประสบการณ์ผู้ใช้ (User Experience - UX):** การใช้งานง่าย, ความต่อเนื่อง, การตอบสนองของระบบ, การจัดการข้อผิดพลาดที่เป็นมิตร
5.  **การออกแบบส่วนติดต่อผู้ใช้ (User Interface - UI):** ความสวยงาม, การอ่านง่าย, ความสอดคล้อง, Responsive Design (ด้วย Chakra UI)
6.  **คุณสมบัติเสริมและส่วนอำนวยความสะดวก (Enhancements & Utilities):** ระบบประกาศ, แดชบอร์ด, การจัดการเว็บไซต์, การจัดการเวอร์ชัน

---

## 1. ปรัชญาการออกแบบโดยรวม

- **การแสดงผลหรูหราและอ่านง่าย:**
  - **โทนสีหลัก:** สีทอง (เช่น `#FFD700`, `#B8860B` หรือโทนใกล้เคียงที่ให้ความรู้สึกหรูหรา)
  - **สีรอง:** สีเทา (หลายเฉด เช่น `#F0F0F0`, `#CCCCCC`, `#808080`, `#333333`)
  - **โหมดสว่าง (Light Mode):** เน้นสีทอง, เทาอ่อน, และขาว เพื่อความสบายตา ดูสะอาด และหรูหรา (Chakra UI theme)
  - **โหมดมืด (Dark Mode):** เน้นสีทอง (อาจปรับให้สว่างขึ้นเล็กน้อยเพื่อคอนทราสต์), เทาเข้ม, และดำ เพื่อลดแสงสะท้อนและช่วยให้อ่านง่ายในที่มืด (Chakra UI theme)
  - **โลโก้:** พิจารณาใช้การไล่ระดับสี (gradient) ที่ผสมผสานสีทองและสีเทาอย่างลงตัว หรือสีทองล้วนบนพื้นหลังที่เหมาะสม เพื่อความโดดเด่นและทันสมัย
  - **ฟอนต์หลัก:** Sarabun สำหรับการแสดงผลภาษาไทยที่สวยงามและอ่านง่าย; พิจารณาฟอนต์สำรองสำหรับภาษาอังกฤษที่เข้ากัน (เช่น Noto Sans, Open Sans)
- **เครื่องมือสำหรับนักแปล:** มีฟังก์ชันสนับสนุนการทำงานของนักแปลโดยเฉพาะ เน้นความชัดเจนและประสิทธิภาพ
- **ใช้งานง่าย ตอบโจทย์ทุกอุปกรณ์:**
  - UI ใช้งานง่าย (Intuitive) และสอดคล้องกันทั้งระบบ
  - Responsive Design เพื่อประสบการณ์ที่ดีในทุกขนาดหน้าจอ (ใช้ **Chakra UI** components และ style props)
  - หน้าอ่านนิยายไม่มี Horizontal Scroll โดยเด็ดขาด
- **ความยืดหยุ่นและการทดสอบ:** โครงสร้างที่เอื้อต่อการเปลี่ยนแปลงโดเมนและการทดสอบระบบที่ง่ายดาย
- **การเปลี่ยนธีม:**
  - การสลับระหว่างโหมดสว่างและโหมดมืดจะต้องราบรื่น (อาจมี transition CSS เบาๆ)
  - สีที่แสดงผลในแต่ละโหมดต้องไม่ผิดเพี้ยนจากที่ออกแบบไว้ และมีคอนทราสต์ที่เหมาะสมสำหรับการอ่าน
  - ควบคุมผ่านองค์ประกอบ UI ที่ชัดเจน (เช่น ไอคอนพระอาทิตย์/พระจันทร์ จาก **Font Awesome Icon** ที่มีการเปลี่ยนแปลงสถานะชัดเจน) และสถานะธีมต้องถูกบันทึกไว้ในการตั้งค่าของผู้ใช้ (`localStorage` และ/หรือ `profiles.user_settings` ใน Supabase) ใช้ **Chakra UI** color mode functionality

## 1.1 ภาพรวมระบบหลัก (Core System Overview)

- **ระบบยืนยันตัวตนและจัดการผู้ใช้:** ลงทะเบียน, เข้าสู่ระบบ (อีเมล/รหัสผ่าน, Google OAuth ผ่าน Supabase Auth), ลืมรหัสผ่าน, จัดการโปรไฟล์ (รวมถึงการเปลี่ยนรูปโปรไฟล์, ชื่อที่แสดง, รหัสผ่าน), การจัดการบทบาทและสิทธิ์ (Admin ควบคุมผ่าน UI และ RLS ใน Supabase) **เมื่อผู้ใช้ลงทะเบียนหรือเข้าสู่ระบบครั้งแรกผ่าน Social OAuth และยังไม่มีข้อมูลในตาราง `profiles`, ระบบ (ผ่าน Supabase Trigger บน `auth.users`) จะสร้าง record ใน `profiles` โดยอัตโนมัติ เพื่อให้ Admin สามารถกำหนดสิทธิ์ได้ทันที**
- **ระบบจัดการเนื้อหานิยาย (CMS):**
  - การสร้าง/แก้ไข/ลบนิยายและตอน (ดำเนินการผ่าน **Chakra UI Modal** เป็นหลัก เพื่อ UX ที่ต่อเนื่อง)
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
  - **ระบบบรรทัดมาร์ค (Mark Lines): บันทึกบรรทัดที่นักแปลเลือก (Real-time update ไปยัง Supabase ผ่าน Supabase Realtime subscriptions และ React Query mutations)**
  - หน้าจัดการบรรทัดมาร์ค: ดูภาพรวม, คัดลอก, ดาวน์โหลดมาร์ค
- **ระบบแดชบอร์ด (สร้างด้วย Next.js Pages/Components และ Chakra UI):**
  - **สำหรับนักแปล:** ภาพรวมนิยายที่ตนเองจัดการ, ทางลัดไปหน้าจัดการบรรทัดมาร์ค
  - **สำหรับผู้ดูแลระบบ:** ภาพรวมระบบ, จัดการผู้ใช้, จัดการหมวดหมู่, จัดการประกาศ, จัดการเว็บไซต์ (การตั้งค่าทั่วไป), จัดการเวอร์ชัน (เว็บไซต์, นิยาย, ตอน)
- **ระบบฐานข้อมูลและไฟล์:** Supabase (PostgreSQL สำหรับฐานข้อมูล, Supabase Storage สำหรับไฟล์)
- **ระบบการแจ้งเตือนผู้ใช้ (User Feedback System):** การใช้ Toast notifications (เช่น **Chakra UI `useToast` hook**) สำหรับยืนยันการกระทำ, แจ้งเตือนข้อผิดพลาด, หรือให้ข้อมูลสั้นๆ
- **ระบบประกาศ (Announcement System):** สำหรับ Admin ในการสร้างและเผยแพร่ประกาศไปยังกลุ่มผู้ใช้เป้าหมายและหน้าที่กำหนด

## 1.2 โครงสร้าง URL ที่เป็นมิตรและการจัดการโดเมน (User-Friendly URL Structure & Domain Management)

(ส่วนนี้ยังคงเหมือนเดิม เนื่องจากไม่ขึ้นกับ UI Library โดยตรง)
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
  - **แถบเครื่องมือสำหรับอ่าน (Reading Toolbar Component - ใช้ Chakra UI `Flex`, `IconButton`, `Menu`):** สารบัญ (เปิด **Chakra UI Modal** แสดงรายการตอน), ปรับแต่งการแสดงผล (เปิด **Chakra UI Popover** หรือ **Menu**), บุ๊คมาร์คตอนปัจจุบัน (สลับสถานะ), สลับธีมเว็บ (Light/Dark), เปิด/ปิดโหมดอ่านต่อเนื่อง, นำทางตอนก่อนหน้า/ถัดไป
- **นักแปล (Translator):**
  - **ความสามารถหลัก:**
    - สิทธิ์ทั้งหมดของสมาชิก
    - สามารถเปิด/ปิด "โหมดนักแปล" สำหรับนิยายที่ตนเองได้รับสิทธิ์แปล (สถานะนี้อาจเก็บใน localStorage หรือ `user_preferences` table และใช้ในการ render UI ที่ต่างกันในหน้าอ่าน)
    - เข้าถึง "โหมดแปล" (เมื่อเปิดใช้งานสำหรับนิยายนั้น): แสดงเลขบรรทัด, **Chakra UI Checkbox**, ดับเบิลคลิกที่บรรทัดเนื้อหาเพื่อมาร์คและคัดลอก
    - ใช้งานระบบบรรทัดมาร์ค (Mark Lines) และบันทึกข้อมูลไปยัง Supabase ทันที (**Real-time Sync**)
    - สร้าง/แก้ไข/ลบนิยายและตอนของตนเอง (นิยายที่ `created_by` ตรงกับ `user_id` ของตน หรือได้รับสิทธิ์จัดการผ่านตาราง permissions แยกต่างหาก ถ้ามี) การดำเนินการผ่าน **Chakra UI Modal**
    - เข้าถึงแดชบอร์ดนักแปล (`/translator/dashboard`) และหน้าจัดการบรรทัดมาร์คสำหรับนิยายของตน
  - **แถบเครื่องมือพิเศษในโหมดแปล (เมื่อเปิด "โหมดนักแปล" สำหรับนิยายนั้น และเข้าหน้าอ่านตอน):** เหมือนของ Member แต่เพิ่ม: ปุ่ม "คัดลอกมาร์คทั้งหมด" (Copy All Marks) สำหรับตอนปัจจุบัน (ใช้ **Font Awesome Icon** เช่น `faCopy` หรือ `faClipboardCheck`)
- **ผู้ดูแลระบบ (Admin):**
  - **ความสามารถหลัก:**
    - สิทธิ์ทั้งหมดของนักแปล (รวมถึงการสร้าง/จัดการนิยายและตอนใดๆ ในระบบ โดย RLS จะอนุญาตถ้า role เป็น Admin)
    - เข้าถึงแดชบอร์ดผู้ดูแลระบบ (`/admin/dashboard`) ซึ่งมี Layout และการป้องกัน Route เฉพาะ Admin
    - จัดการผู้ใช้ทั้งหมด: ดูรายการ, เข้าดู/แก้ไขโปรไฟล์ (ชื่อที่แสดง, รูปโปรไฟล์), เปลี่ยนบทบาท, กำหนดวันหมดอายุสิทธิ์, ระงับบัญชี (ผ่านหน้า User Management `/admin/dashboard/users` และหน้าย่อย `/admin/dashboard/users/[user_id]` หรือ **Chakra UI Modal**)
    - ดูข้อมูลบรรทัดมาร์คทั้งหมดของนักแปลทุกคน (อาจมีหน้าเฉพาะสำหรับ Admin หรือส่วนขยายในหน้าจัดการบรรทัดมาร์ค)
    - การจัดการหมวดหมู่นิยาย: เพิ่ม/แก้ไข/ลบหมวดหมู่
    - การจัดการประกาศ: สร้าง/แก้ไข/ลบ/เผยแพร่ประกาศ
    - การจัดการเว็บไซต์: แก้ไขการตั้งค่าทั่วไปของเว็บไซต์ (ผ่านหน้า Site Management)
    - การจัดการเวอร์ชัน: ดูและกู้คืนเวอร์ชันของเว็บไซต์ (Changelog), นิยาย (metadata), และตอน (content)

## 3. การเข้าสู่ระบบ / ลงทะเบียน / ลืมรหัสผ่าน (Authentication Modals & Pages)

- **ความสามารถ:** ลงทะเบียนผู้ใช้ใหม่, เข้าสู่ระบบด้วยอีเมล/รหัสผ่าน หรือ Google OAuth (ใช้ Supabase Auth UI Helpers หรือสร้าง Form เอง), กระบวนการลืมและรีเซ็ตรหัสผ่าน (Supabase จัดการการส่งอีเมล)
- **การแสดงผล:**
  - แสดงผลผ่าน **Chakra UI Modal** เป็นหลักเมื่อผู้ใช้คลิกปุ่ม "เข้าสู่ระบบ" หรือ "ลงทะเบียน" จากภายใน Navbar หรือส่วนอื่นๆ ของเว็บที่ผู้ใช้ยังไม่ได้ล็อกอิน
  - มีหน้าเว็บเฉพาะ (เช่น `pages/login.tsx`, `pages/register.tsx`, `pages/forgot-password.tsx`, `pages/reset-password.tsx`) สำหรับการเข้าถึงโดยตรง
- **การพัฒนา:**
  - **Backend:** Supabase Auth (จัดการ user accounts, sessions, social logins, password reset emails). **เมื่อผู้ใช้ลงทะเบียนหรือเข้าสู่ระบบครั้งแรกผ่าน Social OAuth และยังไม่มีข้อมูลในตาราง `profiles`, ระบบ (ผ่าน Supabase Trigger บน `auth.users`) จะสร้าง record ใน `profiles` โดยอัตโนมัติ**
  - **Frontend (Next.js):**
    - สร้าง Forms ด้วย React Components และ **Chakra UI** (`Input`, `Button`, `FormControl`, `FormLabel`, `FormErrorMessage` etc.)
    - จัดการ Form State และ Validation ด้วย **React Hook Form** และ **Zod**
    - เรียกใช้ Supabase Auth client methods (`signInWithPassword`, `signUp`, `signInWithOAuth`, `resetPasswordForEmail`) จาก Event Handlers ใน Components
    - จัดการสถานะการโหลด (Loading state - Chakra UI `Spinner`) และแสดงข้อผิดพลาดจาก Supabase Auth
    - อาจใช้ React Context API หรือ Zustand เพื่อจัดการ Global Authentication State
- **การแจ้งเตือน (User Feedback):** แสดง Toast notification (เช่น "เข้าสู่ระบบสำเร็จ", "ลงทะเบียนเรียบร้อย กรุณายืนยันอีเมล") โดยใช้ **Chakra UI `useToast` hook**.

## 4. โครงสร้างแถบนำทาง (Navbar Structure)

- **ความสามารถ:** แสดงเมนูแบบไดนามิกตามบทบาทของผู้ใช้ (Guest, Member, Translator, Admin), Responsive Design (ปรับการแสดงผลสำหรับ Mobile เช่น ใช้ **Chakra UI `Menu`** หรือ **`Drawer`** สำหรับเมนูแบบ Hamburger)
- **ตัวอย่างเมนู (สร้างเป็น React Component ใช้ Chakra UI `Flex`, `Link`, `Button`, `Menu`, `Avatar`):**
  - **Guest:**
    - โลโก้ INKREALM.CO (ใช้ `<Link href="/">` จาก `next/link` ห่อหุ้ม Chakra UI `Image` หรือ `Text`)
    - (อาจมีลิงก์ "นิยายทั้งหมด" หรือ "หมวดหมู่")
    - ปุ่มสลับธีม (ไอคอน **Font Awesome** `faSun`/`faMoon` ใน Chakra UI `IconButton`)
    - ปุ่ม "เข้าสู่ระบบ" (**Chakra UI Button**, เปิด Login Modal)
    - ปุ่ม "ลงทะเบียน" (**Chakra UI Button**, เปิด Register Modal)
  - **Member:**
    - โลโก้ INKREALM.CO
    - `<Link href="/">หน้าแรก</Link>` (Chakra UI `Link` component from Next.js `Link`)
    - `<Link href="/bookmarks">บุ๊คมาร์ค</Link>`
    - ปุ่มสลับธีม
    - โปรไฟล์ผู้ใช้ (**Chakra UI `Menu`** หรือ **`Avatar`** + **`Popover`** เมื่อคลิกที่ Avatar/ชื่อผู้ใช้):
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
  - ใช้ **Chakra UI** Components (`Flex`, `HStack`, `Menu`, `Button`, `Avatar`, `Drawer`)
  - ดึงข้อมูลสถานะผู้ใช้และบทบาทจาก Global State (เช่น Zustand store หรือ React Context)

## 5. รายละเอียดหน้าเว็บไซต์, Modals, และการจัดการเนื้อหา

### 5.1 หน้าแรก (Home Page - `pages/index.tsx`)

- **ส่วนแสดงประกาศ (Announcements Display Area - React Component):**
  - ดึงข้อมูลประกาศจาก Supabase (React Query หรือ `getServerSideProps`)
  - แสดงผลในบริเวณที่เห็นได้ชัดเจน (เช่น Banner ด้านบน) ด้วย **Chakra UI `Alert`** หรือ **`Card`**, มีปุ่ม "X" (ไอคอน **Font Awesome** `faTimes` ใน `IconButton`) ให้ผู้ใช้ปิดประกาศ
- **ส่วนเครื่องมือหลัก (React Component):**
  - **ช่องค้นหานิยาย:** (**Chakra UI `Input`**)
  - **ฟิลเตอร์หมวดหมู่ (Category Filter):** (**Chakra UI `Select`**)
  - **ฟิลเตอร์สถานะนิยาย (Novel Status Filter):** (**Chakra UI `Select`**)
  - **ปุ่มสร้างนิยาย (Create Novel Button):** (สำหรับ Translator/Admin) (**Chakra UI Button**) ประกอบด้วย [ไอคอน **Font Awesome** เช่น `faPlusSquare` หรือ `faFileMedical`] + ข้อความ "สร้างนิยาย". เมื่อคลิก เปิด Modal สร้างนิยาย (ดูข้อ 5.1.1)
- **การแสดงรายการนิยาย (React Component):**
  - รูปแบบ Grid Layout (ใช้ **Chakra UI `SimpleGrid`**)
  - **การ์ดนิยาย (**Chakra UI `Card`**):** แสดง: **รูปปก (อัตราส่วน 3:4, ใช้ `next/image` ภายใน Chakra UI `Box`), ชื่อเรื่อง, จำนวนตอนทั้งหมด**
  - Responsive: PC (เช่น 4-5 คอลัมน์), Tablet (3 คอลัมน์), Mobile (1-2 คอลัมน์)
  - ใช้ `<Link href={`/novels/${novel.id}`}>` ครอบการ์ด
- **ระบบแบ่งหน้า (Pagination):** (**Chakra UI Pagination Component** หรือสร้างเอง)
- **การพัฒนา:**
  - **Data Fetching:** React Query หรือ `getServerSideProps` / `getStaticProps`
  - **UI Components:** **Chakra UI** (`Input`, `Select`, `Button`, `Card`, `SimpleGrid`, Pagination components)
  - **State Management:** Zustand หรือ React Context

### 5.1.1 Modal สร้างนิยาย (Create Novel Modal)

- **การเรียกใช้งาน:** คลิกปุ่ม "[ไอคอน] สร้างนิยาย" บนหน้าแรก
- **UI ภายใน Modal (**Chakra UI `Modal`** Component):**
  - **หัวเรื่อง Modal:** "สร้างนิยายใหม่" (Chakra UI `ModalHeader`)
  - **ฟอร์ม (จัดการด้วย React Hook Form, Validation ด้วย Zod, ใช้ Chakra UI Form Components):**
    - **ชื่อเรื่อง (Title):** (**Chakra UI `Input`**)
    - **คำโปรย (Synopsis):** **Rich Text Editor** (เช่น TipTap ผูกกับ Controller ของ React Hook Form)
    - **หมวดหมู่ (Category):** (**Chakra UI `Select`**)
    - **รูปปกนิยาย (Cover Image):** Chakra UI `Input type="file"`, แสดง Preview, เครื่องมือ Crop (react-image-crop)
    - **ภาษาต้นฉบับ (Original Language):** (**Chakra UI `Select`**)
    - **Slug นิยาย (`novel_slug`):** (**Chakra UI `Input`**)
    - **สถานะนิยาย (Novel Status):** (**Chakra UI `Select`** หรือ **`RadioGroup`**)
    - **สิทธิ์ในการมองเห็นนิยาย (Novel Visibility):** (**Chakra UI `Select` (multiple)** หรือ **`CheckboxGroup`**)
    - **การตั้งค่าเริ่มต้นสำหรับตอนใหม่ของนิยายนี้:**
      - **สถานะเริ่มต้นของตอน:** (**Chakra UI `RadioGroup`**)
      - **สิทธิ์การเข้าถึงตอนเริ่มต้น:** (**Chakra UI `Select` (multiple)** หรือ **`CheckboxGroup`**)
  - **ปุ่มดำเนินการ (**Chakra UI `Button`** ใน `ModalFooter`):** "เผยแพร่นิยาย", "บันทึกเป็นฉบับร่าง", "ยกเลิก"
- **การพัฒนา:**
  - **UI:** **Chakra UI** (`Modal`, `Input`, `Select`, `RadioGroup`, `CheckboxGroup`, `Button`, `FormControl`, `FormLabel`)
  - **Form:** React Hook Form, Zod
  - **Rich Text:** TipTap
  - **Image Crop:** `react-image-crop`
  - **Backend Logic:** Next.js API Route

### 5.2 หน้าข้อมูลนิยายและสารบัญ (Novel Detail Page - `pages/novels/[novel_id].tsx`)

- **Data Fetching:** `getServerSideProps` หรือ `getStaticProps`, หรือ React Query
- **ส่วนแสดงประกาศ (Announcements Display Area):** (เหมือนหน้าแรก)
- **ส่วนข้อมูลนิยาย (Novel Information Section - React Component):**
  - **ข้อมูลที่แสดง:** รูปปก (`next/image` ใน Chakra UI `Box`), ชื่อเรื่อง, หมวดหมู่, คำโปรย, จำนวนตอน, สถานะ, ภาษา, `novel_slug`
  - **(สำหรับนักแปล/Admin) ปุ่มสลับ "โหมดนักแปล":** (**Chakra UI `Switch`**)
- **ปุ่มจัดการนิยาย (สำหรับผู้สร้าง/Admin):**
  - ปุ่ม "แก้ไขข้อมูลนิยาย" (**Chakra UI Button**): เปิด Modal แก้ไขข้อมูลนิยาย (ข้อ 5.2.1)
  - ปุ่ม "ลบนิยาย" (**Chakra UI Button** colorScheme="red"): แสดง **Chakra UI `AlertDialog`**
- **ส่วนเพิ่มตอน (Add Chapter Section - สำหรับผู้สร้าง/Admin):**
  - ปุ่ม: "เพิ่มตอนด้วย .txt" (เปิด Modal 5.2.2)
  - ปุ่ม: "เพิ่มตอน" (เปิด Modal 5.2.3)
- **ส่วนสารบัญ (Table of Contents Section - React Component):**
  - **การแสดงผล:** (**Chakra UI `Accordion`**) หรือ List
  - **รายการตอน:**
    - **(สำหรับทุกคน):** ไอคอนบุ๊คมาร์ค (**Font Awesome**), หมายเลขตอน, ชื่อตอน (ลิงก์), สถานะการอ่าน (ไอคอน **Font Awesome** `faEye`)
    - **(สำหรับผู้สร้าง/Admin):** ไอคอนสถานะตอน, ปุ่ม "แก้ไขตอน" (ไอคอน **Font Awesome** `faPencilAlt`), ปุ่ม "ลบตอน" (ไอคอน **Font Awesome** `faTrashAlt`), ปุ่ม "คัดลอกมาร์คทั้งหมด" (ไอคอน **Font Awesome** `faCopy`)
- **การพัฒนา:** Next.js, **Chakra UI** (`Switch`, `Button`, `Accordion`, `Modal`, `AlertDialog`), **Font Awesome Icon**, Supabase, React Query, DOMPurify

### 5.2.1 Modal แก้ไขข้อมูลนิยาย (Edit Novel Modal)

- **UI ภายใน Modal (**Chakra UI `Modal`**):** ฟอร์ม (React Hook Form + Zod) เติมข้อมูลปัจจุบัน
  - **หัวเรื่อง Modal:** "แก้ไขข้อมูลนิยาย: [ชื่อนิยายปัจจุบัน]"
  - **ฟิลด์ (เหมือน 5.1.1, ใช้ Chakra UI Components):** Title, Synopsis, Category, Cover Image, Original Language, `novel_slug`, Novel Status, Novel Visibility, Default Chapter Settings.
  - **ปุ่มดำเนินการ (**Chakra UI `Button`**):** "บันทึกการเปลี่ยนแปลง", "ยกเลิก", (อาจมี) "ลบนิยาย" (แสดง **Chakra UI `AlertDialog`**)
- **การพัฒนา:** เหมือน 5.1.1 แต่ API สำหรับ update.

### 5.2.2 Modal เพิ่มตอนด้วย .txt (Batch Upload Modal)

- **UI ภายใน Modal (**Chakra UI `Modal`**):**
  - **หัวเรื่อง Modal:** "เพิ่มหลายตอนด้วยไฟล์ .txt..."
  - **พื้นที่ Drag & Drop หรือ ปุ่ม "เลือกไฟล์":** (**Chakra UI `Input type="file"`**, multiple)
  - **การจัดการ Chapter Number จากชื่อไฟล์**
  - **ตัวเลือกการจัดการไฟล์ชื่อซ้ำ:** (**Chakra UI `RadioGroup`**)
  - **Progress Bar (**Chakra UI `Progress`**):**
  - **ปุ่มดำเนินการ:** "เริ่มอัปโหลด", "ยกเลิก"
- **การทำงานเบื้องหลัง:** Next.js API Route.
- **การพัฒนา:** **Chakra UI** (`Modal`, `Input`, `Progress`, `RadioGroup`), Next.js API Route, Supabase.

### 5.2.3 Modal เพิ่ม/แก้ไขตอน (Create/Edit Chapter Modal)

- **UI ภายใน Modal (**Chakra UI `Modal`**):**
  - **หัวเรื่อง:** "เพิ่มตอนใหม่..." หรือ "แก้ไขตอน..."
  - **ฟอร์ม (React Hook Form + Zod, ใช้ Chakra UI Components):**
    - **ชื่อตอน (Chapter Title):** (**Chakra UI `Input`**)
    - **หมายเลขตอน (`chapter_number`):** (**Chakra UI `Input`**)
    - **เนื้อหา (`content_original`):** **Rich Text Editor** (TipTap)
    - **สถานะตอน (Chapter Status):** (**Chakra UI `RadioGroup`**)
    - **สิทธิ์การเข้าถึงตอน (Chapter Access Level):** (**Chakra UI `Select` (multiple)** หรือ **`CheckboxGroup`**)
  - **ปุ่มดำเนินการ (**Chakra UI `Button`**):** "เผยแพร่ตอน", "บันทึกเป็นฉบับร่าง", "บันทึกการเปลี่ยนแปลง", "ยกเลิก"
- **การทำงานเบื้องหลัง:** Next.js API Route.
- **การพัฒนา:** **Chakra UI** (`Modal`, `Input`, `RadioGroup`, `Select`, `CheckboxGroup`), React Hook Form + Zod, TipTap, Next.js API Route, Supabase.

### 5.3 หน้าบุ๊คมาร์ค (Bookmarks Page - `pages/bookmarks.tsx`)

- **Data Fetching:** React Query
- **ความสามารถ:**
  - แสดงรายการนิยายที่บุ๊คมาร์ค
  - **สำหรับแต่ละนิยาย (แสดงเป็น Chakra UI `Card` หรือ List Item):** รูปปก, ชื่อนิยาย, ชื่อตอนล่าสุดที่บุ๊คมาร์ค, ปุ่ม "ไปยังบุ๊คมาร์คล่าสุด" (**Chakra UI Button** หรือ `Link`), ปุ่ม "จัดการบุ๊คมาร์คสำหรับเรื่องนี้" (**Chakra UI Button**)
- **Modal "จัดการบุ๊คมาร์คสำหรับเรื่อง: [ชื่อนิยาย]" (**Chakra UI `Modal`**):**
  - **หัวเรื่อง, ตัวเลือกเรียง (**Chakra UI `Select`**), รายการบุ๊คมาร์ค (List พร้อม **Chakra UI `Checkbox`**), ปุ่ม "ลบรายการที่เลือก", ปุ่ม "ล้างบุ๊คมาร์คทั้งหมดของเรื่องนี้" (แสดง **Chakra UI `AlertDialog`**), ปุ่ม "ปิด"**
- **ปุ่ม "ลบบุ๊คมาร์คทั้งหมด (ทุกเรื่อง)":** (**Chakra UI Button** colorScheme="red", แสดง **Chakra UI `AlertDialog`**)
- **การพัฒนา:** Next.js Page, React Query, Supabase, **Chakra UI** (`Card`, `List`, `Modal`, `AlertDialog`, `Select`, `Checkbox`, `Button`), **Font Awesome Icon**

### 5.4 แดชบอร์ดนักแปล (Translator Dashboard - `pages/translator/dashboard.tsx`)

- **Data Fetching:** React Query
- **ความสามารถ:**
  - **การ์ดนิยาย (**Chakra UI `Card`**):** รูปปก, ชื่อนิยาย, ความคืบหน้าการมาร์ค (**Chakra UI `Progress`**)
  - **สำหรับแต่ละนิยาย:** ปุ่ม "จัดการบรรทัดมาร์ค" (**Chakra UI Button**), ปุ่ม "ไปยังหน้าข้อมูลนิยาย", (อาจมี) "เพิ่มตอนใหม่"
- **การพัฒนา:** Next.js Page, React Query, Supabase, **Chakra UI** (`Card`, `Progress`, `Button`)

### 5.4.1 หน้าจัดการบรรทัดมาร์ค (Mark Line Management Page - `pages/translator/manage-marks/[novel_id].tsx`)

- **Data Fetching:** `getServerSideProps` และ React Query
- **หัวเรื่อง, ส่วนควบคุมหลัก:** ปุ่ม "เลือกตอนเพื่อดำเนินการหลายรายการ" (**Chakra UI Button** หรือ **`Switch`**)
- **การแสดงผลสารบัญตอน (**Chakra UI `Accordion`** หรือ List):**
  - ปรับจำนวนตอน, หัวกลุ่มตอน, (**Chakra UI `Checkbox`** "เลือกทั้งหมดในกลุ่มนี้")
- **รายการตอน:** (**Chakra UI `Checkbox`**), หมายเลขตอน, ชื่อตอน, จำนวนมาร์ค, สถานะมาร์ค (ไอคอน **Font Awesome** `faCheckCircle`/`faCircle`), ปุ่ม "คัดลอกมาร์คตอนนี้" (ไอคอน **Font Awesome** `faCopy`), ปุ่ม "ไปที่หน้าอ่าน/แปลตอนนี้"
- **Batch Actions Toolbar (**Chakra UI `Flex` or custom Toolbar, แสดงเมื่อ Batch Mode):**
  - ข้อความ, ปุ่ม "คัดลอกบรรทัดมาร์คของตอนที่เลือก", ปุ่ม "ดาวน์โหลดไฟล์บรรทัดมาร์คของตอนที่เลือก" (เปิด **Chakra UI `Modal`** ตัวเลือกดาวน์โหลด: รูปแบบไฟล์ (**Chakra UI `RadioGroup`**))
- **การพัฒนา:** Next.js Page, React Query, **Chakra UI** (`Accordion`, `Checkbox`, `Button`, `Modal`, `Switch`, `RadioGroup`), **Font Awesome Icon**, Supabase, JSZip

### 5.5 หน้าอ่านนิยายและโหมดแปล (Reading & Translator Mode UI - `pages/novels/[novel_id]/[chapter_number].tsx`)

- **Data Fetching:** `getServerSideProps`
- **การแสดงผลเนื้อหา:** ชื่อตอน, เนื้อหานิยาย (CSS `text-indent`), Layout PC/Mobile, No Horizontal Scroll
- **ปุ่ม "Arrow Up":** (ไอคอน **Font Awesome** `faArrowUp`)
- **การควบคุมด้วยคีย์บอร์ด**
- **โหมดการอ่าน:**
  - **แถบเครื่องมือหลักด้านบน (Top Reading Toolbar - Chakra UI `Flex`, sticky):**
    - **PC Items:** "สารบัญ" (ไอคอน **Font Awesome** `faListUl`, เปิด **Chakra UI `Modal`**), "ปรับแต่งการแสดงผล (Aa)" (ไอคอน **Font Awesome** `faCog` หรือ `faSlidersH`, เปิด **Chakra UI `Popover`/`Menu`**), "บุ๊คมาร์คตอนนี้" (ไอคอน **Font Awesome** `faBookmark`), "สลับธีม" (ไอคอน **Font Awesome** `faSun`/`faMoon`), "โหมดอ่านต่อเนื่อง" (ไอคอน **Font Awesome** `faInfinity`), นำทางตอน
    - **Mobile Items:** คล้าย PC
  - **การนำทางตอน (Mobile):** แถบเครื่องมือด้านล่าง
  - **ท้ายบท:** ปุ่ม "ตอนต่อไป" หรือ Infinite Scroll
- **โหมดนักแปล:**
  - **แถบเครื่องมือหลักด้านบน:** เหมือนโหมดอ่าน + ปุ่ม "คัดลอกมาร์คทั้งหมด" (ไอคอน **Font Awesome** `faClipboardCheck`)
  - **การแสดงผลเนื้อหา:** แถบเลขบรรทัด, เนื้อหา + **Chakra UI `Checkbox`**
  - **MarkLine Interaction:** คลิก Checkbox (Real-time update, API call, Toast), ดับเบิลคลิกบรรทัด (ไฮไลท์, คัดลอก, มาร์ค, Toast)
  - **ท้ายบท (โหมดนักแปล):** สรุปมาร์ค, ปุ่ม "ล้างมาร์คทั้งหมดของตอนนี้" (**Chakra UI Button** colorScheme="red", **Chakra UI `AlertDialog`**), ปุ่ม "คัดลอกมาร์คทั้งหมดของตอนนี้"
- **การพัฒนา:**
  - Next.js Page, Zustand/React Context, React Logic, **Chakra UI** (`Modal`, `Popover`, `Menu`, `Button`, `Checkbox`, `Switch`, `AlertDialog`, `Flex`, `IconButton`), **Font Awesome Icon**, Supabase, React Query, Intersection Observer API

### 5.6 แดชบอร์ดผู้ดูแลระบบ (Admin Dashboard - `pages/admin/dashboard/*`)

- **โครงสร้างหน้าแดชบอร์ด (Admin Layout Component):**
  - **Sidebar นำทาง (Chakra UI `VStack` หรือ custom Sidebar):** โลโก้, ลิงก์ (ภาพรวม, จัดการผู้ใช้, หมวดหมู่, ประกาศ, เว็บไซต์, เวอร์ชัน)
  - **พื้นที่แสดงเนื้อหาหลัก**
  - **การป้องกัน Route**

### 5.6.1 หน้าภาพรวมแดชบอร์ด (Dashboard Overview Page)

- **หัวเรื่อง, Data Fetching, ความสามารถ:** แสดงข้อมูลสรุปใน **Chakra UI `Card`** หรือกราฟ (recharts/nivo)

### 5.6.2 หน้าจัดการผู้ใช้ (User Management Page)

- **หน้า `/admin/dashboard/users/index.tsx` (List Users):**
  - **หัวเรื่อง, Data Fetching, ส่วนควบคุม:** ช่องค้นหา (**Chakra UI `Input`**), ฟิลเตอร์ (**Chakra UI `Select`**)
  - **ตารางผู้ใช้ (**Chakra UI `Table`**):** คอลัมน์ (#, Avatar, Email, ชื่อ, สิทธิ์ (**Chakra UI `Badge`**), วันที่สมัคร, วันหมดอายุ, สถานะ, ปุ่ม "จัดการ" (ไอคอน **Font Awesome** `faCog` หรือ `faEdit`))
  - Sortable, Pagination (Chakra UI)
- **หน้า `/admin/dashboard/users/[user_id].tsx` (Edit User Detail Page):**
  - **Data Fetching, หัวเรื่อง, UI (Form React Hook Form + Zod, Chakra UI):** Avatar, Email, ชื่อที่แสดง, สิทธิ์, วันที่สมัคร, วันหมดอายุ (**Chakra UI `Input type="date"`** หรือ custom DatePicker), ระงับบัญชี (**Chakra UI `Switch`**)
  - **ปุ่มดำเนินการ (**Chakra UI `Button`**):** "บันทึก", "ยกเลิก"
- **การพัฒนา:** Next.js Pages, **Chakra UI** (`Input`, `Table`, Pagination, `Select`, `DatePicker` (or Input type date), `Switch`, `Badge`), React Hook Form + Zod, Supabase, React Query

### 5.6.3 หน้าจัดการหมวดหมู่นิยาย (Category Management Page)

- **หัวเรื่อง, Data Fetching, ส่วนเพิ่ม/แก้ไขหมวดหมู่ (**Chakra UI `Modal`** + Form):**
  - **Dialog:** ชื่อ, Slug, คำอธิบาย. ปุ่ม "บันทึก", "ยกเลิก"
- **รายการหมวดหมู่ (**Chakra UI `Table`** หรือ `List`):**
  - แสดง: ชื่อ, Slug, จำนวนนิยาย. ปุ่ม "แก้ไข", ปุ่ม "ลบ" (**Chakra UI Button** colorScheme="red", **Chakra UI `AlertDialog`**, ตัวเลือกจัดการนิยาย)
- **การพัฒนา:** Next.js Page, **Chakra UI** (`Modal`, `Table`, `Button`, `Select`, `AlertDialog`), React Hook Form + Zod, Supabase, React Query

### 5.6.4 หน้าจัดการประกาศ (Announcement Management Page)

- **หัวเรื่อง, Data Fetching, ความสามารถ:**
  - ปุ่ม "สร้างประกาศใหม่" (เปิด **Chakra UI `Modal`**)
  - **ตารางประกาศ (**Chakra UI `Table`**):** หัวข้อ, สถานะ (**Chakra UI `Badge`**), หน้าที่แสดง, กลุ่มผู้รับ, วันที่, ปุ่มดำเนินการ ("แก้ไข", "ลบ", "สลับสถานะ")
- **Dialog สร้าง/แก้ไขประกาศ (**Chakra UI `Modal`** + Form):**
  - ฟิลด์: หัวข้อ, เนื้อหา (Markdown - TipTap/react-markdown), วันที่เริ่ม/สิ้นสุด (**Chakra UI `Input type="date"`**), หน้าที่แสดง, กลุ่มผู้รับ, สถานะ "ใช้งาน" (**Chakra UI `Switch`**)
  - ปุ่ม: "บันทึก", "ยกเลิก"
- **การพัฒนา:** Next.js Page, **Chakra UI** (`Modal`, `Table`, `Button`, `Select`, `Input type="date"`, `Switch`, `Badge`), React Hook Form + Zod, TipTap/`react-markdown`, Supabase, React Query

### 5.6.5 หน้าจัดการเว็บไซต์ (Site Management Page)

- **หัวเรื่อง, Data Fetching, Layout (**Chakra UI `Card`** sections), UI (Form React Hook Form + Zod, Chakra UI):**
  - **ทั่วไป:** ชื่อเว็บ, คำอธิบาย, โลโก้, Favicon
  - **ลงทะเบียน/เข้าสู่ระบบ:** เปิด/ปิด (**Chakra UI `Switch`**)
  - **เนื้อหา:** จำนวนรายการต่อหน้า, ค่าเริ่มต้น
- **ปุ่มดำเนินการ (**Chakra UI `Button`**):** "บันทึกการตั้งค่า"
- **การพัฒนา:** Next.js Page, **Chakra UI** (`Card`, `Input`, `Textarea`, `Switch`, `Input type="file"`, `Button`), React Hook Form + Zod, Supabase, React Query

### 5.6.6 หน้าจัดการเวอร์ชัน (Version Management Page)

- **หัวเรื่อง, Layout (**Chakra UI `Tabs`**):**
- **Tab 1: เวอร์ชันเว็บไซต์ / Changelog:**
  - **Data Fetching, แสดง Changelog (**Chakra UI `Table`**), ปุ่ม "เพิ่ม Changelog ใหม่" (เปิด **Chakra UI `Modal`**), Dialog เพิ่ม/แก้ไข (Form, **Chakra UI `Switch`**), ปุ่มดำเนินการ**
- **Tab 2: เวอร์ชันนิยายและตอน:**
  - **Data Fetching, ส่วนค้นหานิยาย (**Chakra UI `Input`** หรือ `Select`**), เมื่อเลือกนิยาย:**
    - **ประวัติข้อมูลเมตานิยาย (**Chakra UI `Table`**):** ปุ่ม "ดูรายละเอียด" (**Chakra UI `Modal`**), "กู้คืน" (**Chakra UI `AlertDialog`**)
    - **ประวัติเวอร์ชันตอน:** **Chakra UI `Select`** เลือกตอน, **Chakra UI `Table`**: ปุ่ม "ดูเนื้อหา" (**Chakra UI `Modal`**), "กู้คืน" (**Chakra UI `AlertDialog`**)
- **การพัฒนา:** Next.js Page, **Chakra UI** (`Tabs`, `Table`, `Modal`, `Input`, `Input type="date"`, `Switch`, `Select`, `AlertDialog`), React Hook Form + Zod, `react-markdown`, Supabase, React Query

### 5.7 Modal สร้างนิยาย (Create Novel Modal)
- (รายละเอียดตามข้อ 5.1.1, ใช้ **Chakra UI `Modal`**, React Hook Form + Zod, TipTap, react-image-crop)

### 5.8 การจัดการตอน (Chapter Management)
- **5.8.1 Modal เพิ่มตอนด้วย .txt (Batch Upload Modal):** (รายละเอียดตามข้อ 5.2.2, ใช้ **Chakra UI `Modal`**)
- **5.8.2 Modal เพิ่ม/แก้ไขตอน (Create/Edit Chapter Modal):** (รายละเอียดตามข้อ 5.2.3, ใช้ **Chakra UI `Modal`**, React Hook Form + Zod, TipTap)

### 5.9 หน้าโปรไฟล์ (Profile Page - `pages/profile.tsx`)

- **การนำทาง, หัวเรื่อง, Data Fetching, Layout (**Chakra UI `Card`**):**
- **ความสามารถ (ใช้ Chakra UI Components, React Hook Form + Zod):**
  - **รูปโปรไฟล์ (**Chakra UI `Avatar`**):** แสดง, ปุ่ม "เปลี่ยนรูป" (File Picker, react-image-crop, API)
  - **ชื่อที่แสดง:** แสดง, **Chakra UI `Input`** + ปุ่ม "บันทึก"
  - **สิทธิ์ผู้ใช้งาน:** แสดง (**Chakra UI `Badge`**)
  - **อีเมล:** แสดง
  - **เปลี่ยนรหัสผ่าน:** ปุ่ม "เปลี่ยนรหัสผ่าน" -> **Chakra UI `Modal`** (Form: ปัจจุบัน, ใหม่, ยืนยัน), ปุ่ม "บันทึก", "ยกเลิก"
  - **ออกจากระบบ:** ปุ่ม "ออกจากระบบ" (**Chakra UI `Button`**) -> **Chakra UI `AlertDialog`**
- **การพัฒนา:** Next.js Page, **Chakra UI** (`Card`, `Avatar`, `Input`, `Button`, `Modal`, `AlertDialog`, `Badge`), React Hook Form + Zod, Supabase, `react-image-crop`

## 6. Key User Experience Flows & Enhancements

- **การล็อกอิน/ลงทะเบียน:** ราบรื่นผ่าน **Chakra UI `Modal`**, Feedback ชัดเจน (**Chakra UI `useToast`**)
- **การนำทาง (Navigation):** Navbar (Chakra UI components) และ Admin Sidebar ชัดเจน, URL เป็นมิตร
- **ประสบการณ์การอ่าน (Reading Experience):** ปรับแต่ง, โหมดอ่านต่อเนื่อง, บุ๊คมาร์ค, สลับตอนราบรื่น
- **เครื่องมือช่วยแปล (Translator Tools):**
  - "โหมดนักแปล" เปิด/ปิดง่าย
  - การมาร์คบรรทัด (คลิก checkbox, ดับเบิลคลิก) **Real-time และตอบสนองทันที (ใช้ Supabase Realtime, `useMutation` จาก React Query, Chakra UI `useToast` feedback)**
  - หน้าจัดการมาร์คมีประสิทธิภาพ
- **การตอบสนองของระบบ (System Responsiveness):**
  - **Empty States:** แสดงผลเหมาะสม
  - **Loading States:** **Chakra UI `Spinner`** หรือ **`Skeleton`**
  - **Error Handling:** Toast หรือข้อความใน UI
- **การออกแบบที่สวยงามและตอบสนองทุกอุปกรณ์:** ตามปรัชญา (ใช้ **Chakra UI**)
- **ปุ่ม Arrow Up:** สำหรับเลื่อนกลับไปบนสุด
- **การแจ้งเตือนการกระทำ (Action Confirmations):** **Chakra UI `useToast`**
- **การสร้าง `mark_lines` อัตโนมัติ:** เมื่อสร้างตอนใหม่, ระบบจะสร้าง record ว่างๆ ใน `mark_lines`
- **การคงสถานะหน้าเมื่อรีเฟรช:** (Next.js Routing, Data Fetching, `localStorage` หรือ Global State)

## 7. Database Structure (Supabase - PostgreSQL)

(โครงสร้างฐานข้อมูลอ้างอิงตามข้อมูลที่ให้มา)

- **`roles`** (id SERIAL PRIMARY KEY, name TEXT UNIQUE NOT NULL)
- **`profiles`** (
  id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
  display_name TEXT,
  avatar_url TEXT,
  role_id INTEGER NOT NULL REFERENCES roles(id) DEFAULT (SELECT id FROM roles WHERE name='Member'),
  role_expiry_date TIMESTAMP WITH TIME ZONE NULL,
  is_suspended BOOLEAN DEFAULT FALSE,
  **user_settings JSONB DEFAULT '{}'::jsonb,** -- For theme, reading preferences etc.
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
  )
- **`categories`** (id SERIAL PRIMARY KEY, name TEXT NOT NULL UNIQUE, slug TEXT UNIQUE NOT NULL, description TEXT NULL, ...)
- **`novels`** (id UUID PRIMARY KEY, title TEXT NOT NULL, slug TEXT UNIQUE NOT NULL, synopsis TEXT, cover_image_url TEXT, category_id INTEGER, original_language TEXT NOT NULL, status TEXT, visibility JSONB, default_chapter_status TEXT, default_chapter_access JSONB, created_by UUID, total_chapters INTEGER, last_chapter_updated_at TIMESTAMP WITH TIME ZONE, ...)
- **`chapters`** (id UUID PRIMARY KEY, novel_id UUID, chapter_number TEXT NOT NULL, title TEXT NOT NULL, content_original TEXT, status TEXT, access_level JSONB, created_by UUID, word_count INTEGER, ..., UNIQUE(novel_id, chapter_number))
- **`user_chapter_reads`** (user_id UUID, chapter_id UUID, read_at TIMESTAMP WITH TIME ZONE, PRIMARY KEY (user_id, chapter_id))
- **`user_bookmarks`** (id SERIAL PRIMARY KEY, user_id UUID, chapter_id UUID, novel_id UUID, bookmarked_at TIMESTAMP WITH TIME ZONE, UNIQUE(user_id, chapter_id))
- **`mark_lines`** (
  id UUID PRIMARY KEY, user_id UUID, chapter_id UUID,
  user_email TEXT, user_display_name TEXT, novel_id UUID, novel_title TEXT, chapter_number TEXT, chapter_title TEXT,
  marked_line_numbers JSONB NOT NULL DEFAULT '[]'::jsonb,
  ..., UNIQUE(user_id, chapter_id)
  )
  *   **หมายเหตุ `mark_lines`:** การ denormalize ข้อมูลต้องมีกลไก (Database Triggers หรือ Application-level logic) ในการอัปเดตเมื่อข้อมูลต้นทางเปลี่ยน
- **`announcements`** (id SERIAL PRIMARY KEY, title TEXT, content TEXT, start_date TIMESTAMP WITH TIME ZONE, end_date TIMESTAMP WITH TIME ZONE, target_roles JSONB, target_pages JSONB, is_active BOOLEAN, ...)
- **`site_settings`** (key TEXT PRIMARY KEY, value JSONB, description TEXT NULL, ...)
- **`site_changelogs`** (id SERIAL PRIMARY KEY, version_number TEXT, release_date DATE, summary TEXT, is_published BOOLEAN, ...)
- **`novel_versions`** (id SERIAL PRIMARY KEY, novel_id UUID, version_number INTEGER, metadata_snapshot JSONB, ..., UNIQUE(novel_id, version_number))
- **`chapter_versions`** (id SERIAL PRIMARY KEY, chapter_id UUID, version_number INTEGER, content_original_snapshot TEXT, ..., UNIQUE(chapter_id, version_number))

- **การรักษาความปลอดภัย (Row-Level Security - RLS):**
  - RLS จะถูกนำมาใช้อย่างเข้มงวดใน **ทุกตาราง**
  - Policies จะอ้างอิง `auth.uid()`, `auth.role()`, และข้อมูลจาก `profiles`
  - เมื่อแสดงผลข้อมูล `created_by` (UUID) ใน UI จะต้อง JOIN กับ `profiles` table เพื่อดึง `display_name` มาแสดง

## 8. Supabase Storage Buckets

- **`novel-covers`:** (Public: true)
- **`chapter-text-files`:** (Public: false, restricted access)
- **`avatars`:** (Public: true)
- **`site_assets`:** (Public: true)

## 9. สรุป Tech Stack (Tech Stack Summary)

- **Frontend Framework:** **Next.js (React)**
- **Styling & UI Component Library:** **Chakra UI**
- **Form Handling & Validation:** **React Hook Form** และ **Zod**
- **Icon Library:** **Font Awesome Icon** (ใช้ผ่าน `@fortawesome/react-fontawesome`)
- **State Management (Global State):** **Zustand** (หรือ React Context API)
- **Data Fetching & Server State Management:** **React Query (TanStack Query)**
- **Backend & Database Platform:** **Supabase**
  - **Database:** PostgreSQL
  - **Authentication:** Supabase Auth
  - **File Storage:** Supabase Storage
  - **Realtime:** Supabase Realtime
  - **Serverless Functions:** Supabase Edge Functions (หรือ Next.js API Routes)
- **ภาษา:** **TypeScript**
- **Rich Text Editor:** **TipTap**
- **Image Cropping:** **`react-image-crop`**
- **ZIP file generation (Client-side):** **JSZip**

## 10. การจัดการข้อผิดพลาดเบื้องต้น (Basic Error Handling)

- **Client-Side Validation:** Zod + React Hook Form
- **Server-Side Validation:** Zod ใน API Routes/Edge Functions
- **User-Facing Error Messages:** **Chakra UI `useToast`** หรือข้อความใน UI, React Error Boundaries
- **Code-Level Error Handling:** `try...catch`, Supabase error object, React Query/SWR error state
- **Error Logging:** `console.*`, Supabase Logs, Sentry/Logtail (Production)

## 11. มาตรการความปลอดภัยเบื้องต้น (Basic Security Measures)

- **Authentication:** Supabase Auth, Email Verification, พิจารณา MFA (Admin)
- **Authorization (RLS):** **หัวใจสำคัญ, ใช้กับทุกตาราง**
- **Input Validation:** Zod (Client & Server), Sanitize HTML (DOMPurify)
- **HTTPS:** เสมอ
- **Environment Variables:** จัดการ `SUPABASE_SERVICE_ROLE_KEY` อย่างปลอดภัย (Server-side เท่านั้น)
- **CSRF Protection:** (Next.js API Routes)
- **Security HTTP Headers:** (next.config.js)
- **Data Backups:** Supabase auto-backups, Manual backups
- **Principle of Least Privilege**
- **Software Updates:** อัปเดต dependencies (`npm audit` / `yarn audit`)
