# INKREALM.CO - README ฉบับสมบูรณ์ (V4.16)

เอกสารนี้เป็นคู่มือฉบับสมบูรณ์สำหรับโปรเจกต์ INKREALM.CO แพลตฟอร์มนิยายออนไลน์ที่ออกแบบมาเพื่อมอบประสบการณ์การอ่านที่หรูหราและเครื่องมือที่ทรงพลังสำหรับนักแปลโดยเฉพาะ

## สารบัญ

1.  [ปรัชญาการออกแบบโดยรวม](#1-ปรัชญาการออกแบบโดยรวม)
2.  [Tech Stack (ชุดเทคโนโลยีที่ใช้)](#2-tech-stack-ชุดเทคโนโลยีที่ใช้)
    * [Frontend](#frontend)
    * [Backend](#backend)
    * [เครื่องมือและอื่นๆ (Tools & Others)](#เครื่องมือและอื่นๆ-tools--others)
3.  [โทนสีและสไตล์ (Color Palette & Style)](#3-โทนสีและสไตล์-color-palette--style)
4.  [โครงสร้าง Layout หลัก (Main Layout Structure)](#4-โครงสร้าง-layout-หลัก)
5.  [บทบาทผู้ใช้และสิทธิ์ (User Roles & Permissions)](#5-บทบาทผู้ใช้และสิทธิ์-user-roles--permissions)
6.  [หน้าเข้าสู่ระบบ / ลงทะเบียน / ลืมรหัสผ่าน](#6-หน้าเข้าสู่ระบบ--ลงทะเบียน--ลืมรหัสผ่าน)
7.  [โครงสร้างแถบนำทาง (Navbar - Dynamic ตาม Role & Responsive)](#7-โครงสร้างแถบนำทาง-navbar---dynamic-ตาม-role--responsive)
8.  [รายละเอียดหน้าเว็บไซต์หลัก (Responsive Design)](#8-รายละเอียดหน้าเว็บไซต์หลัก-responsive-design)
    * [8.1 หน้าแรก (Home Page)](#81-หน้าแรก-home-page)
    * [8.2 หน้าข้อมูลนิยายและสารบัญ (Novel Detail Page)](#82-หน้าข้อมูลนิยายและสารบัญ-novel-detail-page)
    * [8.3 หน้าบุ๊คมาร์ค (Bookmarks Page)](#83-หน้าบุ๊คมาร์ค-bookmarks-page)
    * [8.4 แดชบอร์ดนักแปล (Translator Dashboard)](#84-แดชบอร์ดนักแปล-translator-dashboard)
    * [8.5 หน้าอ่านนิยายและโหมดแปล (Reading & Translator Mode UI)](#85-หน้าอ่านนิยายและโหมดแปล-reading--translator-mode-ui)
    * [8.6 แดชบอร์ดผู้ดูแลระบบ (Admin Dashboard)](#86-แดชบอร์ดผู้ดูแลระบบ-admin-dashboard)
    * [8.7 Modal สร้างนิยาย (Create Novel Modal)](#87-modal-สร้างนิยาย-create-novel-modal)
    * [8.8 การจัดการตอน (Chapter Management - ผ่าน Modals)](#88-การจัดการตอน-chapter-management---ผ่าน-modals)
    * [8.9 หน้าโปรไฟล์ (Profile Page)](#89-หน้าโปรไฟล์-profile-page)
9.  [โครงสร้างโฟลเดอร์โปรเจกต์ (Project Folder Structure)](#9-โครงสร้างโฟลเดอร์โปรเจกต์-project-folder-structure)
10. [ประสบการณ์ผู้ใช้งาน (Leading UX & User Flows)](#10-ประสบการณ์ผู้ใช้งาน-leading-ux--user-flows)
    * [10.1 การเข้าสู่ระบบ และหน้าแรก (Login & Home Page Experience)](#101-การเข้าสู่ระบบ-และหน้าแรก-login--home-page-experience)
    * [10.2 การเข้าดูนิยายและสารบัญ (Novel Detail Page Flow)](#102-การเข้าดูนิยายและสารบัญ-novel-detail-page-flow)
    * [10.3 การใช้งานบุ๊คมาร์ค (Bookmark Functionality)](#103-การใช้งานบุ๊คมาร์ค-bookmark-functionality)
    * [10.4 การเปลี่ยน Avatar](#104-การเปลี่ยน-avatar)
    * [10.5 ขั้นตอนการแปลและการจัดการเส้นมาร์ค (Translator Flow)](#105-ขั้นตอนการแปลและการจัดการเส้นมาร์ค-translator-flow)
    * [10.6 (สำหรับผู้ดูแลระบบ/ผู้ใช้ที่มีสิทธิ์) การวางข้อมูลบรรทัดมาร์คเก่า](#106-สำหรับผู้ดูแลระบบผู้ใช้ที่มีสิทธิ์-การวางข้อมูลบรรทัดมาร์คเก่า)
11. [ระบบความปลอดภัย (Security)](#11-ระบบความปลอดภัย-security)
12. [การทดสอบและประกันคุณภาพ (Testing & QA)](#12-การทดสอบและประกันคุณภาพ-testing--qa)
13. [การปรับปรุงประสบการณ์ผู้ใช้ (UX Enhancements)](#13-การปรับปรุงประสบการณ์ผู้ใช้-ux-enhancements---responsive-focus)
14. [การจัดการข้อมูลและการสำรองข้อมูล (Data Management & Backup)](#14-การจัดการข้อมูลและการสำรองข้อมูล-data-management--backup)
15. [Backend: โครงสร้างฐานข้อมูล (Supabase - PostgreSQL)](#15-backend-โครงสร้างฐานข้อมูล-supabase---postgresql)
    * [SQL Schema สำหรับสร้างตาราง](#sql-schema-สำหรับสร้างตาราง)
    * [ตาราง `mark_lines` และ View `admin_markline_overview` เพื่อการจัดการข้อมูลมาร์คที่ง่ายขึ้น](#ตาราง-mark_lines-และ-view-admin_markline_overview-เพื่อการจัดการข้อมูลมาร์คที่ง่ายขึ้น)
    * [SQL สำหรับสร้าง View `admin_markline_overview`](#sql-สำหรับสร้าง-view-admin_markline_overview)
16. [Backend: Supabase Storage Buckets](#16-backend-supabase-storage-buckets)
17. [การนำไปใช้งานและการติดตามผล (Deployment & Monitoring)](#17-การนำไปใช้งานและการติดตามผล-deployment--monitoring)
18. [การตั้งค่าเริ่มต้นสำหรับการพัฒนา (Getting Started - Local Development)](#18-การตั้งค่าเริ่มต้นสำหรับการพัฒนา-getting-started---local-development)

---

## 1. ปรัชญาการออกแบบโดยรวม (Overall Design Philosophy)

* **หรูหราและสง่างาม (Elegant & Luxurious):** เน้นความหรูหรา สง่างาม ผ่านการใช้สีทองเป็นหลัก และสีเทาเพื่อเสริมความทันสมัย
* **ชัดเจนและอ่านง่าย (Clarity & Readability):** ให้ความสำคัญสูงสุดกับการอ่านเนื้อหานิยายอย่างสบายตา ตัวอักษรต้องชัดเจน คอนทราสต์เหมาะสมบนพื้นหลังสีเทาและทอง โดยใช้ฟอนต์ Sarabun เพื่อความเป็นเอกภาพและอ่านง่ายในภาษาไทย
* **เครื่องมือสำหรับนักแปลโดยเฉพาะ (Translator-Focused Tools):** มอบเครื่องมือที่ทรงพลังและใช้งานง่ายสำหรับนักแปลในการอ่าน ศึกษา และจัดเก็บบันทึกคำศัพท์หรือข้อความสำคัญจากเนื้อหาต้นฉบับ
* **ใช้งานง่ายและคำนึงถึงผู้ใช้เป็นศูนย์กลาง (Intuitive & User-Centric):** ออกแบบโดยคำนึงถึงผู้ใช้งานทุกกลุ่ม (สมาชิก, นักแปล, ผู้ดูแลระบบ) มอบประสบการณ์ที่ใช้งานง่ายและตรงตามความต้องการ
* **ตอบสนองทุกอุปกรณ์และปรับเปลี่ยนแบบไดนามิก (Fully Responsive & Dynamic):** ออกแบบให้เว็บไซต์ปรับเปลี่ยนการแสดงผลอัตโนมัติตามขนาดหน้าจอของทุกอุปกรณ์ (Desktop, Tablet, Mobile) ทั้งแนวตั้งและแนวนอน โดยเฉพาะหน้าอ่านนิยายที่จะต้องไม่มีการเลื่อนซ้าย-ขวา (No Horizontal Scroll)
* **ความเป็นเอกลักษณ์ของแบรนด์ที่สอดคล้องกัน (Consistent Branding):** การใช้สีทอง-เทา, ฟอนต์ Sarabun, ไอคอน, และองค์ประกอบ UI ต่างๆ ต้องมีความสอดคล้องกันทั่วทั้งแพลตฟอร์ม

---

## 2. Tech Stack (ชุดเทคโนโลยีที่ใช้)

ส่วนนี้จะแบ่งเทคโนโลยีที่ใช้ในโปรเจกต์ออกเป็น Frontend, Backend, และเครื่องมืออื่นๆ เพื่อความชัดเจน

### Frontend

* **Framework:** Next.js
* **Styling:** TailwindCSS
* **Component Library:** DaisyUI (ทำงานร่วมกับ TailwindCSS)
* **Language:** TypeScript
* **Form Handling & Validation:** React Hook Form + Zod
* **Rich Text Editor:** TipTap หรือ Editor.js (สำหรับ Modal เพิ่ม/แก้ไขตอน)

### Backend

* **Platform:** Supabase
    * **Database:** PostgreSQL
    * **Authentication:** Supabase Auth
    * **Storage:** Supabase Storage

### เครื่องมือและอื่นๆ (Tools & Others)

* **Deployment:** Vercel
* **CI/CD:** GitHub Actions
* **Monitoring:** Sentry, Grafana (หรือ Supabase Dashboard)

---

## 3. โทนสีและสไตล์ (Color Palette & Style)

* **สีหลัก (Primary Color):**
    * ทอง (Gold): Old Gold (`#CFB53B`), Satin Gold (`#C4A647`), Dark Goldenrod (`#B8860B`)
* **สีรอง (Secondary Color):**
    * เทา (Grey): Light Grey (`#F0F0F0`, `#E8E8E8`), Medium Grey (`#A9A9A9`, `#989898`), Dark Grey (`#4A4A4A`, `#333333`)
* **สีเน้น (Accent Color):** สีทองสว่าง, ขาว/ครีม, แดงอ่อน (`#E57373`) สำหรับแจ้งเตือน
* **สีกลาง (Neutral Colors):** White/Off-White (`#FFFFFF`, `#FAFAFA`)
* **การพิมพ์ (Typography):**
    * ฟอนต์หลัก (Font Family): `Sarabun` (สำหรับทั้งส่วนหัวเรื่องและเนื้อหาทั่วไป)
    * ส่วนหัวเรื่อง (Headings): `Sarabun` (ปรับน้ำหนักฟอนต์ เช่น Bold, Semi-Bold)
    * เนื้อหาทั่วไป (Body Text): `Sarabun` (น้ำหนักปกติ Regular หรือ Medium)
* **ไอคอน (Icons):** FontAwesome, Material Icons (Outline) - สีทองหรือเทาเข้ม
    * ไอคอนสถานะการอ่าน: ไอคอนรูปตาแบบมี Animation (เช่น ตาที่กะพริบเบาๆ หรือมีการเคลื่อนไหวเล็กน้อยเมื่อถูกมองเห็น)
    * ไอคอนสถานะตอน: เผยแพร่ (ไอคอนรูปลูกโลก - Globe icon), ฉบับร่าง (ไอคอนรูปเอกสาร - Document icon)
    * ไอคอนปรับแต่งการแสดงผล: ไอคอน "Aa" หรือไอคอนรูปเฟืองการตั้งค่าการแสดงผล
    * ไอคอนบุ๊คมาร์ค: ไอคอนรูปที่คั่นหนังสือ
* **ปุ่มสลับธีม (Theme Toggle - สว่าง/มืด):**
    * เว็บไซต์รองรับการสลับธีมระหว่างสว่าง (Light Mode) และมืด (Dark Mode) โดยใช้ DaisyUI Theme Controller (แบบ Toggle พร้อมไอคอนพระอาทิตย์/พระจันทร์)
    * การตั้งค่าธีมจะถูกจดจำสำหรับผู้ใช้แต่ละคน (เช่น ใช้ localStorage)
    * ผู้ใช้สามารถสลับธีมได้จากแถบนำทาง (Navbar) และ ภายในหน้าอ่านนิยาย
* **Breadcrumbs Styling:** ใช้ DaisyUI Breadcrumbs component จัดสไตล์ให้เข้ากับธีมสีทอง-เทา มีขนาดตัวอักษรที่เหมาะสมและไม่รบกวนเนื้อหาหลัก

---

## 4. โครงสร้าง Layout หลัก (Main Layout Structure)

* **แถบนำทาง (Navbar - Sticky Top):** ตอบสนองทุกอุปกรณ์, มีปุ่มสลับธีม (ใช้ DaisyUI Navbar component)
* **ส่วน Breadcrumbs:** แสดงอยู่ใต้ Navbar หรือบริเวณบนสุดของพื้นที่เนื้อหาหลักในแต่ละหน้า (ยกเว้นหน้าแรกสุด) เพื่อบอกตำแหน่งปัจจุบันของผู้ใช้
* **พื้นที่เนื้อหาหลัก (Main Content Area):** พื้นหลังเทาอ่อน (`#F0F0F0` ในโหมดสว่าง), ปรับความกว้างสูงสุด (Max-width)
* **ส่วนท้ายเว็บ (Footer):** ดีไซน์เรียบหรู, ตอบสนองทุกอุปกรณ์

---

## 5. บทบาทผู้ใช้และสิทธิ์ (User Roles & Permissions)

* **สมาชิก (Member):**
    * อ่านเนื้อหาสาธารณะและตอนที่เผยแพร่แบบปกติ
    * ไม่มีบรรทัดมาร์ค, ไม่มีสิทธิ์สร้างนิยาย หรือใช้โหมดแปล
    * สามารถคั่นหน้า (Bookmark) ตอนที่ชอบได้
    * แก้ไขโปรไฟล์ (ชื่อที่แสดง, Avatar, รหัสผ่าน)
    * **แถบเครื่องมือสำหรับอ่าน (Reading Toolbar):** ไอคอนสารบัญ, ไอคอนปรับแต่งการแสดงผล ("Aa"), ไอคอนบุ๊คมาร์ค, ปุ่มสลับธีมเว็บ, ปุ่มนำทาง < (ตอนก่อนหน้า), ปุ่มนำทาง > (ตอนถัดไป)
* **นักแปล (Translator):**
    * สิทธิ์ทั้งหมดของสมาชิก
    * เข้าถึง "โหมดแปล (Translator Mode)"
    * ใช้งานบรรทัดมาร์ค: คลิกเพื่อมาร์คบรรทัด, ดับเบิลคลิกเพื่อคัดลอกข้อความต้นฉบับของบรรทัดนั้นพร้อมไฮไลท์
    * สร้างนิยาย (ผ่าน Modal สร้างนิยาย), แก้ไขตอน (ผ่าน Modal แก้ไข/เพิ่มตอน)
    * **แถบเครื่องมือพิเศษในโหมดแปล (Translator Mode Toolbar):** ไอคอนสารบัญ (แสดงจำนวนมาร์คของแต่ละตอน), ไอคอนปรับแต่งการแสดงผล ("Aa"), ไอคอนบุ๊คมาร์ค, เครื่องมือสำหรับนักแปล (ไอคอน "คัดลอกมาร์คทั้งหมด", ไอคอน "ล้างมาร์คทั้งหมด" พร้อมยืนยัน), ปุ่มสลับธีมเว็บ, ปุ่มนำทาง <, >
* **ผู้ดูแลระบบ (Admin):**
    * สิทธิ์ทั้งหมดของนักแปล
    * มีสิทธิ์จัดการทุกอย่าง, รวมถึงการเลื่อนขั้น/ลดขั้น/ระงับ/ลบผู้ใช้
    * ดูบรรทัดมาร์คของนักแปลคนอื่น (ผ่าน View `admin_markline_overview`)
    * ย้อนเวอร์ชันบท/นิยาย
* **การจัดการสิทธิ์:** ใช้ Row-Level Security (RLS) ใน Supabase ตามบทบาท (Role)

---

## 6. หน้าเข้าสู่ระบบ / ลงทะเบียน / ลืมรหัสผ่าน

* **Stack:** Supabase Auth, React Hook Form, Zod, DaisyUI (สำหรับ UI components เช่น Input, Button, Card)
* **UI:** ธีมทอง-เทา, ฟอนต์ Sarabun, ตอบสนองทุกอุปกรณ์, ข้อความ UI ทั้งหมดเป็นภาษาไทย
* **การแสดงผล:** โดยปกติจะแสดงผ่าน Modal ที่ปรากฏขึ้นอัตโนมัติเมื่อเข้าสู่หน้าแรก (ดูหัวข้อ 10.1) หรือเมื่อผู้ใช้พยายามเข้าถึงเนื้อหาที่ต้องมีการยืนยันตัวตน นอกจากนี้ยังมีหน้าเฉพาะสำหรับกรณีที่ผู้ใช้คลิกลิงก์โดยตรง (เช่น จากอีเมลลืมรหัสผ่าน)

---

## 7. โครงสร้างแถบนำทาง (Navbar - Dynamic ตาม Role & Responsive)

* **Stack:** DaisyUI (Navbar, Dropdown, Avatar), TailwindCSS, Custom logic สำหรับ ThemeToggle
* **ดีไซน์:** พื้นหลังเทาเข้ม/ทองเข้ม, ตัวอักษร/ไอคอนสีทองอ่อน/ขาว (ฟอนต์ Sarabun)
* **การจัดเรียงเมนู (Desktop - ซ้ายไปขวา):**
    * [โลโก้ INKREALM]: ไปหน้าแรก
    * [หน้าแรก]
    * [บุ๊คมาร์ค] (สำหรับผู้ใช้ที่เข้าสู่ระบบ)
    * [+ สร้างนิยาย] (นักแปล, Admin - เปิด Modal สร้างนิยาย)
    * [แดชบอร์ดนักแปล] (นักแปล, Admin)
    * [แดชบอร์ดผู้ดูแลระบบ] (Admin เท่านั้น)
    * [สลับธีม] (ไอคอนพระจันทร์/พระอาทิตย์ - DaisyUI Theme Controller)
    * [โปรไฟล์ (Avatar Dropdown - DaisyUI Avatar & Dropdown)]: รูปโปรไฟล์, ชื่อผู้ใช้
        * ตัวเลือกใน Dropdown: [ตั้งค่าโปรไฟล์], [เปลี่ยนรหัสผ่าน], [ออกจากระบบ]
* **การแสดงผลตามบทบาท:**
    * **สมาชิก:** [โลโก้], [หน้าแรก], [บุ๊คมาร์ค], [สลับธีม], [โปรไฟล์]
    * **นักแปล:** [โลโก้], [หน้าแรก], [บุ๊คมาร์ค], [+ สร้างนิยาย], [แดชบอร์ดนักแปล], [สลับธีม], [โปรไฟล์]
    * **ผู้ดูแลระบบ:** [โลโก้], [หน้าแรก], [บุ๊คมาร์ค], [+ สร้างนิยาย], [แดชบอร์ดนักแปล], [แดชบอร์ดผู้ดูแลระบบ], [สลับธีม], [โปรไฟล์]
* **การตอบสนองบนมือถือ (Mobile Responsive):**
    * แสดง [โลโก้ INKREALM] ทางซ้าย, ไอคอน Hamburger Menu (สีทอง) ทางขวา
    * คลิก Hamburger Menu: เมนูเลื่อนออกจากด้านข้าง (พื้นหลังเทาเข้ม, รายการเมนูสีทอง/ขาว - DaisyUI Drawer หรือ Dropdown)

---

## 8. รายละเอียดหน้าเว็บไซต์หลัก (Responsive Design)

ส่วนนี้จะอธิบายรายละเอียด UI และ UX ของแต่ละหน้าหลักในเว็บไซต์ โดยเน้นการออกแบบที่ตอบสนองทุกอุปกรณ์

### 8.1 หน้าแรก (Home Page)

* **Stack:** DaisyUI (Card, Grid, Pagination, Modal), Supabase (Query novels)
* **Breadcrumbs:** ไม่จำเป็นต้องแสดง
* **หัวเรื่อง:** "นิยายทั้งหมด"
* **การทำงานเริ่มต้น:** เมื่อผู้ใช้เข้าสู่หน้าแรก (โดยเฉพาะหากยังไม่ได้เข้าสู่ระบบ หรือเป็นการเข้าชมครั้งแรก) Modal สำหรับเข้าสู่ระบบ/ลงทะเบียน (DaisyUI Modal) จะปรากฏขึ้นโดยอัตโนมัติ ผู้ใช้สามารถปิด Modal เพื่อเข้าชมเนื้อหาบนหน้าแรกได้ (หากเนื้อหานั้นไม่ต้องการการยืนยันตัวตน)
* **Layout:** ส่วน Hero (ถ้ามี), Grid Layout แสดงนิยาย (Desktop 3-4 คอลัมน์, Tablet 2-3, Mobile 1-2)
* **การแบ่งหน้า (Pagination):** มีระบบแบ่งหน้า (DaisyUI Pagination)
* **การ์ดนิยาย (Novel Card - DaisyUI Card):**
    * รูปปกนิยาย (Cover Image): อัตราส่วน 3:4, คลิกไปยังหน้า "ข้อมูลนิยายและสารบัญ" (หากผู้ใช้ยังไม่ได้เข้าสู่ระบบและเนื้อหาต้องการการยืนยันตัวตน Modal เข้าสู่ระบบอาจปรากฏขึ้นอีกครั้ง)
    * ชื่อเรื่อง (Title): สีเทาเข้ม (Sarabun)
    * จำนวนตอน (Chapter Count): สีเทากลาง (Sarabun) (นับเฉพาะตอน `status = 'published'`)
* **ตัวเลือกการกรอง/จัดเรียง:** กรองตามหมวดหมู่, เรียงตามอัปเดตล่าสุด (DaisyUI Dropdown หรือ Select)
* **ปุ่ม "[+ สร้างนิยาย]" (สำหรับนักแปล/Admin):** อาจเป็นปุ่มลอย (FAB - DaisyUI Button) หรือมีใน Navbar แล้ว

### 8.2 หน้าข้อมูลนิยายและสารบัญ (Novel Detail Page)

* **Stack:** DaisyUI (Card, Button, Collapse, Modal, Tabs - ถ้ามี, Breadcrumbs), Supabase
* **Path:** `/[novel_slug]` หรือ `/[novel_id]`
* **Breadcrumbs:** แสดง เช่น หน้าแรก > [ชื่อหมวดหมู่ ถ้ามี] > [ชื่อนิยาย]
* **Layout:**
    * **ส่วนข้อมูลนิยาย (Novel Information Section):**
        * รูปปกนิยาย, ชื่อเรื่อง (ตัวใหญ่, สีเทาเข้ม), ผู้แต่ง/ผู้สร้าง (จาก `novels.author_id JOIN profiles.display_name`), หมวดหมู่ (จาก `novels.category_id JOIN categories.name` - แสดงเป็น Tag สีทองอ่อน DaisyUI Badge), ภาษาต้นฉบับ (แสดงชื่อเต็ม), คำโปรย (แสดงเต็มหรือย่อพร้อม "อ่านเพิ่มเติม"), จำนวนตอนทั้งหมด, สถานะนิยาย ("ต่อเนื่อง", "จบแล้ว")
        * ปุ่ม "อ่านตอนแรก" / "อ่านต่อ" (ถ้ามีประวัติการอ่าน) (สีทองหลัก - DaisyUI Button) นำไปยังโหมดที่เหมาะสม (หากผู้ใช้ยังไม่ได้เข้าสู่ระบบ Modal เข้าสู่ระบบอาจปรากฏขึ้น)
        * (สำหรับผู้สร้างนิยาย/Admin) ปุ่มจัดการนิยาย:
            * ปุ่ม "แก้ไขข้อมูลนิยาย": ไปยังหน้าแก้ไข (คล้ายหน้าสร้าง)
            * ปุ่ม "ลบนิยาย": พร้อม Modal ยืนยัน (สีแดง, DaisyUI Modal)
    * **ส่วนปุ่มเพิ่มตอน (Add Chapter Buttons - สำหรับผู้สร้างนิยาย/Admin - DaisyUI Button Group):**
        * ปุ่ม "เพิ่มตอนด้วย .txt": เปิด Modal เพิ่มตอนด้วย .txt (ดูข้อ 8.8.1)
        * ปุ่ม "เพิ่มตอน": เปิด Modal เพิ่ม/แก้ไขตอน (ดูข้อ 8.8.2)
    * **ส่วนสารบัญ (Table of Contents Section - DaisyUI Collapse):**
        * คลิกหัวข้อ "สารบัญ" (หรือชื่อนิยาย) เพื่อเปิด/ปิดรายการตอน
        * **รายการตอน (แต่ละแถวแนวตั้ง):**
            * ชื่อตอน (Chapter Title - Sarabun)
            * สถานะการอ่าน: หากผู้ใช้อ่านแล้ว แสดง ไอคอนรูปตา (Animation), พื้นหลังแถวอาจเปลี่ยนเป็นสีเทาอ่อน
            * (นักแปล/Admin) จำนวนเส้นมาร์ค (Mark Lines): " (X มาร์ค)"
            * (ผู้สร้าง/Admin) ปุ่มจัดการตอน:
                * ปุ่ม "แก้ไขตอน" (ไอคอนรูปดินสอ): เปิด Modal เพิ่ม/แก้ไขตอน (ดูข้อ 8.8.2) โหลดข้อมูลตอนปัจจุบัน
                * ปุ่ม "ลบตอน" (ไอคอนรูปถังขยะ): พร้อม Modal ยืนยัน (DaisyUI Modal)
            * (นักแปล/Admin) ปุ่ม "คัดลอกมาร์คทั้งหมดของตอนนี้" (ไอคอนรูป copy)
            * สถานะตอน (Chapter Status): ไอคอนลูกโลก (เผยแพร่), ไอคอนเอกสาร (ฉบับร่าง) (แสดงเฉพาะผู้สร้าง/Admin หรือตามความเหมาะสม)
        * (ถ้ามีตอนมาก) Pagination หรือ Scroll ภายใน Content ของ Collapse
* **การเข้าถึง:** สมาชิกเห็นเฉพาะตอน `published` (หลังเข้าสู่ระบบ), นักแปล/Admin เห็นทุกตอน

### 8.3 หน้าบุ๊คมาร์ค (Bookmarks Page)

* **Stack:** Supabase (จัดการ relation `user_bookmarks`), DaisyUI (Table หรือ Card List, Button, Modal สำหรับยืนยัน, Breadcrumbs)
* **Breadcrumbs:** แสดง เช่น หน้าแรก > บุ๊คมาร์ค
* **UI:** ธีมทอง-เทา, ฟอนต์ Sarabun, ตอบสนองทุกอุปกรณ์, UI ภาษาไทย
* **ปุ่ม "อ่าน"** พาไปยังตอนที่ Bookmark ในโหมดที่เหมาะสมกับบทบาท (ต้องการการเข้าสู่ระบบ)

### 8.4 แดชบอร์ดนักแปล (Translator Dashboard)

* **Stack:** Supabase, DaisyUI (Breadcrumbs), Custom JavaScript สำหรับ Line Numbers, บรรทัดมาร์ค และ Copy Mark functionality
* **Breadcrumbs:** แสดง เช่น หน้าแรก > แดชบอร์ดนักแปล
* **Layout:**
    * แสดงรายการนิยายที่นักแปลมีสิทธิ์จัดการ (นิยายที่ตนเองสร้าง หรือได้รับมอบหมาย)
    * สถิติส่วนตัว (จำนวนตอนที่ Mark, นิยายที่กำลังทำ)
* คลิกเลือกนิยายและตอน จะเข้าสู่ "โหมดแปล" โดยตรง (ต้องการการเข้าสู่ระบบและสิทธิ์นักแปล)

### 8.5 หน้าอ่านนิยายและโหมดแปล (Reading & Translator Mode UI)

* **Stack:** DaisyUI (Toolbar, Modal, Button, Range/Slider, Progress, Breadcrumbs), Custom JavaScript
* **Path (โหมดแปล):** `/translate/[novel_slug]/[chapter_slug]`
* **Path (โหมดอ่าน):** `/[novel_slug]/[chapter_slug]`
* **Breadcrumbs:** แสดง เช่น หน้าแรก > [ชื่อนิยาย] > [ชื่อตอน] (ลิงก์ไปหน้าข้อมูลนิยาย และหน้าแรก)
* **UI Layout (Single Pane View เน้นเนื้อหา):**
    * **การเลื่อนหน้า:** เนื้อหาจำกัดความกว้าง, เลื่อนแนวตั้งเท่านั้น (No Horizontal Scrollbar)
    * **แถบความคืบหน้าการอ่าน (Reading Progress Bar):** แสดงด้านบนหรือล่าง (แถบบางๆ สีทอง, DaisyUI Progress) คำนวณจาก Scroll Position / Total Scrollable Height
    * **พื้นที่เนื้อหา (Content Area):**
        * แสดงเนื้อหานิยาย (`chapters.content_original` สำหรับโหมดแปล, หรือเนื้อหาประมวลผลสำหรับสมาชิก) (Sarabun หรือฟอนต์ภาษาต้นฉบับ)
        * (โหมดแปล) หมายเลขบรรทัด (Line Numbers): แสดงด้านข้างเนื้อหาต้นฉบับ, สีเทากลาง
        * (โหมดแปล) การโต้ตอบกับเส้นมาร์ค (MarkLine Interaction):
            * **คลิก (Click):** มาร์ค/ยกเลิกมาร์คบรรทัด (เปลี่ยนสีพื้นหลังบรรทัดเป็นสีทองอ่อนๆ หรือมีสัญลักษณ์ติ๊กถูกสีทอง) -> บันทึกใน `mark_lines.marked_lines_data`
            * **ดับเบิลคลิก (Double-click):** คัดลอกข้อความต้นฉบับของบรรทัดนั้นไปยัง Clipboard + ไฮไลท์บรรทัดสีทองโปร่งแสงชั่วคราว
    * **แถบเครื่องมือลอย (Floating Toolbar - DaisyUI Button Group หรือ Custom Styling):**
        * ไอคอนสารบัญ (List/Menu Icon): เปิด Modal/Dropdown แสดงรายการตอน (คล้าย Collapse ในหน้าข้อมูลนิยาย)
        * ไอคอนปรับแต่งการแสดงผล "Aa": เปิด Popover/Modal (DaisyUI Modal/Popover)
            * ขนาดตัวอักษร (DaisyUI Range หรือ Button - / +)
            * ระยะห่างระหว่างบรรทัด (DaisyUI Range หรือ Button - / +)
            * สีพื้นหลัง (ตัวเลือกสี: ขาว-ดำ, เหลืองนวล/ซีเปีย-ดำ, ดำ-ขาว - DaisyUI Button Group)
            * (ถ้ามี) รูปแบบตัวอักษร
        * ไอคอนบุ๊คมาร์ค (Bookmark Icon)
        * (โหมดแปลเท่านั้น) เครื่องมือสำหรับนักแปล:
            * ไอคอน "คัดลอกมาร์คทั้งหมด" (Copy All Marks)
            * ไอคอน "ล้างมาร์คทั้งหมด" (Clear Marks) (พร้อมยืนยันผ่าน DaisyUI Modal)
        * ปุ่มสลับธีมเว็บ (Website Theme Toggle - ไอคอนพระจันทร์/พระอาทิตย์, DaisyUI Swap)
        * ปุ่มนำทาง < (ตอนก่อนหน้า), > (ตอนถัดไป)
* **การตอบสนองบนมือถือ:** เนื้อหา, หมายเลขบรรทัด, แถบเครื่องมือใช้งานสะดวก (แถบเครื่องมืออาจอยู่ล่างจอ, ปุ่มใหญ่ขึ้น)

### 8.6 แดชบอร์ดผู้ดูแลระบบ (Admin Dashboard)

* **Stack:** DaisyUI (Table, Modal, Button, Breadcrumbs, Stats, Tabs หรือ Accordion), Supabase (Admin API, Policies, Functions)
* **Breadcrumbs:** หน้าแรก > แดชบอร์ดผู้ดูแลระบบ (หน้าหลัก) หรือ หน้าแรก > แดชบอร์ดผู้ดูแลระบบ > [ชื่อส่วน]
* **Layout:** Sidebar Menu (Desktop), Main Content Area (ต้องการการเข้าสู่ระบบและสิทธิ์ Admin)
* **ส่วนต่างๆ ในแดชบอร์ดผู้ดูแลระบบ:**
    1.  **ภาพรวมสถิติ (Statistics Overview - DaisyUI Stats):**
        * การ์ดสถิติ: จำนวนผู้ใช้ (ทั้งหมด, ตามบทบาท), จำนวนนิยาย, จำนวนตอน (ทั้งหมด, ตามสถานะ: เผยแพร่, ฉบับร่าง), (ถ้าต้องการ) จำนวนบุ๊คมาร์ค, จำนวนบรรทัดมาร์ค, กราฟแนวโน้ม (Recharts), รายการกิจกรรมล่าสุด
    2.  **การจัดการผู้ใช้ (User Management - DaisyUI Table):**
        * ตาราง: รูปโปรไฟล์ (Avatar), ชื่อที่แสดงผล, อีเมล, บทบาทปัจจุบัน (Badge สีตามบทบาท), วันที่สมัคร, สถานะ (ใช้งาน/ถูกระงับ - Badge สี)
        * ค้นหา/กรอง: ตามชื่อ, อีเมล, บทบาท, สถานะ
        * ดำเนินการ (Dropdown/กลุ่มไอคอนต่อแถว): ดูรายละเอียด (Modal), แก้ไขข้อมูล (ชื่อ), เปลี่ยนบทบาท (Modal ยืนยัน), ระงับ/ยกเลิก (Modal ยืนยัน), ลบบัญชี (Soft delete, Modal ยืนยันผลกระทบ)
    3.  **การจัดการเวอร์ชัน (Version Control Management):**
        * เลือกนิยาย/ตอน -> ตารางแสดงเวอร์ชัน (เลขเวอร์ชัน, วันที่แก้ไข, ผู้แก้ไข, หมายเหตุสั้นๆ)
        * ดำเนินการ: ดูเนื้อหาเวอร์ชัน (Modal/Read-only), ย้อนกลับ (Revert - Modal ยืนยัน), ลบเวอร์ชัน (Modal ยืนยัน, มีข้อจำกัด)
    4.  **การตั้งค่าเว็บไซต์ (Site Settings - DaisyUI Tabs/Accordion):**
        * **ทั่วไป:** ชื่อเว็บไซต์, คำอธิบาย (SEO), ข้อความประกาศส่วนกลาง (เปิด/ปิด, แก้ไข), จำนวนรายการต่อหน้าเริ่มต้น
        * **การลงทะเบียนและการเข้าสู่ระบบ:** เปิด/ปิดการลงทะเบียน, (ถ้ามี) ตั้งค่า OAuth
        * **เนื้อหา:** เปิด/ปิดระบบความคิดเห็น (ถ้ามี), ตั้งค่าอัปโหลด (ขนาดไฟล์ปก, ประเภทไฟล์ .txt)
        * **การแจ้งเตือน:** อีเมลผู้ส่ง, Template ข้อความอีเมล (ยืนยัน, รีเซ็ตรหัสผ่าน)
        * **การจัดการหมวดหมู่นิยาย:** รายการหมวดหมู่, ฟอร์มเพิ่ม, แก้ไขชื่อ, ลบ (พร้อมคำเตือน/ตัวเลือกย้ายนิยาย)
    5.  **Log การทำงานของระบบ (Activity Logs - DaisyUI Table):**
        * ตาราง: วันที่, ผู้ใช้ (หรือ "ระบบ"), ประเภทการกระทำ, รายละเอียด (ID สิ่งที่ถูกกระทำ, ค่าเก่า/ใหม่)
        * กรอง: ตามช่วงวันที่, ประเภทการกระทำ, ผู้ใช้
        * (Optional) ปุ่ม Export Log
    6.  **การตรวจสอบข้อมูลบรรทัดมาร์ค (Mark Line Data Review):**
        * ใช้ View `admin_markline_overview` แสดงข้อมูลในตาราง (DaisyUI Table)
        * ค้นหา/กรอง: ตามชื่อนักแปล (`translator_name`), ชื่อนิยาย (`novel_title`), ชื่อตอน (`chapter_title`)
        * คอลัมน์: ชื่อนักแปล, ชื่อนิยาย, ชื่อตอน, เลขที่ตอน, จำนวนมาร์ค (`total_marks_in_chapter`), ข้อมูลมาร์ค (`marked_lines_data`), วันที่อัปเดตล่าสุด (`markline_last_updated`)

### 8.7 Modal สร้างนิยาย (Create Novel Modal)

* **การเรียกใช้งาน:** คลิกปุ่ม "[+ สร้างนิยาย]" ใน Navbar (ต้องการสิทธิ์นักแปล/Admin)
* **Stack:** React Hook Form, Zod, DaisyUI (Modal, Input, Textarea, Dropdown/Select, FileInput, Checkbox, Radio, Button), Supabase (Storage for cover, Database for novel data)
* **UI ภายใน Modal (DaisyUI Modal):**
    * หัวเรื่อง Modal: "สร้างนิยายใหม่"
    * ฟอร์ม:
        * ชื่อเรื่อง (Title): (Input text, required)
        * คำโปรย (Synopsis): (Textarea)
        * หมวดหมู่ (Category): (Dropdown - DaisyUI Select)
        * ปกนิยาย (Cover Image): (อัปโหลดไฟล์ - DaisyUI FileInput, รองรับ jpg, png, webp, แสดง Preview)
        * ภาษาต้นฉบับ (Original Language): (Dropdown - DaisyUI Select, required, ตัวเลือก: จีน (zh), อังกฤษ (en), ไทย (th))
        * สิทธิ์ในการมองเห็นนิยายนี้ (Novel Visibility): (Radio buttons - DaisyUI Radio: ทุกคน (All Members), เฉพาะนักแปล (Translators Only))
        * ค่าเริ่มต้นสำหรับการสร้างตอน (Default Chapter Settings):
            * สถานะเริ่มต้นของตอน (Default Chapter Status): (Radio buttons - DaisyUI Radio: เผยแพร่ (Published), ฉบับร่าง (Draft))
            * สิทธิ์การเข้าถึงตอนเริ่มต้น (Default Chapter Access): (Checkbox - DaisyUI Checkbox, แสดงตามสิทธิ์นิยาย: สมาชิก (Member), นักแปล (Translator))
    * ปุ่มดำเนินการ: "สร้างนิยาย" (DaisyUI Button - สีทองหลัก), "ยกเลิก" (DaisyUI Button - สีเทา หรือ Outline) หรือปุ่มปิด Modal (X)
* **การตอบสนองบนมือถือ:** Modal ปรับขนาด, เนื้อหา Scroll ได้

### 8.8 การจัดการตอน (Chapter Management - ผ่าน Modals)

#### 8.8.1 Modal เพิ่มตอนด้วย .txt (Batch Upload Modal)

* **การเรียกใช้งาน:** คลิก "เพิ่มตอนด้วย .txt" ในหน้าข้อมูลนิยายและสารบัญ (ข้อ 8.2) (ต้องการสิทธิ์ผู้สร้างนิยาย/Admin)
* **Stack:** DaisyUI (Modal, FileInput, Progress), Supabase (Storage for .txt, Database for chapter data)
* **UI ภายใน Modal (DaisyUI Modal):**
    * หัวเรื่อง Modal: "เพิ่มตอนด้วยไฟล์ .txt"
    * ส่วนอัปโหลดไฟล์: Drag & Drop Area หรือ ปุ่ม "เลือกไฟล์" (DaisyUI FileInput), รองรับหลายไฟล์ (สูงสุด 500), แสดงรายการไฟล์พร้อมสถานะ
    * การจัดการไฟล์ซ้ำ (ตามชื่อตอน): ตัวเลือก "ข้าม (Skip)", "เขียนทับ (Overwrite)" ต่อไฟล์
    * ความคืบหน้า (Progress): แถบความคืบหน้าโดยรวม (Overall Progress Bar - DaisyUI Progress), อาจมีรายไฟล์
    * ข้อความสรุป: จำนวนตอนที่เพิ่มสำเร็จ, ข้าม, เขียนทับ
    * ปุ่มดำเนินการ: "เริ่มอัปโหลด", "ยกเลิก", "ปิด"

#### 8.8.2 Modal เพิ่ม/แก้ไขตอน (Create/Edit Chapter Modal)

* **การเรียกใช้งาน:**
    * เพิ่มตอน: คลิก "เพิ่มตอน" ในหน้าข้อมูลนิยาย (ข้อ 8.2) (ต้องการสิทธิ์ผู้สร้างนิยาย/Admin)
    * แก้ไขตอน: คลิกไอคอนดินสอในสารบัญ (ข้อ 8.2) (โหลดข้อมูลตอนมาแสดง) (ต้องการสิทธิ์ผู้สร้างนิยาย/Admin)
* **Stack:** TipTap หรือ Editor.js, DaisyUI (Modal, Input, Radio, Checkbox, Button), Supabase
* **UI ภายใน Modal (DaisyUI Modal):**
    * หัวเรื่อง Modal: "เพิ่มตอนใหม่" หรือ "แก้ไขตอน: [ชื่อตอนปัจจุบัน]"
    * ฟอร์ม:
        * ชื่อตอน (Chapter Title): (Input text, required)
        * เนื้อหา (Content): (Rich Text Editor - TipTap หรือ Editor.js)
        * สถานะ (Status): (Radio buttons - DaisyUI Radio: เผยแพร่ (Published) พร้อมไอคอนลูกโลก, ฉบับร่าง (Draft) พร้อมไอคอนเอกสาร)
        * สิทธิ์การเข้าถึง (Access Level): (Checkbox group - DaisyUI Checkbox: สมาชิก (Member), นักแปล (Translator)) (Admin เห็นทุกอย่าง)
    * ปุ่มดำเนินการใน Modal:
        * (สำหรับเพิ่มตอนใหม่): "เผยแพร่ตอน" (สีทองหลัก), "บันทึกเป็นฉบับร่าง" (สีทองรอง)
        * (สำหรับแก้ไขตอน): "บันทึกการเปลี่ยนแปลง" (สีทองหลัก), "ลบตอน" (สีแดง, Modal ยืนยัน), "ยกเลิก" (สีเทา/Outline) หรือปุ่มปิด (X)

### 8.9 หน้าโปรไฟล์ (Profile Page)

* **Stack:** DaisyUI (Modal หรือ Tabs, Avatar, Form, Input, Button, Breadcrumbs), Supabase (Auth for password change, Storage for avatar, Database for display name)
* **Breadcrumbs:** แสดง เช่น หน้าแรก > โปรไฟล์
* **UI:** ธีมทอง-เทา, ฟอนต์ Sarabun, ตอบสนองทุกอุปกรณ์, UI ภาษาไทย (ต้องการการเข้าสู่ระบบ)
* **ฟังก์ชัน:**
    * แก้ไขชื่อที่แสดงผล (`profiles.display_name`)
    * เปลี่ยนรูปโปรไฟล์ (Avatar): อัปโหลดไฟล์ใหม่ไปที่ `avatars` bucket และอัปเดต `profiles.avatar_url`
    * เปลี่ยนรหัสผ่าน (ใช้ Supabase Auth)

---

## 9. โครงสร้างโฟลเดอร์โปรเจกต์ (Project Folder Structure)

โครงสร้างนี้เป็นแนวทางสำหรับโปรเจกต์ Next.js ที่ใช้ TypeScript และ Supabase แสดงการแบ่งส่วนของ Frontend (เช่น `src/app`, `src/components`) และส่วนที่ติดต่อกับ Backend (เช่น `src/lib/supabase`, `src/services`, `src/app/api`).

```text
inkrealm-project/
├── .github/                    # GitHub Actions Workflows (CI/CD - ส่วนหนึ่งของ DevOps)
│   └── workflows/
│       └── deploy.yml          # CI/CD Workflow
├── .vscode/                    # VSCode settings (optional)
│   └── settings.json
├── public/                     # Frontend: Static assets
│   ├── fonts/                  # (ถ้ามีฟอนต์ที่ host เอง)
│   └── images/                 # รูปภาพ static อื่นๆ
├── src/
│   ├── app/                    # Frontend: Next.js App Router (Routing, Pages, Layouts)
│   │   ├── (auth)/             # Route group สำหรับหน้า Authentication
│   │   │   ├── login/
│   │   │   │   └── page.tsx    # อาจไม่จำเป็นถ้าใช้ Modal เป็นหลัก แต่มีไว้สำหรับ direct access/fallback
│   │   │   ├── register/
│   │   │   │   └── page.tsx    # อาจไม่จำเป็นถ้าใช้ Modal เป็นหลัก
│   │   │   └── forgot-password/
│   │   │       └── page.tsx
│   │   ├── (main)/             # Route group สำหรับหน้าหลักที่ต้องมี Layout (Navbar, Footer)
│   │   │   ├── layout.tsx      # Layout หลัก (Navbar, Footer)
│   │   │   ├── page.tsx        # หน้าแรก (Home page) - จะแสดง Login Modal ที่นี่
│   │   │   ├── bookmarks/
│   │   │   │   └── page.tsx
│   │   │   ├── novels/
│   │   │   │   ├── [novel_slug]/
│   │   │   │   │   ├── page.tsx            # หน้าข้อมูลนิยายและสารบัญ
│   │   │   │   │   └── (reader)/
│   │   │   │   │       ├── [chapter_slug]/
│   │   │   │   │       │   └── page.tsx    # หน้าอ่านนิยาย (สมาชิก)
│   │   │   ├── translate/
│   │   │   │   ├── [novel_slug]/
│   │   │   │   │   ├── [chapter_slug]/
│   │   │   │   │   │   └── page.tsx    # หน้าโหมดแปล (นักแปล)
│   │   │   ├── profile/
│   │   │   │   └── page.tsx
│   │   │   ├── translator-dashboard/
│   │   │   │   └── page.tsx
│   │   │   └── admin-dashboard/
│   │   │       ├── page.tsx
│   │   │       ├── users/
│   │   │       │   └── page.tsx
│   │   │       ├── versions/
│   │   │       │   └── page.tsx
│   │   │       └── settings/
│   │   │           └── page.tsx
│   │   ├── api/                  # Backend: API Routes (Next.js Route Handlers - Serverless Functions)
│   │   │   └── auth/
│   │   │       └── callback/
│   │   │           └── route.ts  # Supabase Auth callback
│   │   ├── layout.tsx          # Root layout
│   │   └── global.css          # Global styles (Tailwind directives)
│   ├── components/             # Frontend: React Components (UI)
│   │   ├── auth/               # Components สำหรับ Auth pages และ Login Modal
│   │   │   └── LoginModal.tsx  # Component สำหรับ Login Modal
│   │   ├── common/             # Components ที่ใช้ซ้ำทั่วไป (Button, Modal Shell, Card, etc.)
│   │   ├── layout/             # Navbar, Footer, Sidebar components
│   │   ├── novel/              # Components เกี่ยวกับนิยาย (NovelCard, ChapterList)
│   │   ├── reader/             # Components สำหรับหน้าอ่าน (ReadingToolbar, DisplaySettings)
│   │   ├── translator/         # Components สำหรับโหมดแปล (MarkLine, TranslatorToolbar)
│   │   └── admin/              # Components สำหรับ Admin Dashboard
│   ├── contexts/               # Frontend: React Contexts (e.g., ThemeContext, AuthContext, ModalContext)
│   ├── hooks/                  # Frontend: Custom React Hooks (e.g., useAuth, useModal)
│   ├── lib/                    # Shared: Helper functions, Supabase client, utilities
│   │   ├── supabase/           # Supabase client configurations
│   │   │   ├── client.ts       # Supabase client instance (for client-side)
│   │   │   ├── server.ts       # Supabase client instance (for server-side/Route Handlers)
│   │   │   └── admin.ts        # Supabase admin client (for privileged operations - use with caution on server-side)
│   │   ├── zod-schemas.ts    # Shared: Zod schemas for validation (used by Frontend forms and Backend API)
│   │   └── utils.ts            # Shared: Utility functions
│   ├── services/               # Frontend: API call functions, data fetching logic to interact with Backend
│   ├── store/                  # Frontend: State management (e.g., Zustand, Redux) - (optional)
│   ├── styles/                 # Frontend: Additional global styles or theme configurations
│   ├── types/                  # Shared: TypeScript type definitions
│   │   ├── database.types.ts   # Auto-generated types from Supabase schema (Backend structure for Frontend use)
│   │   └── index.ts            # Custom types
│   └── middleware.ts           # Backend (Edge): Next.js Middleware (e.g., for auth protection, redirect logic)
├── .env.local                  # Local environment variables (ignored by Git)
├── .env.example                # Example environment variables
├── .eslintrc.json              # ESLint configuration
├── .gitignore                  # Git ignore file
├── next.config.mjs             # Next.js configuration
├── package.json
├── postcss.config.js           # PostCSS configuration (for TailwindCSS)
├── tailwind.config.ts          # TailwindCSS configuration
└── tsconfig.json               # TypeScript configuration


10. ประสบการณ์ผู้ใช้งาน (Leading UX & User Flows)
10.1 การเข้าสู่ระบบ และหน้าแรก (Login & Home Page Experience)
การเข้าสู่เว็บไซต์ครั้งแรก / ผู้ใช้ที่ยังไม่ได้เข้าสู่ระบบ:
ผู้ใช้จะเข้าสู่หน้าแรก (Home Page /)
Modal เข้าสู่ระบบ (Login Modal) จะปรากฏขึ้นโดยอัตโนมัติ (ใช้ DaisyUI Modal) Modal นี้จะมีฟอร์มสำหรับเข้าสู่ระบบ และลิงก์ไปยังการลงทะเบียน หรือลืมรหัสผ่าน
ผู้ใช้สามารถเลือกดำเนินการอย่างใดอย่างหนึ่งใน Modal (เข้าสู่ระบบ, ไปหน้าลงทะเบียน, ฯลฯ)
ผู้ใช้สามารถเลือกที่จะ ปิด Modal เพื่อเข้าชมเนื้อหาบนหน้าแรกที่อนุญาตสำหรับผู้ใช้ทั่วไป (Guest/Anonymous users)
หากผู้ใช้ปิด Modal และพยายามเข้าถึงเนื้อหาหรือฟังก์ชันที่ต้องการการยืนยันตัวตน (เช่น คลิกอ่านตอนนิยาย, เข้าหน้าบุ๊คมาร์ค, สร้างนิยาย) Modal เข้าสู่ระบบจะปรากฏขึ้นอีกครั้ง
ผู้ใช้ที่เข้าสู่ระบบแล้ว:
เมื่อผู้ใช้เข้าสู่เว็บไซต์ จะสามารถเข้าถึงเนื้อหาและฟังก์ชันตามสิทธิ์ของตนเองได้โดยตรง โดย Modal เข้าสู่ระบบจะไม่ปรากฏขึ้น
การคลิกปกนิยายบนหน้าแรกจะนำไปยัง "หน้าข้อมูลนิยายและสารบัญ (/[novel_slug])" โดยตรง
10.2 การเข้าดูนิยายและสารบัญ (Novel Detail Page Flow)
UI ภาษาไทย, สารบัญแบบ Collapse.
เมื่อผู้ใช้อ่านตอนจบ: ระบบจะบันทึก user_id ของผู้ใช้ลงใน chapters.read_by_users ของตอนนั้น (ต้องการการเข้าสู่ระบบ).
หากผู้ใช้ที่ยังไม่ได้เข้าสู่ระบบพยายามเข้าถึงหน้านี้หรือคลิกอ่านตอน Modal เข้าสู่ระบบจะปรากฏขึ้น.
10.3 การใช้งานบุ๊คมาร์ค (Bookmark Functionality)
UI ภาษาไทย.
ปุ่ม "อ่าน" พาไปยังตอนที่ Bookmark ในโหมดที่เหมาะสมกับบทบาท.
การใช้งานฟังก์ชันบุ๊คมาร์ค (เพิ่ม/ลบ/ดู) ต้องการการเข้าสู่ระบบ (Modal เข้าสู่ระบบจะปรากฏหากยังไม่ได้ login).
10.4 การเปลี่ยน Avatar
ผู้ใช้ไปที่หน้าโปรไฟล์ (Profile Page - ต้องการการเข้าสู่ระบบ).
เลือกอัปโหลดรูปใหม่.
ระบบ Frontend ส่งไฟล์ไป Backend (API Route).
Backend อัปโหลดไฟล์ไปยัง avatars bucket ใน Supabase Storage และอัปเดต avatar_url ในตาราง profiles.
10.5 ขั้นตอนการแปลและการจัดการเส้นมาร์ค (Translator Flow)
(ต้องการการเข้าสู่ระบบและสิทธิ์นักแปล)
การสร้างตอนใหม่ (โดยนักแปล):
นักแปลสร้างตอนใหม่ (ผ่าน Modal เพิ่มตอนด้วย .txt หรือ Modal เพิ่ม/แก้ไขตอน).
Backend: ข้อมูลตอนถูกบันทึกลงตาราง chapters. ระบบ (เช่น Trigger หรือ Supabase Function หรือ Logic ใน API Route ตอนสร้างตอน) สร้าง Record ใหม่ในตาราง mark_lines อัตโนมัติ: chapter_id (ID ตอนใหม่), translator_id (ID ผู้สร้าง), marked_lines_data: [].
นักแปลเข้าสู่ "โหมดแปล" ของตอนที่ต้องการ:
หากเป็นครั้งแรกที่นักแปลคนนี้เข้าถึงตอนนี้ และยังไม่มี Record ใน mark_lines สำหรับ chapter_id และ translator_id นี้ ระบบ (Frontend หรือ Backend) จะสร้าง Record ใหม่ให้โดยอัตโนมัติด้วย marked_lines_data = [].
การมาร์คบรรทัด: นักแปลคลิกที่บรรทัด -> หมายเลขบรรทัดของบรรทัดนั้นจะถูกเพิ่มเข้าไปใน Array ของ mark_lines.marked_lines_data (หากยังไม่มี) หรือลบออก (หากมีอยู่แล้ว) สำหรับนักแปลคนนั้นและตอนนี้ (Frontend ส่งคำขอไป Backend API เพื่ออัปเดต).
การล้างมาร์ค: คลิกปุ่ม "ล้างมาร์คทั้งหมด" -> Modal ยืนยัน -> หากยืนยัน mark_lines.marked_lines_data สำหรับนักแปลคนนั้นและตอนนี้จะถูกตั้งค่าเป็น [] (Frontend ส่งคำขอไป Backend API).
การคัดลอกมาร์คทั้งหมด (Copy All Marks):
นักแปลคลิกปุ่ม "คัดลอกมาร์คทั้งหมด".
Backend/Frontend Logic: ดึง marked_lines_data (Array หมายเลขบรรทัด) จาก mark_lines และ content_original จาก chapters -> แยก content_original เป็น Array ของบรรทัด -> สร้าง String ใหม่โดยวน Loop ตามหมายเลขบรรทัดใน marked_lines_data ดึงข้อความมาต่อกัน -> คัดลอก String ไปยัง Clipboard (Frontend) -> แสดง Feedback.
การดับเบิลคลิก: คัดลอกข้อความต้นฉบับของบรรทัดนั้นๆ (Frontend Logic).
10.6 (สำหรับผู้ดูแลระบบ/ผู้ใช้ที่มีสิทธิ์) การวางข้อมูลบรรทัดมาร์คเก่า
ผู้ใช้เข้าถึงตาราง mark_lines (ผ่าน Supabase Studio หรือ Admin Interface ที่สร้างขึ้น).
ค้นหา Record ที่ต้องการ (ตาม chapter_id และ translator_id - อาจใช้ View admin_markline_overview ช่วยค้นหา).
แก้ไข Field marked_lines_data โดยวาง Array ของหมายเลขบรรทัดที่ต้องการทับลงไป.
11. ระบบความปลอดภัย (Security)
Authentication (Backend - Supabase Auth): JWT Auth พร้อม Refresh Token.
Transport Security: HTTPS ตลอดการเชื่อมต่อ.
API Security (Backend - Next.js API Routes & Supabase):
Rate Limiting (สามารถตั้งค่าที่ Vercel หรือ Supabase).
ป้องกัน XSS (Sanitize user input ใน Frontend และ Backend, ใช้ Content Security Policy (CSP) ที่เหมาะสม).
ป้องกัน CSRF (Next.js และ Supabase อาจมีกลไกจัดการส่วนนี้, พิจารณา anti-CSRF tokens หากจำเป็น).
ตรวจสอบสิทธิ์ (Authorization) ก่อนเรียกใช้งาน API ทุกตัว โดยอิงตาม Role และ Permissions (RLS ใน Supabase, Middleware ใน Next.js).
Data Security (Backend - Supabase):
Row-Level Security (RLS) ใน Supabase PostgreSQL เพื่อให้แน่ใจว่าผู้ใช้สามารถเข้าถึงได้เฉพาะข้อมูลที่ตนเองมีสิทธิ์.
Account Security (Backend - Supabase Auth & Frontend Logic):
ฟีเจอร์ยืนยันอีเมล (Email Verification).
การป้องกันการเข้าสู่ระบบที่ไม่สำเร็จหลายครั้ง (Login attempt throttling).
12. การทดสอบและประกันคุณภาพ (Testing & QA)
Unit Test (Frontend): ใช้ Jest หรือ Vitest ร่วมกับ React Testing Library (RTL) สำหรับทดสอบ Components และ Functions.
End-to-End (E2E) Test (Frontend/Full-stack): ใช้ Cypress สำหรับการทดสอบ User Flows ทั้งหมดของแอปพลิเคชัน.
Load Test (Backend/Full-stack): ใช้ k6 หรือ JMeter สำหรับทดสอบประสิทธิภาพของระบบภายใต้ภาระงานสูง.
Accessibility (A11y) Test (Frontend): ใช้ axe-core หรือ Lighthouse ใน Chrome DevTools เพื่อตรวจสอบและปรับปรุงการเข้าถึงเว็บไซต์.
13. การปรับปรุงประสบการณ์ผู้ใช้ (UX Enhancements - Responsive Focus)
ส่วนใหญ่เกี่ยวข้องกับ Frontend:
Empty States: ทุกส่วนที่มีการแสดงรายการข้อมูล (เช่น หน้าแรกไม่มีนิยาย, ไม่มีบุ๊คมาร์ค) ควรมี Empty State ที่สวยงาม พร้อม Call-to-action ที่เหมาะสม.
Loading States:
Skeleton Loaders: ขณะรอข้อมูลโหลด แสดงโครงร่างของ UI.
Spinners/Progress Bars: สำหรับ action ที่ใช้เวลานาน (เช่น อัปโหลดไฟล์, บันทึกข้อมูล).
Notifications/Toasts:
ใช้สำหรับแจ้งผลการกระทำที่ไม่เปลี่ยนหน้า (เช่น "บันทึกการเปลี่ยนแปลงสำเร็จ", "คัดลอกข้อความแล้ว", "เพิ่มในบุ๊คมาร์คแล้ว").
ควรปรากฏในตำแหน่งที่ไม่บดบังเนื้อหาสำคัญ และหายไปเองในเวลาที่เหมาะสม (ใช้ DaisyUI Alert หรือ Toast).
Error Handling & Feedback:
แสดงข้อความ Error ที่เป็นมิตร เข้าใจง่าย และบอกแนวทางการแก้ไข (ถ้าทำได้).
หน้า 404 (Not Found) และ 500 (Server Error) ที่ออกแบบมาเฉพาะสำหรับ INKREALM.
Accessibility (A11y):
Contrast ratio เพียงพอ.
Keyboard navigation สมบูรณ์.
ARIA attributes ที่เหมาะสมสำหรับ dynamic content.
Alt text สำหรับรูปภาพ.
Transitions & Micro-interactions:
การเปลี่ยนหน้า (page transitions) ที่นุ่มนวล.
Hover effects, button press effects ที่ตอบสนองต่อผู้ใช้. (ทั้งหมดนี้ควร subtle ไม่ใช่รกหรือทำให้เว็บช้า)
14. การจัดการข้อมูลและการสำรองข้อมูล (Data Management & Backup)
เกี่ยวข้องกับ Backend (Supabase) และกระบวนการ DevOps:
Supabase Built-in Backups: Supabase ให้บริการสำรองข้อมูลอัตโนมัติสำหรับ PostgreSQL database (Point-in-Time Recovery - PITR อาจขึ้นอยู่กับ Plan ที่เลือก). ควรตรวจสอบนโยบายการสำรองข้อมูลและระยะเวลาการเก็บรักษา (Retention Policy) ของ Supabase.
Supabase Storage Backups: ข้อมูลที่เก็บใน Supabase Storage (เช่น รูปปกนิยาย, ไฟล์ .txt, รูป Avatar) ควรมีแผนการสำรองข้อมูลแยกต่างหาก หรือตรวจสอบว่าบริการของ Supabase ครอบคลุมส่วนนี้อย่างไร. อาจพิจารณาการทำสำเนาไปยัง Storage อื่นเป็นระยะ (เช่น AWS S3, Google Cloud Storage).
Manual/Scheduled Backups (Optional but Recommended):
Database Dumps: พิจารณาการทำ Database Dumps (เช่น pg_dump) เป็นระยะ (เช่น รายวัน หรือ รายสัปดาห์) และเก็บไว้ในที่ปลอดภัยแยกต่างหาก.
Scripting: อาจสร้าง Script สำหรับการทำ Backup อัตโนมัติและแจ้งเตือนเมื่อสำเร็จหรือล้มเหลว.
Testing Restore Procedures: สิ่งสำคัญคือต้องมีการทดสอบกระบวนการกู้คืนข้อมูล (Restore) จาก Backup เป็นประจำ.
Data Retention Policy: กำหนดนโยบายการเก็บรักษาข้อมูลสำรองให้ชัดเจน ว่าจะเก็บไว้นานเท่าใด และมีกี่เวอร์ชัน.
15. Backend: โครงสร้างฐานข้อมูล (Supabase - PostgreSQL)
ชื่อตารางและคอลัมน์เป็นภาษาอังกฤษ. ส่วนนี้เป็นหัวใจของ Backend.
SQL Schema สำหรับสร้างตาราง
-- ตาราง: บทบาทผู้ใช้
CREATE TABLE roles (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL UNIQUE, -- e.g., 'member', 'translator', 'admin'
    permissions JSONB -- สามารถระบุสิทธิ์ต่างๆ ในรูปแบบ JSON
);

-- ตาราง: โปรไฟล์ผู้ใช้
CREATE TABLE profiles (
    id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE, -- Foreign Key to Supabase auth.users
    display_name TEXT,
    avatar_url TEXT, -- URL ของรูปโปรไฟล์ผู้ใช้
    role_id INT REFERENCES roles(id) DEFAULT 1, -- Default to 'member' or appropriate ID
    created_at TIMESTAMPTZ DEFAULT now(),
    updated_at TIMESTAMPTZ DEFAULT now()
);

-- ตาราง: หมวดหมู่นิยาย
CREATE TABLE categories (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL UNIQUE,
    slug TEXT NOT NULL UNIQUE,
    created_at TIMESTAMPTZ DEFAULT now()
);

-- ตาราง: นิยาย
CREATE TABLE novels (
    id SERIAL PRIMARY KEY,
    title TEXT NOT NULL,
    slug TEXT NOT NULL UNIQUE,
    synopsis TEXT,
    cover_image_url TEXT,
    author_id UUID REFERENCES profiles(id) ON DELETE SET NULL, -- ผู้สร้างนิยาย
    category_id INT REFERENCES categories(id) ON DELETE SET NULL,
    original_language TEXT NOT NULL, -- e.g., 'zh', 'en', 'th'
    visibility TEXT DEFAULT 'All Members', -- e.g., 'All Members', 'Translators Only'
    default_chapter_status TEXT DEFAULT 'Published', -- e.g., 'Published', 'Draft'
    default_chapter_access TEXT DEFAULT 'Member', -- e.g., 'Member', 'Translator' (can be JSON array if multiple)
    total_chapters INT DEFAULT 0,
    published_chapters INT DEFAULT 0,
    status TEXT DEFAULT 'ต่อเนื่อง', -- e.g., 'ต่อเนื่อง', 'จบแล้ว'
    created_at TIMESTAMPTZ DEFAULT now(),
    updated_at TIMESTAMPTZ DEFAULT now(),
    last_published_at TIMESTAMPTZ
);

-- ตาราง: ตอนของนิยาย
CREATE TABLE chapters (
    id SERIAL PRIMARY KEY,
    novel_id INT REFERENCES novels(id) ON DELETE CASCADE NOT NULL,
    chapter_number INT NOT NULL, -- ลำดับตอนภายในนิยาย
    title TEXT NOT NULL,
    slug TEXT NOT NULL, -- จะถูกสร้างขึ้น เช่น novel-slug-chapter-number
    content_original TEXT, -- เนื้อหาต้นฉบับ (สำหรับนักแปล) หรือเนื้อหาที่แสดงผล
    status TEXT DEFAULT 'Published', -- e.g., 'Published', 'Draft'
    access_level TEXT DEFAULT 'Member', -- e.g., 'Member', 'Translator' (can be JSON array if multiple)
    created_by UUID REFERENCES profiles(id) ON DELETE SET NULL, -- ผู้สร้าง/เพิ่มตอน
    word_count INT DEFAULT 0,
    read_by_users JSONB DEFAULT '{}'::jsonb, -- เก็บ {"user_uuid_1": true, "user_uuid_2": true}
    created_at TIMESTAMPTZ DEFAULT now(),
    updated_at TIMESTAMPTZ DEFAULT now(),
    published_at TIMESTAMPTZ,
    UNIQUE (novel_id, chapter_number),
    UNIQUE (novel_id, slug)
);

-- ตาราง: บุ๊คมาร์ค
CREATE TABLE bookmarks (
    id SERIAL PRIMARY KEY,
    user_id UUID REFERENCES profiles(id) ON DELETE CASCADE NOT NULL,
    chapter_id INT REFERENCES chapters(id) ON DELETE CASCADE NOT NULL,
    created_at TIMESTAMPTZ DEFAULT now(),
    UNIQUE (user_id, chapter_id)
);

-- ตาราง: บรรทัดที่มาร์คโดยนักแปล
CREATE TABLE mark_lines (
    id SERIAL PRIMARY KEY,
    chapter_id INT REFERENCES chapters(id) ON DELETE CASCADE NOT NULL,
    translator_id UUID REFERENCES profiles(id) ON DELETE CASCADE NOT NULL, -- ID ของนักแปล (จาก profiles.id)
    marked_lines_data JSONB DEFAULT '[]'::jsonb, -- Array ของหมายเลขบรรทัด เช่น [21, 40, 39]
    created_at TIMESTAMPTZ DEFAULT now(),
    updated_at TIMESTAMPTZ DEFAULT now(),
    UNIQUE (chapter_id, translator_id) -- นักแปลหนึ่งคนมีชุดมาร์คเดียวต่อหนึ่งตอน
);
-- หมายเหตุ: สำหรับการแสดงผลที่เป็นมิตรต่อผู้ใช้ (เช่น แสดงชื่อนักแปล, ชื่อนิยาย) ให้ใช้ View `admin_markline_overview`

-- ตาราง: เวอร์ชันของตอน (Optional, for version history)
CREATE TABLE chapter_versions (
    id SERIAL PRIMARY KEY,
    chapter_id INT REFERENCES chapters(id) ON DELETE CASCADE NOT NULL,
    version_number INT NOT NULL,
    title TEXT,
    content_original TEXT,
    changed_by UUID REFERENCES profiles(id) ON DELETE SET NULL,
    created_at TIMESTAMPTZ DEFAULT now(),
    UNIQUE (chapter_id, version_number)
);

-- ตาราง: Log การทำงานของระบบ
CREATE TABLE activity_logs (
    id SERIAL PRIMARY KEY,
    user_id UUID REFERENCES profiles(id) ON DELETE SET NULL, -- ผู้ใช้ที่กระทำการ (ถ้ามี)
    action_type TEXT NOT NULL, -- e.g., 'CREATE_NOVEL', 'UPDATE_USER_ROLE', 'DELETE_CHAPTER'
    target_type TEXT, -- e.g., 'novel', 'user', 'chapter'
    target_id TEXT, -- ID ของสิ่งที่ถูกกระทำ (อาจเป็น INT หรือ UUID จึงใช้ TEXT)
    details JSONB, -- รายละเอียดเพิ่มเติมของการกระทำ (เช่น ค่าเก่า/ค่าใหม่)
    created_at TIMESTAMPTZ DEFAULT now()
);

-- Initial Roles Data
INSERT INTO roles (name, permissions) VALUES
('member', '{}'::jsonb),
('translator', '{"can_create_novel": true, "can_access_translator_mode": true}'::jsonb),
('admin', '{"can_manage_users": true, "can_manage_all_content": true, "can_access_admin_dashboard": true}'::jsonb);

-- Functions for RLS (Row Level Security)
-- Get user's role
CREATE OR REPLACE FUNCTION get_user_role(user_id_input UUID)
RETURNS TEXT AS $$
DECLARE
  user_role TEXT;
BEGIN
  SELECT r.name INTO user_role
  FROM profiles p
  JOIN roles r ON p.role_id = r.id
  WHERE p.id = user_id_input;
  RETURN user_role;
END;
$$ LANGUAGE plpgsql SECURITY DEFINER;

-- Function to update updated_at timestamp
CREATE OR REPLACE FUNCTION trigger_set_timestamp()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Apply trigger to tables
CREATE TRIGGER set_profiles_timestamp
BEFORE UPDATE ON profiles
FOR EACH ROW
EXECUTE FUNCTION trigger_set_timestamp();

CREATE TRIGGER set_novels_timestamp
BEFORE UPDATE ON novels
FOR EACH ROW
EXECUTE FUNCTION trigger_set_timestamp();

CREATE TRIGGER set_chapters_timestamp
BEFORE UPDATE ON chapters
FOR EACH ROW
EXECUTE FUNCTION trigger_set_timestamp();

CREATE TRIGGER set_mark_lines_timestamp
BEFORE UPDATE ON mark_lines
FOR EACH ROW
EXECUTE FUNCTION trigger_set_timestamp();

-- Indexes for performance
CREATE INDEX idx_novels_author_id ON novels(author_id);
CREATE INDEX idx_novels_category_id ON novels(category_id);
CREATE INDEX idx_chapters_novel_id ON chapters(novel_id);
CREATE INDEX idx_bookmarks_user_id ON bookmarks(user_id);
CREATE INDEX idx_bookmarks_chapter_id ON bookmarks(chapter_id);
CREATE INDEX idx_mark_lines_chapter_id ON mark_lines(chapter_id);
CREATE INDEX idx_mark_lines_translator_id ON mark_lines(translator_id);
CREATE INDEX idx_activity_logs_user_id ON activity_logs(user_id);
CREATE INDEX idx_activity_logs_action_type ON activity_logs(action_type);
CREATE INDEX idx_activity_logs_created_at ON activity_logs(created_at);


ตาราง mark_lines และ View admin_markline_overview เพื่อการจัดการข้อมูลมาร์คที่ง่ายขึ้น
ตาราง mark_lines ถูกออกแบบมาเพื่อเก็บข้อมูลการมาร์คบรรทัดของนักแปล โดยเก็บ translator_id (UUID) ซึ่งเป็น Foreign Key ไปยังตาราง profiles และ chapter_id ซึ่งเป็น Foreign Key ไปยังตาราง chapters เพื่อความเป็นมาตรฐานของฐานข้อมูลเชิงสัมพันธ์
เพื่อให้การตรวจสอบและย้ายข้อมูลง่ายขึ้น โดยเฉพาะเมื่อคุณมีนิยายหลายเรื่องและนักแปลหลายคน เราได้สร้าง SQL View ชื่อ admin_markline_overview ขึ้นมา View นี้จะทำการ JOIN ตาราง mark_lines, profiles, chapters, และ novels เข้าด้วยกัน ทำให้คุณสามารถ:
เห็นชื่อนักแปล (Display Name): แทนที่จะเป็น translator_id (UUID) ที่ดูยาก View จะแสดง translator_name ซึ่งดึงมาจาก profiles.display_name
เห็นชื่อนิยายและชื่อตอน: View จะแสดง novel_title และ chapter_title ทำให้ทราบทันทีว่าข้อมูลมาร์คนี้เป็นของนิยายและตอนใด
กรองและค้นหาง่าย: สามารถใช้ View นี้ใน Admin Dashboard เพื่อค้นหาและกรองข้อมูลมาร์คตามชื่อนักแปล, ชื่อนิยาย, หรือชื่อตอนได้สะดวก
การใช้งานสำหรับการย้ายข้อมูล: เมื่อคุณต้องการย้ายข้อมูลมาร์คเก่า คุณสามารถ Query จาก View admin_markline_overview เพื่อระบุ chapter_id และ translator_id (UUID) ที่ถูกต้องของแต่ละ Record ที่ต้องการอัปเดต จากนั้นจึงไปแก้ไขข้อมูล marked_lines_data ในตาราง mark_lines โดยตรง (ผ่าน Supabase Studio หรือ Admin Interface ที่สร้างขึ้น) โดยอ้างอิงจาก id ของตาราง mark_lines หรือคู่ chapter_id และ translator_id
SQL สำหรับสร้าง View admin_markline_overview
CREATE VIEW admin_markline_overview AS
SELECT
    ml.id AS markline_id, -- ID ของ record ในตาราง mark_lines
    p.display_name AS translator_name, -- ชื่อที่แสดงผลของนักแปล (จาก profiles.display_name)
    p.id AS translator_uuid, -- UUID ของนักแปล (เผื่อใช้ในการอ้างอิงกลับไปที่ profiles.id)
    n.title AS novel_title, -- ชื่อนิยาย (จาก novels.title)
    n.id AS novel_id, -- ID นิยาย
    c.title AS chapter_title, -- ชื่อตอน (จาก chapters.title)
    c.chapter_number, -- เลขที่ตอน
    c.id AS chapter_id, -- ID ตอน
    ml.marked_lines_data, -- Array ของหมายเลขบรรทัดที่มาร์คไว้ (JSONB)
    jsonb_array_length(
        CASE
            WHEN ml.marked_lines_data IS NULL THEN '[]'::jsonb
            ELSE ml.marked_lines_data
        END
    ) AS total_marks_in_chapter, -- จำนวนบรรทัดที่มาร์คในตอนนี้
    ml.updated_at AS markline_last_updated -- วันที่อัปเดตข้อมูลมาร์คล่าสุด
FROM
    mark_lines ml
JOIN
    profiles p ON ml.translator_id = p.id -- เชื่อมกับตาราง profiles เพื่อเอาชื่อนักแปล
JOIN
    chapters c ON ml.chapter_id = c.id -- เชื่อมกับตาราง chapters เพื่อเอาข้อมูลตอน
JOIN
    novels n ON c.novel_id = n.id -- เชื่อมกับตาราง novels เพื่อเอาชื่อนิยาย
ORDER BY
    p.display_name, n.title, c.chapter_number;


หมายเหตุเกี่ยวกับ Database (Backend):
profiles.id ควรเชื่อมโยงกับ auth.users.id ของ Supabase โดยอัตโนมัติเมื่อผู้ใช้สมัครสมาชิก.
การใช้ ON DELETE CASCADE และ ON DELETE SET NULL ถูกกำหนดตามความเหมาะสมของความสัมพันธ์.
พิจารณาเพิ่ม Indexes เพิ่มเติมตามความจำเป็นเพื่อประสิทธิภาพในการ Query.
RLS (Row-Level Security) Policies จะต้องถูกกำหนดใน Supabase Dashboard เพื่อควบคุมการเข้าถึงข้อมูลตามบทบาทผู้ใช้.
16. Backend: Supabase Storage Buckets
ส่วนนี้เป็นส่วนหนึ่งของ Backend infrastructure. จำเป็นต้องสร้าง Storage Buckets ต่อไปนี้ใน Supabase Project ของคุณ:
novel-covers:
วัตถุประสงค์: สำหรับเก็บไฟล์รูปปกนิยาย
สิทธิ์การเข้าถึง (Policies):
ผู้ใช้ทั่วไป (Public/Anon): สามารถอ่าน (Read) ได้
ผู้ใช้ที่ยืนยันตัวตนแล้ว (Authenticated Users) ที่มีบทบาท 'translator' หรือ 'admin': สามารถอัปโหลด (Create), แก้ไข (Update), ลบ (Delete) ได้ (ควรจำกัดตาม author_id ของนิยาย หรือผ่าน Functions เพื่อความปลอดภัย)
ประเภทไฟล์ที่อนุญาต: image/jpeg, image/png, image/webp
ขนาดไฟล์สูงสุด: (ตามที่กำหนด เช่น 5MB)
chapter-text-files:
วัตถุประสงค์: สำหรับเก็บไฟล์ .txt ที่นักแปลอัปโหลดเพื่อสร้างตอนแบบ Batch
สิทธิ์การเข้าถึง (Policies):
ผู้ใช้ที่ยืนยันตัวตนแล้ว (Authenticated Users) ที่มีบทบาท 'translator' หรือ 'admin': สามารถอัปโหลด (Create) ได้
ควรมีการลบไฟล์อัตโนมัติหลังจากประมวลผลเสร็จสิ้น หรือตั้งค่าให้เข้าถึงได้เฉพาะ Backend Functions
ประเภทไฟล์ที่อนุญาต: text/plain
ขนาดไฟล์สูงสุด: (ตามที่กำหนด เช่น 2MB ต่อไฟล์)
avatars:
วัตถุประสงค์: สำหรับเก็บไฟล์รูปโปรไฟล์ผู้ใช้ (Avatar)
สิทธิ์การเข้าถึง (Policies):
ผู้ใช้ทั่วไป (Public/Anon): สามารถอ่าน (Read) รูปโปรไฟล์ได้
ผู้ใช้ที่ยืนยันตัวตนแล้ว (Authenticated Users): สามารถอัปโหลด (Create), แก้ไข (Update โดยการอัปโหลดทับ), ลบ (Delete) รูปโปรไฟล์ของตนเองได้ (ควรมีการกำหนด Policy ให้ User สามารถจัดการได้เฉพาะไฟล์ที่ตนเองเป็นเจ้าของ โดยอาจจะตั้งชื่อไฟล์เป็น user_id/<filename> หรือใช้ metadata และ RLS บน Storage)
ประเภทไฟล์ที่อนุญาต: image/jpeg, image/png
ขนาดไฟล์สูงสุด: (ตามที่กำหนด เช่น 2MB)
17. การนำไปใช้งานและการติดตามผล (Deployment & Monitoring)
กระบวนการนี้เกี่ยวข้องกับการนำ Application (Frontend และ Backend APIs) ขึ้น Production และการดูแลระบบ.
DevOps (CI/CD):
GitHub Actions:
Workflow ใน .github/workflows/deploy.yml จะถูก trigger เมื่อมีการ push code ไปยัง branch ที่กำหนด (เช่น main สำหรับ Production, staging สำหรับ Staging).
ขั้นตอนใน Workflow:
Checkout code
Set up Node.js
Install dependencies (npm ci หรือ pnpm install --frozen-lockfile)
Build Next.js application (npm run build หรือ pnpm build) (สร้าง Frontend assets และ Backend API handlers)
(Optional) Run tests (npm test หรือ pnpm test)
Deploy to Vercel (ใช้ Vercel CLI หรือ GitHub Action ของ Vercel)
Environments:
Vercel Environment Variables:
Production Environment: ตั้งค่า Environment Variables ใน Vercel Dashboard สำหรับ Production deployment (เชื่อมกับ main branch).
NEXT_PUBLIC_SUPABASE_URL (Frontend & Backend API access)
NEXT_PUBLIC_SUPABASE_ANON_KEY (Frontend & Backend API access)
SUPABASE_SERVICE_ROLE_KEY (สำหรับ Backend API Routes หรือ Server Components ที่ต้องการสิทธิ์สูง)
Preview/Staging Environment: ตั้งค่า Environment Variables สำหรับ Preview deployments.
Deployment Checklist (ก่อน Production ครั้งแรก):
[ ] Backend (Supabase) Project Setup:
[ ] สร้าง Supabase Project สำหรับ Production.
[ ] ตั้งค่า Authentication (Email provider, Third-party logins ถ้ามี).
[ ] รัน Database schema migrations ทั้งหมด (SQL จากข้อ 15).
[ ] ตั้งค่า Row-Level Security (RLS) Policies ให้ครบถ้วนและถูกต้อง.
[ ] สร้าง Storage Buckets (ตามข้อ 16) พร้อม Policies ที่ถูกต้อง (โดยเฉพาะ avatars bucket สำหรับการเปลี่ยนรูปโปรไฟล์).
[ ] ตั้งค่านโยบาย Backup ของ Database.
[ ] Deployment Platform (Vercel) Project Setup:
[ ] สร้าง Project ใหม่บน Vercel และเชื่อมต่อกับ GitHub Repository.
[ ] กำหนด Production Branch (เช่น main).
[ ] ตั้งค่า Environment Variables ทั้งหมดสำหรับ Production.
[ ] (Optional) ตั้งค่า Custom Domain.
[ ] Codebase & Application (Frontend & Backend Logic):
[ ] ตรวจสอบว่า Environment Variables ถูกเรียกใช้ถูกต้อง.
[ ] ทดสอบ User Flows ทั้งหมด รวมถึงการเปลี่ยน Avatar และการแสดง Modal เข้าสู่ระบบอัตโนมัติ.
[ ] ตรวจสอบ Error Handling และ Logging.
[ ] ตรวจสอบ Responsive Design.
[ ] ทำ A11y checks.
[ ] ลบ console.log ที่ไม่จำเป็น.
[ ] CI/CD Pipeline:
[ ] ตรวจสอบว่า GitHub Actions workflow ทำงานถูกต้อง.
[ ] Monitoring Setup:
[ ] เชื่อมต่อ Sentry.
[ ] ตั้งค่าการแจ้งเตือนสำหรับ Critical Errors.
การ Deploy (ผ่าน Vercel):
Vercel จะทำการ build และ deploy โดยอัตโนมัติเมื่อมีการ push code ไปยัง branch ที่เชื่อมต่อ (เช่น main สำหรับ production, หรือ branch อื่นๆ สำหรับ preview deployments).
ตรวจสอบสถานะการ deploy และ logs ผ่าน Vercel Dashboard.
Monitoring หลัง Deploy:
Sentry: ติดตาม Real-time errors (Frontend และ Backend API).
Supabase Dashboard: Logs, Usage, Reports (Backend).
Vercel Dashboard: Build logs, Deployment status, Analytics (Frontend & Backend API).
18. การตั้งค่าเริ่มต้นสำหรับการพัฒนา (Getting Started - Local Development)
ส่วนนี้จะอธิบายขั้นตอนการตั้งค่าโปรเจกต์เพื่อเริ่มพัฒนาบนเครื่อง local.
สิ่งที่ต้องมี (Prerequisites):
Node.js และ npm/yarn/pnpm: (แนะนำ LTS version ล่าสุด)
Git: สำหรับ Version Control
Supabase Account: สร้างโปรเจกต์ใหม่บน Supabase (สำหรับ Backend)
Supabase CLI (Optional but Recommended): สำหรับจัดการ Database Migrations และ Local Development Supabase CLI
IDE/Text Editor: เช่น VSCode
ขั้นตอนการตั้งค่า:
Clone Repository:
git clone <your-repository-url>
cd inkrealm-project


Install Dependencies (Frontend & Dev tools):
npm install
# หรือ yarn install
# หรือ pnpm install


ตั้งค่า Environment Variables (.env.local สำหรับ Frontend และ Backend API local access):
คัดลอกไฟล์ .env.example ไปเป็น .env.local:
cp .env.example .env.local


เปิดไฟล์ .env.local และกรอกค่าจาก Supabase Project ของคุณ:
NEXT_PUBLIC_SUPABASE_URL: Project URL (Supabase Dashboard > Project Settings > API)
NEXT_PUBLIC_SUPABASE_ANON_KEY: Project API Keys > anon public (Supabase Dashboard)
SUPABASE_SERVICE_ROLE_KEY: Project API Keys > service_role secret (Supabase Dashboard - ข้อควรระวัง: สำหรับ local dev หรือ server-side เท่านั้น)
DATABASE_URL: Connection string > URI (Supabase Dashboard > Project Settings > Database - ถ้าใช้ Supabase CLI)
(Optional) Supabase Local Development Setup (Backend local instance):
Login Supabase CLI: supabase login
Link Project: supabase link --project-ref <your-project-id>
Pull Database Schema: supabase db pull
Start Local Supabase Services: supabase start
Run Database Migrations/Seed Data (Backend Setup):
SQL Schema: นำโค้ด SQL จาก SQL Schema สำหรับสร้างตาราง และ SQL สำหรับสร้าง View admin_markline_overview ไปรันใน SQL Editor ของ Supabase Dashboard หรือผ่าน Supabase CLI migrations.
Seed Data (ถ้ามี): สร้างไฟล์ seed script และรัน.
Generate Supabase Types (เพื่อให้ Frontend รู้จักโครงสร้าง Backend):
npx supabase gen types typescript --project-id <your-project-id> --schema public > src/types/database.types.ts
# หรือถ้าใช้ local dev กับ Supabase CLI
# npx supabase gen types typescript --local > src/types/database.types.ts


Run Development Server (Frontend & Backend API local):
npm run dev
# หรือ yarn dev
# หรือ pnpm dev

แอปพลิเคชันควรจะเปิดให้เข้าชมได้ที่ `http
