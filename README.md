# INKREALM.CO: ข้อกำหนดระบบและคุณสมบัติหลัก (V5.1) - ฉบับปรับปรุง README (Next.js, Supabase, Chakra UI, Font Awesome)

เอกสารนี้เป็นข้อกำหนดระบบและคุณสมบัติหลักสำหรับแพลตฟอร์ม INKREALM.CO (V5.1) โดยเน้นฟังก์ชันที่จำเป็นตามการปรับปรุงล่าสุด โดยอ้างอิง Tech Stack ใหม่ (Next.js (React), Supabase, **Chakra UI**, **Font Awesome Icons**) และออกแบบมาเพื่อความสะดวกในการปรับใช้, ทดสอบ, และย้ายไปยังโดเมนต่างๆ

---

**ลำดับความสำคัญในการพิจารณาและพัฒนา (โดยรวม):**

1.  **ความปลอดภัยของระบบและข้อมูล (Security & Data Integrity):** การยืนยันตัวตน, การกำหนดสิทธิ์ (RLS), การป้องกันข้อมูลรั่วไหล, การตรวจสอบข้อมูลนำเข้า, การสำรองข้อมูล
2.  **โครงสร้างฐานข้อมูล (Database Structure):** ความถูกต้อง, ความครบถ้วน, ความสัมพันธ์ของข้อมูล, การรองรับการขยายตัว และการ query ที่มีประสิทธิภาพ (รวมถึงการ denormalize ที่จำเป็นอย่าง `mark_lines`)
3.  **ระบบหลัก (Core Systems):** ระบบยืนยันตัวตน, ระบบจัดการเนื้อหานิยาย (CMS), ระบบการอ่านและบุ๊คมาร์ค, ระบบเครื่องมือสำหรับนักแปล
4.  **ประสบการณ์ผู้ใช้ (User Experience - UX):** การใช้งานง่าย, ความต่อเนื่อง, การตอบสนองของระบบ, การจัดการข้อผิดพลาดที่เป็นมิตร
5.  **การออกแบบส่วนติดต่อผู้ใช้ (User Interface - UI):** ความสวยงาม, การอ่านง่าย, ความสอดคล้อง, Responsive Design (ใช้ **Chakra UI**)
6.  **คุณสมบัติเสริมและส่วนอำนวยความสะดวก (Enhancements & Utilities):** ระบบประกาศ, แดชบอร์ด, การจัดการเว็บไซต์, การจัดการเวอร์ชัน

---

## 1. ปรัชญาการออกแบบโดยรวม

- **การแสดงผลหรูหราและอ่านง่าย:**
  - **โทนสีหลัก:** สีทอง (เช่น `#FFD700`, `#B8860B` หรือโทนใกล้เคียงที่ให้ความรู้สึกหรูหรา – กำหนดใน `theme` ของ Chakra UI)
  - **สีรอง:** สีเทา (หลายเฉด เช่น `#F0F0F0`, `#CCCCCC`, `#808080`, `#333333` – กำหนดใน `theme` ของ Chakra UI)
  - **โหมดสว่าง (Light Mode):** เน้นสีทอง, เทาอ่อน, และขาว เพื่อความสบายตา ดูสะอาด และหรูหรา
  - **โหมดมืด (Dark Mode):** เน้นสีทอง (อาจปรับให้สว่างขึ้นเล็กน้อยเพื่อคอนทราสต์), เทาเข้ม, และดำ เพื่อลดแสงสะท้อนและช่วยให้อ่านง่ายในที่มืด (ใช้ `useColorMode` hook ของ Chakra UI)
  - **โลโก้:** พิจารณาใช้การไล่ระดับสี (gradient) ที่ผสมผสานสีทองและสีเทาอย่างลงตัว หรือสีทองล้วนบนพื้นหลังที่เหมาะสม เพื่อความโดดเด่นและทันสมัย
  - **ฟอนต์หลัก:** Sarabun สำหรับการแสดงผลภาษาไทยที่สวยงามและอ่านง่าย; พิจารณาฟอนต์สำรองสำหรับภาษาอังกฤษที่เข้ากัน (เช่น Noto Sans, Open Sans) – กำหนดใน `theme` ของ Chakra UI
- **เครื่องมือสำหรับนักแปล:** มีฟังก์ชันสนับสนุนการทำงานของนักแปลโดยเฉพาะ เน้นความชัดเจนและประสิทธิภาพ
- **ใช้งานง่าย ตอบโจทย์ทุกอุปกรณ์:**
  - UI ใช้งานง่าย (Intuitive) และสอดคล้องกันทั้งระบบ
  - Responsive Design เพื่อประสบการณ์ที่ดีในทุกขนาดหน้าจอ (ใช้ Style Props และ Layout Components ของ Chakra UI เช่น `Box`, `Flex`, `Grid`)
  - หน้าอ่านนิยายไม่มี Horizontal Scroll โดยเด็ดขาด
- **ความยืดหยุ่นและการทดสอบ:** โครงสร้างที่เอื้อต่อการเปลี่ยนแปลงโดเมนและการทดสอบระบบที่ง่ายดาย
- **การเปลี่ยนธีม:**
  - การสลับระหว่างโหมดสว่างและโหมดมืดจะต้องราบรื่น (ใช้ `useColorMode` hook ของ Chakra UI)
  - สีที่แสดงผลในแต่ละโหมดต้องไม่ผิดเพี้ยนจากที่ออกแบบไว้ และมีคอนทราสต์ที่เหมาะสมสำหรับการอ่าน
  - ควบคุมผ่านองค์ประกอบ UI ที่ชัดเจน (เช่น `IconButton` ของ Chakra UI พร้อมไอคอนพระอาทิตย์/พระจันทร์จาก **Font Awesome** ที่มีการเปลี่ยนแปลงสถานะชัดเจน) และสถานะธีมต้องถูกบันทึกไว้ในการตั้งค่าของผู้ใช้ (`localStorage` และ/หรือ `profiles.user_settings` ใน Supabase)

## 1.1 ภาพรวมระบบหลัก (Core System Overview)

- **ระบบยืนยันตัวตนและจัดการผู้ใช้:** ลงทะเบียน, เข้าสู่ระบบ (อีเมล/รหัสผ่าน, Google OAuth ผ่าน Supabase Auth), ลืมรหัสผ่าน, จัดการโปรไฟล์ (รวมถึงการเปลี่ยนรูปโปรไฟล์, ชื่อที่แสดง, รหัสผ่าน), การจัดการบทบาทและสิทธิ์ (Admin ควบคุมผ่าน UI และ RLS ใน Supabase)
  - **การสร้าง/ซิงค์โปรไฟล์:** เมื่อผู้ใช้ Login หรือ Signup ผ่าน Supabase Auth หากยังไม่มีข้อมูลในตาราง `profiles` สำหรับ `user_id` นั้น ระบบ (ผ่าน Next.js API Route หรือ Supabase Edge Function ที่ trigger โดย `onAuthStateChange`) จะสร้าง record ใหม่ใน `profiles` โดยอัตโนมัติ กำหนด `role_id` เริ่มต้นเป็น 'Member' (จาก `roles` table) ซึ่ง Admin สามารถแก้ไขได้ในภายหลัง
- **ระบบจัดการเนื้อหานิยาย (CMS):**
  - การสร้าง/แก้ไข/ลบนิยายและตอน (ดำเนินการผ่าน **Chakra UI Modal** เป็นหลัก เพื่อ UX ที่ต่อเนื่อง)
  - การอัปโหลดเนื้อหา: รูปปกนิยาย (พร้อม crop ด้วย `react-image-crop`), ไฟล์ .txt สำหรับตอนนิยาย (Batch Upload ผ่าน Next.js API Route)
  - การจัดการหมวดหมู่นิยาย (สำหรับ Admin)
  - ระบบจัดการเวอร์ชันสำหรับข้อมูลเมตานิยายและเนื้อหาตอน
- **ระบบการอ่านและบุ๊คมาร์ค:**
  - หน้าอ่านที่ปรับแต่งการแสดงผลได้ (ขนาดฟอนต์, font-family เบื้องต้น, ระยะห่างบรรทัด)
  - ระบบบุ๊คมาร์คตอนที่ผู้ใช้อ่านค้างไว้
  - โหมดอ่านต่อเนื่อง (Infinite Scroll ด้วย React Query หรือ SWR ร่วมกับ Intersection Observer API)
  - การสลับโหมดการอ่านสำหรับนักแปล (เปิด/ปิด "โหมดนักแปล" สำหรับนิยายนั้นๆ)
- **ระบบเครื่องมือสำหรับนักแปล:**
  - "โหมดนักแปล" (เมื่อเปิดใช้งานสำหรับนิยายที่กำหนด): แสดงเลขบรรทัด, checkbox สำหรับมาร์คบรรทัด
  - ระบบบรรทัดมาร์ค (Mark Lines): บันทึกบรรทัดที่นักแปลเลือก (**Real-time update** ไปยัง Supabase ผ่าน Subscription หรือ `useMutation` ทันที)
  - หน้าจัดการบรรทัดมาร์ค: ดูภาพรวม, คัดลอก, ดาวน์โหลดมาร์ค
- **ระบบแดชบอร์ด (สร้างด้วย Next.js Pages/Components และ Chakra UI):**
  - **สำหรับนักแปล:** ภาพรวมนิยายที่ตนเองจัดการ, ทางลัดไปหน้าจัดการบรรทัดมาร์ค
  - **สำหรับผู้ดูแลระบบ:** ภาพรวมระบบ, จัดการผู้ใช้, จัดการหมวดหมู่, จัดการประกาศ, จัดการเว็บไซต์ (การตั้งค่าทั่วไป), จัดการเวอร์ชัน (เว็บไซต์, นิยาย, ตอน)
- **ระบบฐานข้อมูลและไฟล์:** Supabase (PostgreSQL สำหรับฐานข้อมูล, Supabase Storage สำหรับไฟล์)
- **ระบบการแจ้งเตือนผู้ใช้ (User Feedback System):** การใช้ **Chakra UI Toast** สำหรับยืนยันการกระทำ, แจ้งเตือนข้อผิดพลาด, หรือให้ข้อมูลสั้นๆ
- **ระบบประกาศ (Announcement System):** สำหรับ Admin ในการสร้างและเผยแพร่ประกาศไปยังกลุ่มผู้ใช้เป้าหมายและหน้าที่กำหนด

## 1.2 โครงสร้าง URL ที่เป็นมิตรและการจัดการโดเมน (User-Friendly URL Structure & Domain Management)

(โครงสร้าง URL เหมือนเดิม)

- **โครงสร้าง URL หลัก (ตัวอย่างการจัดวางใน `pages` directory ของ Next.js):**
  - หน้ารวมนิยาย (หน้าแรก): `/` (ไฟล์ `pages/index.tsx`)
  - หน้าข้อมูลนิยาย: `/novels/[novel_id]` (ไฟล์ `pages/novels/[novel_id].tsx`, `novel_id` คือ UUID)
  - หน้าอ่านตอนนิยาย: `/novels/[novel_id]/[chapter_number]` (ไฟล์ `pages/novels/[novel_id]/[chapter_number].tsx`, `chapter_number` คือหมายเลขตอน (TEXT) อาจมี padding เช่น `001`, `123`)
  - หน้าโปรไฟล์ผู้ใช้ (สำหรับผู้ใช้ปัจจุบัน): `/profile` (ไฟล์ `pages/profile.tsx`)
  - หน้าโปรไฟล์ผู้ใช้ (สำหรับ Admin ดูผู้ใช้อื่น): `/admin/dashboard/users/[user_id]` (ไฟล์ `pages/admin/dashboard/users/[user_id].tsx`)
  - หน้าบุ๊คมาร์ค: `/bookmarks` (ไฟล์ `pages/bookmarks.tsx`)
  - แดชบอร์ดนักแปล: `/translator/dashboard` (ไฟล์ `pages/translator/dashboard.tsx`)
  - หน้าจัดการบรรทัดมาร์ค (นักแปล): `/translator/manage-marks/[novel_id]` (ไฟล์ `pages/translator/manage-marks/[novel_id].tsx`)
  - แดชบอร์ดผู้ดูแลระบบ: `/admin/dashboard` (ไฟล์ `pages/admin/dashboard/index.tsx` หรือ `_layout.tsx` สำหรับ layout)
  - หน้าย่อยในแดชบอร์ดผู้ดูแลระบบ: `/admin/dashboard/users` (ไฟล์ `pages/admin/dashboard/users/index.tsx`), `/admin/dashboard/categories` (ไฟล์ `pages/admin/dashboard/categories.tsx`), etc.
- **การสร้าง Slug สำหรับนิยาย (`novel_slug`):** (เหมือนเดิม)
- **สำหรับตอนนิยาย:** จะใช้ `chapter_number` (TEXT) โดยตรงใน URL
- **การจัดการโดเมนและ Base URL:** (เหมือนเดิม)

## 2. บทบาทผู้ใช้และสิทธิ์ (User Roles & Permissions)

(บทบาทและสิทธิ์เหมือนเดิม, ควบคุมโดย Supabase RLS และ logic ใน Next.js)

- **สมาชิก (Member):**
  - **แถบเครื่องมือสำหรับอ่าน (Reading Toolbar Component - Chakra UI `Flex`):** สารบัญ (เปิด **Chakra UI Modal** แสดงรายการตอน), ปรับแต่งการแสดงผล (เปิด **Chakra UI Popover/Menu**), บุ๊คมาร์คตอนปัจจุบัน (สลับสถานะ), สลับธีมเว็บ (Light/Dark), เปิด/ปิดโหมดอ่านต่อเนื่อง, นำทางตอนก่อนหน้า/ถัดไป (ใช้ **Font Awesome Icons** เช่น `faList`, `faCog`, `faBookmark`, `faSun/faMoon`, `faInfinity`, `faChevronLeft/faChevronRight`)
- **นักแปล (Translator):**
  - สิทธิ์ทั้งหมดของสมาชิก
  - สามารถเปิด/ปิด "โหมดนักแปล"
  - เข้าถึง "โหมดแปล": แสดงเลขบรรทัด, **Chakra UI Checkbox**, ดับเบิลคลิกที่บรรทัดเนื้อหาเพื่อมาร์คและคัดลอก
  - สร้าง/แก้ไข/ลบนิยายและตอนของตนเอง (ผ่าน **Chakra UI Modal**)
  - แถบเครื่องมือพิเศษในโหมดแปล: เพิ่มปุ่ม "คัดลอกมาร์คทั้งหมด" (ใช้ **Font Awesome Icon** เช่น `faCopy`)
- **ผู้ดูแลระบบ (Admin):**
  - สิทธิ์ทั้งหมดของนักแปล
  - จัดการผู้ใช้ทั้งหมด (ผ่านหน้า User Management `/admin/dashboard/users` และ `/admin/dashboard/users/[user_id]` หรือ **Chakra UI Modal**)

## 3. การเข้าสู่ระบบ / ลงทะเบียน / ลืมรหัสผ่าน (Authentication Modals & Pages)

- **การแสดงผล:**
  - แสดงผลผ่าน **Chakra UI Modal** เป็นหลัก
  - มีหน้าเว็บเฉพาะ (เช่น `pages/login.tsx`, `pages/register.tsx`, `pages/forgot-password.tsx`, `pages/reset-password.tsx`)
- **การพัฒนา:**
  - **Frontend (Next.js):**
    - สร้าง Forms ด้วย React Components และ **Chakra UI** (`Input`, `Button`, `FormControl`, `FormLabel`, `FormErrorMessage` etc.)
    - จัดการ Form State และ Validation ด้วย **React Hook Form** และ **Zod**
- **การแจ้งเตือน (User Feedback):** แสดง **Chakra UI Toast**

## 4. โครงสร้างแถบนำทาง (Navbar Structure)

- **ความสามารถ:** Responsive Design (ปรับการแสดงผลสำหรับ Mobile เช่น ใช้ **Chakra UI Menu** หรือ **Drawer** สำหรับเมนูแบบ Hamburger)
- **ตัวอย่างเมนู (สร้างเป็น React Component ใช้ Chakra UI `Flex`, `Link`, `Menu`, `Avatar`, `IconButton`):**
  - **Guest:** โลโก้, (ลิงก์), ปุ่มสลับธีม (**Font Awesome** `faSun/faMoon`), ปุ่ม "เข้าสู่ระบบ" (**Chakra UI Button**), ปุ่ม "ลงทะเบียน" (**Chakra UI Button**)
  - **Member:** โลโก้, ลิงก์, ปุ่มสลับธีม, โปรไฟล์ผู้ใช้ (**Chakra UI Menu** หรือ **Avatar + Popover**):
    - แสดงชื่อที่แสดง (Display Name) และบทบาท (เช่น "สมาชิก")
    - `<Link href="/profile">หน้าโปรไฟล์</Link>`
    - ปุ่ม "ออกจากระบบ"
  - **Translator:** เหมือน Member แต่เพิ่มเมนูแดชบอร์ดนักแปล
  - **Admin:** เหมือน Translator แต่เพิ่มเมนูแดชบอร์ดผู้ดูแลระบบ

## 5. รายละเอียดหน้าเว็บไซต์, Modals, และการจัดการเนื้อหา

### 5.1 หน้าแรก (Home Page - `pages/index.tsx`)

- **ส่วนแสดงประกาศ (Announcements Display Area - React Component):**
  - แสดงผลด้วย **Chakra UI Alert** หรือ **Card**, มีปุ่ม "X" (**Font Awesome** `faTimes`) ให้ปิด
- **ส่วนเครื่องมือหลัก (React Component):**
  - **ช่องค้นหานิยาย:** (**Chakra UI Input**)
  - **ฟิลเตอร์หมวดหมู่ (Category Filter):** (**Chakra UI Select**)
  - **ฟิลเตอร์สถานะนิยาย (Novel Status Filter):** (**Chakra UI Select**)
  - **ปุ่มสร้างนิยาย (Create Novel Button):** (สำหรับ Translator/Admin) (**Chakra UI Button**) ประกอบด้วย [**Font Awesome Icon** เช่น `faPlusSquare` หรือ `faFileMedical`] + ข้อความ "สร้างนิยาย". เมื่อคลิก เปิด Modal สร้างนิยาย (ดูข้อ 5.1.1)
- **การแสดงรายการนิยาย (React Component):**
  - รูปแบบ Grid Layout (ใช้ **Chakra UI Grid**)
  - **การ์ดนิยาย (**Chakra UI Card** หรือ **Box** ที่มี `Image`, `Heading`, `Text`):** แสดง: **รูปปก, ชื่อเรื่อง, จำนวนตอนทั้งหมด**
- **ระบบแบ่งหน้า (Pagination):** (ใช้ **Chakra UI Button** และ `IconButton` ประกอบกัน หรือ custom component)
- **การพัฒนา:**
  - **UI Components:** **Chakra UI** (`Input`, `Select`, `Button`, `Card`, `Grid`), **Font Awesome Icons**

### 5.1.1 Modal สร้างนิยาย (Create Novel Modal)

- **การเรียกใช้งาน:** คลิกปุ่ม "สร้างนิยาย" บนหน้าแรก
- **UI ภายใน Modal (**Chakra UI Modal** Component):**
  - **หัวเรื่อง Modal:** "สร้างนิยายใหม่"
  - **ฟอร์ม (จัดการด้วย React Hook Form, Validation ด้วย Zod):**
    - **ชื่อเรื่อง (Title):** (**Chakra UI Input**)
    - **คำโปรย (Synopsis):** **Rich Text Editor** (เช่น TipTap)
    - **หมวดหมู่ (Category):** (**Chakra UI Select**)
    - **รูปปกนิยาย (Cover Image):** `Input type="file"`, Crop (ใช้ `react-image-crop`)
    - **ภาษาต้นฉบับ (Original Language):** (**Chakra UI Select**) เลือก ("จีน (lang code: `zh`)", "อังกฤษ (lang code: `en`)", "ไทย (lang code: `th`)")
    - **Slug นิยาย (`novel_slug`):** (**Chakra UI Input**)
    - **สถานะนิยาย (Novel Status):** (**Chakra UI Select** หรือ **RadioGroup**) ("ต่อเนื่อง", "จบแล้ว", "ยุติการลง")
    - **สิทธิ์ในการมองเห็นนิยาย (Novel Visibility):** (**Chakra UI Select** (multiple) หรือ **CheckboxGroup**) ("ทุกคน", "เฉพาะสมาชิก", "เฉพาะนักแปล")
    - **การตั้งค่าเริ่มต้นสำหรับตอนใหม่ของนิยายนี้:**
      - **สถานะเริ่มต้นของตอน:** (**Chakra UI RadioGroup**) ("เผยแพร่", "ฉบับร่าง")
      - **สิทธิ์การเข้าถึงตอนเริ่มต้น:** (**Chakra UI Select** (multiple) หรือ **CheckboxGroup**)
  - **ปุ่มดำเนินการ (**Chakra UI Button**):** "เผยแพร่นิยาย", "บันทึกเป็นฉบับร่าง", "ยกเลิก"
- **การพัฒนา:**
  - **UI:** **Chakra UI Modal, Input, Select, RadioGroup, Checkbox, Button**
  - **API Route:** สร้าง `novels` record, อัปโหลดปก, สร้าง `novel_versions`

### 5.2 หน้าข้อมูลนิยายและสารบัญ (Novel Detail Page - `pages/novels/[novel_id].tsx`)

- **ส่วนแสดงประกาศ:** (เหมือนหน้าแรก)
- **ส่วนข้อมูลนิยาย:** รูปปก (`next/image`), ชื่อเรื่อง, หมวดหมู่, คำโปรย (Render HTML จาก Rich Text), จำนวนตอน, สถานะ, ภาษาต้นฉบับ (แสดง lang code เช่น `zh`, `en`, `th`), `novel_slug`
- **(สำหรับนักแปล/Admin) ปุ่มสลับ "โหมดนักแปล":** (**Chakra UI Switch**)
- **ปุ่มจัดการนิยาย:**
  - ปุ่ม "แก้ไขข้อมูลนิยาย" (**Chakra UI Button**): เปิด Modal แก้ไข (5.2.1)
  - ปุ่ม "ลบนิยาย" (**Chakra UI Button** colorScheme="red"): แสดง **Chakra UI AlertDialog**
- **ส่วนเพิ่มตอน:**
  - ปุ่ม: "เพิ่มตอนด้วย .txt" (เปิด Modal 5.2.2)
  - ปุ่ม: "เพิ่มตอน" (เปิด Modal 5.2.3)
- **ส่วนสารบัญ (Table of Contents Section - Chakra UI Accordion หรือ custom List):**
  - **รายการตอน:**
    - ไอคอนบุ๊คมาร์ค (**Font Awesome**), หมายเลขตอน (`chapter_number`), ชื่อตอน (Link)
    - สถานะการอ่าน (ไอคอน **Font Awesome** `faEye`)
    - **(สำหรับผู้สร้าง/Admin):** ไอคอนสถานะตอน, ปุ่ม "แก้ไขตอน" (**Font Awesome** `faPencilAlt`), ปุ่ม "ลบตอน" (**Font Awesome** `faTrashAlt`), ปุ่ม "คัดลอกมาร์คทั้งหมด" (**Font Awesome** `faCopy`)
- **การพัฒนา:** Chakra UI (Switch, Button, Accordion, Modal, AlertDialog), Font Awesome Icons

### 5.2.1 Modal แก้ไขข้อมูลนิยาย (Edit Novel Modal)

- **UI ภายใน Modal (**Chakra UI Modal**):** ฟอร์มเหมือน 5.1.1 แต่สำหรับแก้ไข
  - **ภาษาต้นฉบับ:** แสดงค่าปัจจุบัน (เช่น `zh`, `en`, `th`) และแก้ไขได้ผ่าน **Chakra UI Select**.
  - ปุ่ม: "บันทึกการเปลี่ยนแปลง", "ยกเลิก", "ลบนิยาย" (แสดง **Chakra UI AlertDialog**)
- **การพัฒนา:** สร้าง `novel_versions` เมื่อมีการเปลี่ยนแปลง

### 5.2.2 Modal เพิ่มตอนด้วย .txt (Batch Upload Modal)

- **UI ภายใน Modal (**Chakra UI Modal**):**
  - Drag & Drop หรือ ปุ่ม "เลือกไฟล์" (**Chakra UI Input type="file"**)
  - ตัวเลือกจัดการชื่อซ้ำ: (**Chakra UI RadioGroup**) "เขียนทับ" หรือ "ข้าม"
  - **Progress Bar (**Chakra UI Progress**):**
- **การทำงานเบื้องหลัง:** API Route ประมวลผลไฟล์ .txt, สร้าง `chapters`, `mark_lines` (ว่าง), `chapter_versions`

### 5.2.3 Modal เพิ่ม/แก้ไขตอน (Create/Edit Chapter Modal)

- **UI ภายใน Modal (**Chakra UI Modal**):**
  - ฟอร์ม (React Hook Form + Zod):
    - **ชื่อตอน (Chapter Title):** (**Chakra UI Input**)
    - **หมายเลขตอน (`chapter_number` - TEXT):** (**Chakra UI Input**) (Validate ไม่ซ้ำในนิยาย, รูปแบบตัวเลข+padding)
    - **เนื้อหา (`content_original`):** **Rich Text Editor** (TipTap)
    - **สถานะตอน:** (**Chakra UI RadioGroup**) ("เผยแพร่", "ฉบับร่าง")
    - **สิทธิ์การเข้าถึงตอน:** (**Chakra UI Select** (multiple) หรือ **CheckboxGroup**)
- **การทำงานเบื้องหลัง:** API Route สร้าง/อัปเดต `chapters`, `mark_lines` (ถ้าสร้างใหม่), `chapter_versions`

### 5.3 หน้าบุ๊คมาร์ค (Bookmarks Page - `pages/bookmarks.tsx`)

- **สำหรับแต่ละนิยายในรายการ (**Chakra UI Card** หรือ **ListItem**):**
  - ปุ่ม "ไปยังบุ๊คมาร์คล่าสุด" (**Chakra UI Button** หรือ `Link`)
  - ปุ่ม "จัดการบุ๊คมาร์คสำหรับเรื่องนี้" (**Chakra UI Button**): เปิด **Chakra UI Modal**
- **Modal "จัดการบุ๊คมาร์คสำหรับเรื่อง: [ชื่อนิยาย]" (**Chakra UI Modal**):**
  - ตัวเลือกเรียง: (**Chakra UI Select**)
  - รายการบุ๊คมาร์ค (List พร้อม **Chakra UI Checkbox**)
  - ปุ่ม: "ลบรายการที่เลือก", "ล้างบุ๊คมาร์คทั้งหมดของเรื่องนี้" (แสดง **Chakra UI AlertDialog**)
- **ปุ่ม "ลบบุ๊คมาร์คทั้งหมด (ทุกเรื่อง)":** (**Chakra UI Button** colorScheme="red"), แสดง **Chakra UI AlertDialog**
- **การพัฒนา:** Chakra UI (Card, List, Modal, AlertDialog, Select, Checkbox, Button), Font Awesome Icons

### 5.4 แดชบอร์ดนักแปล (Translator Dashboard - `pages/translator/dashboard.tsx`)

- **การแสดงผล: การ์ดนิยาย (**Chakra UI Card**):**
  - รูปปก, ชื่อนิยาย
  - ความคืบหน้าการมาร์ค + **Chakra UI Progress Bar**
  - ปุ่ม "จัดการบรรทัดมาร์ค" (**Chakra UI Button**), ปุ่ม "ไปยังหน้าข้อมูลนิยาย" (**Chakra UI Button**)
- **การพัฒนา:** Chakra UI (Card, Progress, Button)

### 5.4.1 หน้าจัดการบรรทัดมาร์ค (Mark Line Management Page - `pages/translator/manage-marks/[novel_id].tsx`)

- **ส่วนควบคุมหลัก:** ปุ่ม "เลือกตอนเพื่อดำเนินการหลายรายการ" (**Chakra UI Button** หรือ `Switch`)
- **การแสดงผลสารบัญตอน (**Chakra UI Accordion** หรือ List):**
  - หัวกลุ่มตอน: (ถ้า Batch Mode) **Chakra UI Checkbox** "เลือกทั้งหมดในกลุ่มนี้"
- **รายการตอน:**
  - (ถ้า Batch Mode) **Chakra UI Checkbox**
  - จำนวนบรรทัดที่มาร์ค, สถานะการมาร์ค (ไอคอน **Font Awesome** `faCheckCircle`/`faCircle`)
  - ปุ่ม "คัดลอกมาร์คตอนนี้" (ไอคอน **Font Awesome** `faCopy`), ปุ่ม "ไปที่หน้าอ่าน/แปลตอนนี้"
- **Batch Actions Toolbar (**Chakra UI `Flex` หรือ `Stack`, แสดงเมื่อ Batch Mode):**
  - ปุ่ม "คัดลอกบรรทัดมาร์คของตอนที่เลือก", ปุ่ม "ดาวน์โหลดไฟล์บรรทัดมาร์คของตอนที่เลือก" (เปิด **Chakra UI Modal**)
  - **Modal ดาวน์โหลด:** รูปแบบไฟล์ (**Chakra UI RadioGroup**) "รวม .txt" หรือ "แยก .zip"
- **การพัฒนา:** Chakra UI (Accordion, Checkbox, Button, Modal, RadioGroup, Switch), Font Awesome Icons, JSZip

### 5.5 หน้าอ่านนิยายและโหมดแปล (Reading & Translator Mode UI - `pages/novels/[novel_id]/[chapter_number].tsx`)

- **การแสดงผลเนื้อหา:**
  - ปุ่ม "Arrow Up" (**Font Awesome** `faArrowUp`)
- **โหมดการอ่าน:**
  - **แถบเครื่องมือหลักด้านบน (Top Reading Toolbar - Chakra UI `Flex`, sticky):**
    - ปุ่ม "สารบัญ" (**Font Awesome** `faList`): เปิด **Chakra UI Modal**
    - ปุ่ม "ปรับแต่ง (Aa)" (**Font Awesome** `faCog`): เปิด **Chakra UI Popover/Menu**
    - ปุ่ม "บุ๊คมาร์ค" (**Font Awesome** `faBookmark`)
    - ปุ่ม "สลับธีม" (**Font Awesome** `faSun/faMoon`)
    - ปุ่ม "อ่านต่อเนื่อง" (**Font Awesome** `faInfinity`)
    - ปุ่มนำทางตอน "<", ">"
- **โหมดนักแปล (Translator Mode):**
  - **แถบเครื่องมือ:** เพิ่มปุ่ม "คัดลอกมาร์คทั้งหมด" (**Font Awesome** `faCopy`)
  - **การแสดงผลเนื้อหา:** แถบเลขบรรทัด, **Chakra UI Checkbox** ทางขวา
  - **MarkLine Interaction:**
    - **คลิก Checkbox:** Real-time update `mark_lines.marked_line_numbers` (API + React Query `useMutation`), **Chakra UI Toast**
    - **ดับเบิลคลิกบรรทัด:** คัดลอก, มาร์ค (API update), **Chakra UI Toast**
  - **ท้ายบทโหมดนักแปล:** ปุ่ม "ล้างมาร์คทั้งหมด" (**Chakra UI Button** colorScheme="red", แสดง **Chakra UI AlertDialog**)
- **การพัฒนา:** Chakra UI (Modal, Popover, Button, Checkbox, Switch, AlertDialog), Font Awesome Icons

### 5.6 แดชบอร์ดผู้ดูแลระบบ (Admin Dashboard - `pages/admin/dashboard/*`)

- **โครงสร้าง (Admin Layout Component):**
  - **Sidebar (**Chakra UI `VStack` หรือ `Box`**):** ลิงก์ (ใช้ `<Link>`) ไปยังส่วนต่างๆ
- **5.6.1 หน้าภาพรวม:** (**Chakra UI Card**)
- **5.6.2 หน้าจัดการผู้ใช้:**
  - **List Users (`/users/index.tsx`):**
    - **Chakra UI Input** (ค้นหา), **Chakra UI Select** (ฟิลเตอร์บทบาท)
    - **User Table (**Chakra UI Table**):** Avatar, Email, ชื่อ, สิทธิ์ (**Chakra UI Badge**), วันที่, ปุ่ม "จัดการ" (**Font Awesome** `faCog` หรือ `faEdit`)
    - Pagination (Custom with **Chakra UI Button/IconButton**)
  - **Edit User (`/users/[user_id].tsx`):**
    - ฟอร์ม: Avatar, ชื่อ, สิทธิ์ (**Chakra UI Select**), วันหมดอายุ (**Chakra UI Input type="date"** หรือ `react-datepicker` styled), ระงับ (**Chakra UI Switch**)
    - ปุ่ม: "บันทึก" (**Chakra UI Button**)
- **5.6.3 หน้าจัดการหมวดหมู่นิยาย:**
  - ปุ่ม "เพิ่มหมวดหมู่" (เปิด **Chakra UI Modal**)
  - **Modal เพิ่ม/แก้ไข:** ชื่อ, Slug, คำอธิบาย
  - **รายการหมวดหมู่ (**Chakra UI Table** หรือ List):** ปุ่ม "แก้ไข", "ลบ" (แสดง **Chakra UI AlertDialog**, ตัวเลือกย้ายนิยาย)
- **5.6.4 หน้าจัดการประกาศ:**
  - ปุ่ม "สร้างประกาศใหม่" (เปิด **Chakra UI Modal**)
  - **ตารางประกาศ (**Chakra UI Table**):** สถานะ (**Chakra UI Badge**), ปุ่ม "แก้ไข", "ลบ", "สลับสถานะ"
  - **Modal สร้าง/แก้ไข:** หัวข้อ, เนื้อหา (Markdown), วันที่ (**Chakra UI Input type="date"**), หน้าที่แสดง (Multi-select/Checkbox Group), กลุ่มผู้รับ, สถานะ (**Chakra UI Switch**)
- **5.6.5 หน้าจัดการเว็บไซต์:**
  - UI (**Chakra UI Card** หรือ `FormControl`s): ชื่อเว็บ, โลโก้, Favicon, ตั้งค่าลงทะเบียน (**Chakra UI Switch**), ค่าเริ่มต้นนิยาย
  - ปุ่ม "บันทึก" (**Chakra UI Button**)
- **5.6.6 หน้าจัดการเวอร์ชัน:**
  - **Layout (**Chakra UI Tabs**):**
    - **Tab 1 (Changelog):** **Chakra UI Table**, ปุ่ม "เพิ่ม" (เปิด **Chakra UI Modal**)
    - **Tab 2 (Novel/Chapter Versions):** **Chakra UI Input/Select** (ค้นหานิยาย), **Chakra UI Table** (ประวัติ), ปุ่ม "ดู" (เปิด **Chakra UI Modal**), "กู้คืน" (แสดง **Chakra UI AlertDialog**)
- **การพัฒนา:** Chakra UI (Modal, Table, Button, Select, Badge, Switch, Input, Tabs, AlertDialog), Font Awesome Icons

### 5.7 Modal สร้างนิยาย (Create Novel Modal)
- (รายละเอียดตามข้อ 5.1.1 ทุกประการ, ใช้ **Chakra UI Modal**, React Hook Form + Zod, TipTap, `react-image-crop`)

### 5.8 การจัดการตอน (Chapter Management)
- **5.8.1 Modal เพิ่มตอนด้วย .txt:** (รายละเอียดตามข้อ 5.2.2, ใช้ **Chakra UI Modal**)
- **5.8.2 Modal เพิ่ม/แก้ไขตอน:** (รายละเอียดตามข้อ 5.2.3, ใช้ **Chakra UI Modal**, React Hook Form + Zod, TipTap)

### 5.9 หน้าโปรไฟล์ (Profile Page - `pages/profile.tsx`)

- UI (ใช้ **Chakra UI Card, Avatar, Input, Button, Modal, AlertDialog, Badge**):
  - รูปโปรไฟล์: แสดง, ปุ่มเปลี่ยน (Crop 1:1)
  - ชื่อที่แสดง: แสดง, แก้ไข + บันทึก
  - สิทธิ์: แสดง (**Chakra UI Badge**)
  - อีเมล: แสดง
  - เปลี่ยนรหัสผ่าน: ปุ่มเปิด **Chakra UI Modal** (ฟอร์ม: ปัจจุบัน, ใหม่, ยืนยัน)
  - ออกจากระบบ: ปุ่ม (**Chakra UI Button**) -> แสดง **Chakra UI AlertDialog**
- **การพัฒนา:** Font Awesome Icons สำหรับปุ่มต่างๆ หากต้องการ

## 6. Key User Experience Flows & Enhancements

- **การล็อกอิน/ลงทะเบียน:** ราบรื่นผ่าน **Chakra UI Modal**, Feedback ชัดเจน (**Chakra UI Toast**)
- **การนำทาง:** Navbar และ Admin Sidebar (ใช้ **Chakra UI Flex, VStack, Link, Menu**)
- **เครื่องมือช่วยแปล (Translator Tools):**
  - การมาร์คบรรทัด: **Real-time**, ตอบสนองทันที (React Query `useMutation` + Supabase Realtime + API Route), **Chakra UI Toast** feedback
- **การตอบสนองของระบบ:**
  - **Loading States:** **Chakra UI Spinner** หรือ **Chakra UI Skeleton**
  - **Error Handling:** **Chakra UI Toast** หรือข้อความใน UI
- **การแจ้งเตือนการกระทำ:** **Chakra UI Toast**
- **การสร้าง `mark_lines` อัตโนมัติ:** (Logic เดิม, สำคัญมาก)
- **การคงสถานะหน้าเมื่อรีเฟรช:** (Logic เดิม)

## 7. Database Structure (Supabase - PostgreSQL)

(โครงสร้างฐานข้อมูลตาม CSV ที่ให้มา และสอดคล้องกับเอกสารเดิม)

- **`roles`** (id SERIAL PRIMARY KEY, name TEXT UNIQUE NOT NULL)
- **`profiles`** (
  id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
  display_name TEXT,
  avatar_url TEXT,
  role_id INTEGER NOT NULL REFERENCES roles(id) DEFAULT (SELECT id FROM roles WHERE name='Member'), -- Default role
  role_expiry_date TIMESTAMP WITH TIME ZONE NULL,
  is_suspended BOOLEAN DEFAULT FALSE,
  **user_settings JSONB DEFAULT '{}'::jsonb,** -- For theme, reading prefs
  created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
  )
- **`categories`** (id SERIAL PK, name TEXT N U, slug TEXT U N, description TEXT NULL, ...)
- **`novels`** (id UUID PK, title TEXT N, slug TEXT U N, synopsis TEXT, cover_image_url TEXT, category_id INT FK, original_language TEXT N -- e.g., `zh`, `en`, `th`, status TEXT DEFAULT 'ongoing', visibility JSONB, default_chapter_status TEXT, default_chapter_access JSONB, created_by UUID N FK, total_chapters INT, last_chapter_updated_at TIMESTAMPTZ, ...)
- **`chapters`** (id UUID PK, novel_id UUID N FK, chapter_number TEXT N, title TEXT N, content_original TEXT, status TEXT, access_level JSONB, created_by UUID N FK, word_count INT, ..., UNIQUE(novel_id, chapter_number))
- **`user_chapter_reads`** (user_id UUID FK, chapter_id UUID FK, read_at TIMESTAMPTZ, PK(user_id, chapter_id))
- **`user_bookmarks`** (id SERIAL PK, user_id UUID N FK, chapter_id UUID N FK, novel_id UUID N FK, bookmarked_at TIMESTAMPTZ, UNIQUE(user_id, chapter_id))
- **`mark_lines`** (
  id UUID PK, user_id UUID N FK, chapter_id UUID N FK,
  user_email TEXT, user_display_name TEXT, novel_id UUID N FK, novel_title TEXT, chapter_number TEXT, chapter_title TEXT,
  marked_line_numbers JSONB N DEFAULT '[]'::jsonb, -- **สำคัญ: Real-time updates**
  ..., UNIQUE(user_id, chapter_id)
  )
  *   **หมายเหตุ `mark_lines`:** การ denormalize ข้อมูลต้องมีกลไกอัปเดต (Triggers หรือ App-level logic)
- **`announcements`** (id SERIAL PK, title TEXT N, content TEXT N, start_date TIMESTAMPTZ, end_date TIMESTAMPTZ, target_roles JSONB, target_pages JSONB, is_active BOOL, created_by UUID FK, ...)
- **`site_settings`** (key TEXT PK, value JSONB, description TEXT, ...)
- **`site_changelogs`** (id SERIAL PK, version_number TEXT N U, release_date DATE N, summary TEXT, is_published BOOL, created_by UUID FK, ...)
- **`novel_versions`** (id SERIAL PK, novel_id UUID N FK, version_number INT N, metadata_snapshot JSONB N, created_by UUID FK, notes TEXT, ..., UNIQUE(novel_id, version_number))
- **`chapter_versions`** (id SERIAL PK, chapter_id UUID N FK, version_number INT N, content_original_snapshot TEXT, created_by UUID FK, notes TEXT, ..., UNIQUE(chapter_id, version_number))

- **การรักษาความปลอดภัย (RLS):** ใช้ในทุกตาราง อ้างอิง `auth.uid()`, `auth.role()`, `profiles`.

## 8. Supabase Storage Buckets

(เหมือนเดิม)
- **`novel-covers`:** (Public: true)
- **`chapter-text-files`:** (Public: false, restricted)
- **`avatars`:** (Public: true)
- **`site_assets`:** (Public: true)

## 9. สรุป Tech Stack (Tech Stack Summary)

- **Frontend Framework:** **Next.js (React)**
- **Styling & UI Component Library:** **Chakra UI**
- **Form Handling & Validation:** **React Hook Form** และ **Zod**
- **Icon Library:** **Font Awesome (React components, e.g., `@fortawesome/react-fontawesome`, `@fortawesome/free-solid-svg-icons`)**
- **State Management (Global):** **Zustand** (หรือ React Context API)
- **Data Fetching & Server State Management:** **React Query (TanStack Query)**
- **Backend & Database Platform:** **Supabase**
  - **Database:** PostgreSQL
  - **Authentication:** Supabase Auth
  - **File Storage:** Supabase Storage
  - **Serverless Functions:** Supabase Edge Functions (หรือ Next.js API Routes)
- **ภาษา:** **TypeScript**
- **Rich Text Editor:** **TipTap**
- **Image Cropping:** **`react-image-crop`**
- **ZIP file generation (Client-side):** **JSZip**
- **Toast Notifications:** **Chakra UI Toast**

## 10. การจัดการข้อผิดพลาดเบื้องต้น (Basic Error Handling)

- **Client-Side Validation:** Zod + React Hook Form
- **Server-Side Validation:** Zod ใน API Routes/Edge Functions
- **User-Facing Error Messages:** **Chakra UI Toast** หรือข้อความใน UI (ใช้ Chakra UI Components)
- **Code-Level Error Handling:** `try...catch`, ตรวจสอบ `{ data, error }` จาก Supabase
- **Error Logging:** `console.*`, Supabase Logs, Sentry/Logtail (optional)

## 11. มาตรการความปลอดภัยเบื้องต้น (Basic Security Measures)

(มาตรการเหมือนเดิม, เน้น RLS, Input Validation, HTTPS, Environment Variables, CSRF, HTTP Headers, Backups, Principle of Least Privilege, Updates)
- **Sanitize HTML output:** DOMPurify ก่อน render HTML ที่มาจากผู้ใช้ (คำโปรย, เนื้อหานิยาย) ด้วย `dangerouslySetInnerHTML` (ถ้า Chakra UI component ไม่ได้ handle เอง).
