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
```
## 10. ประสบการณ์ผู้ใช้งาน (Leading UX & User Flows)

### 10.1 การเข้าสู่ระบบและหน้าแรก (Login & Home Page Experience)

#### 10.1.1 ผู้ใช้ที่ยังไม่ได้เข้าสู่ระบบ
- เมื่อเข้าหน้าแรก (`/`) จะเปิด Login Modal ขึ้นอัตโนมัติ (DaisyUI Modal)
  - ฟอร์มเข้าสู่ระบบ (Email/Password) พร้อมลิงก์ไปยัง “ลงทะเบียน” และ “ลืมรหัสผ่าน”
  - ปุ่ม “ปิด” เพื่อดูเนื้อหาสำหรับ Guest/Anonymous
- หาก Guest พยายามเข้าถึงฟังก์ชันที่ต้องการการยืนยันตัวตน (อ่านตอน, บุ๊คมาร์ค, สร้างนิยาย ฯลฯ) ให้แสดง Login Modal อีกครั้ง

#### 10.1.2 ผู้ใช้ที่เข้าสู่ระบบแล้ว
- เข้าถึงเนื้อหาและฟังก์ชันตามบทบาทได้ทันที (Login Modal จะไม่ปรากฏ)
- คลิกที่ปกนิยายบนหน้าแรก → ไปยัง “หน้าข้อมูลนิยายและสารบัญ” โดยตรง

---

### 10.2 การเข้าดูนิยายและสารบัญ (Novel Detail Page Flow)
- UI ภาษาไทย, สารบัญแบบ Collapse
- เมื่ออ่านตอนจบ ระบบบันทึก `user_id` ลงใน `chapters.read_by_users`
- หากยังไม่เข้าสู่ระบบและคลิกอ่านตอน → แสดง Login Modal

---

### 10.3 การใช้งานบุ๊คมาร์ค (Bookmark Functionality)
- UI ภาษาไทย
- ปุ่ม “อ่าน” นำไปยังตอนที่ Bookmark ในโหมดที่เหมาะสมกับบทบาท
- เพิ่ม/ลบ/ดูบุ๊คมาร์ค ต้องเข้าสู่ระบบ (Guest → Login Modal)

---

### 10.4 การเปลี่ยน Avatar (Avatar Change)
- เข้า “หน้าโปรไฟล์” (ต้องเข้าสู่ระบบ)
- อัปโหลดรูปใหม่ → Frontend ส่งไฟล์ไปยัง API Route
- Backend อัปโหลดไปยัง `avatars` bucket และอัปเดต `profiles.avatar_url`

---

### 10.5 ขั้นตอนการแปลและการจัดการเส้นมาร์ค (Translator Flow)

> **ต้องเข้าสู่ระบบ และมีสิทธิ์ `translator`**

#### 10.5.1 การสร้างตอนใหม่
- ผ่าน Modal “เพิ่มตอนด้วย .txt” (Batch Upload) หรือ Modal “เพิ่ม/แก้ไขตอน”
- Backend สร้าง record ใน `mark_lines` อัตโนมัติ
  - `chapter_id`, `translator_id`, `marked_lines_data: []`

#### 10.5.2 เข้าโหมดแปล (Translator Mode)
- หากยังไม่มี Record ใน `mark_lines` สำหรับ `(chapter_id, translator_id)` → สร้างใหม่
- **มาร์ค/ยกเลิกมาร์คบรรทัด**  
  - Frontend ส่ง API → อัปเดต `marked_lines_data` (JSONB array)
- **ล้างมาร์คทั้งหมด**  
  - คลิก “ล้างมาร์คทั้งหมด” → Modal ยืนยัน → ตั้ง `marked_lines_data = []`

#### 10.5.3 คัดลอกมาร์คทั้งหมด (Copy All Marks)
- ดึง `marked_lines_data` และ `content_original`
- แยกเนื้อหาเป็น array ของบรรทัด → รวมข้อความตามหมายเลขบรรทัด → คัดลอกไป Clipboard (Frontend)

#### 10.5.4 คัดลอกบรรทัดแบบดับเบิลคลิก (Double-click Copy)
- ดับเบิลคลิกที่บรรทัด → คัดลอกข้อความต้นฉบับของบรรทัดนั้น

---

### 10.6 การย้ายข้อมูลบรรทัดมาร์คเก่า (Legacy Mark Lines Management)
- ใช้ View `admin_markline_overview` ใน Supabase Studio หรือ Admin Interface
  - ค้นหาโดย `translator_name`, `novel_title`, `chapter_title`
  - ระบุ `markline_id` หรือ `(chapter_id, translator_id)`
- แก้ไข `marked_lines_data` ในตาราง `mark_lines` โดยตรง


### 11. ระบบความปลอดภัย (Security)
- **Authentication:** Supabase Auth + JWT (มี Refresh Token)
- **Transport Security:** HTTPS ตลอดการเชื่อมต่อ
- **API Security:**  
  - Rate-limiting (Vercel/Supabase)  
  - XSS protection (Sanitize input + CSP)  
  - CSRF protection (Anti-CSRF tokens หากจำเป็น)  
  - Authorization: ตรวจสิทธิ์ทุก API โดยใช้ RLS ใน Supabase และ Middleware ใน Next.js  
- **Data Security:**  
  - Row-Level Security (RLS) ควบคุมการเข้าถึงตามบทบาท  
  - Email verification  
  - Login attempt throttling  

---

### 12. การทดสอบและประกันคุณภาพ (Testing & QA)
- **Unit Tests (Frontend):** Jest/Vitest + React Testing Library  
- **End-to-End Tests:** Cypress  
- **Load Tests:** k6 หรือ JMeter  
- **Accessibility (A11y):** axe-core / Lighthouse  
- **Test Restore Procedures:** ทดสอบกู้คืนจากสำรองข้อมูลเป็นระยะ  

---

### 13. การปรับปรุงประสบการณ์ผู้ใช้ (UX Enhancements – Responsive Focus)
- **Empty States:** มีข้อความเชิญชวนและ CTA  
- **Loading States:**  
  - Skeleton loaders  
  - Spinners/Progress bars  
- **Notifications/Toasts:** DaisyUI Alert/Toast  
- **Error Handling:** ข้อความชัดเจน แนะนำทางแก้ไข  
- **A11y:**  
  - Contrast ratio เพียงพอ  
  - Keyboard navigation  
  - ARIA attributes + Alt text  
- **Transitions & Micro-interactions:** นุ่มนวล ไม่รก  

---

### 14. การจัดการข้อมูลและการสำรองข้อมูล (Data Management & Backup)
- **Supabase DB Backups:** ตรวจสอบ PITR และ Retention Policy  
- **Supabase Storage Backups:** สำรอง `avatars`, `novel-covers`, `chapter-text-files` ไปยังที่เก็บอื่น (เช่น S3)  
- **Manual/Scheduled Backups:**  
  - pg_dump รายวัน/รายสัปดาห์  
  - สคริปต์แจ้งเตือนเมื่อสำรองล้มเหลว  
- **Data Retention Policy:** กำหนดเวอร์ชันและระยะเวลาการเก็บ  

---

### 15. Backend: โครงสร้างฐานข้อมูล (Supabase – PostgreSQL)
- **roles** (id, name, permissions JSONB)  
- **profiles** (id UUID → auth.users, display_name, avatar_url, role_id)  
- **categories** (id, name, slug)  
- **novels** (id, title, slug, synopsis, cover_image_url, author_id, category_id, original_language, visibility, default_chapter_status, default_chapter_access, total_chapters, published_chapters, status, timestamps)  
- **chapters** (id, novel_id, chapter_number, title, slug, content_original, status, access_level, created_by, word_count, read_by_users JSONB, timestamps)  
- **bookmarks** (id, user_id, chapter_id, created_at)  
- **mark_lines** (id, chapter_id, translator_id, marked_lines_data JSONB, timestamps)  
- **chapter_versions**, **activity_logs**  
- **Triggers:** `trigger_set_timestamp` on update  
- **Indexes:** ตามคีย์หลักและ FK  
- **Initial Roles:** ‘member’, ‘translator’, ‘admin’  
- **RLS Functions:** `get_user_role(user_id)`  

---

### 16. Backend: Supabase Storage Buckets
- **novel-covers:**  
  - Public read, translators/admins write  
  - image/jpeg, png, webp (≤5 MB)  
- **chapter-text-files:**  
  - translators/admins upload  
  - text/plain (≤2 MB)  
  - ลบอัตโนมัติหลังประมวลผล  
- **avatars:**  
  - Public read, user upload/update/delete เฉพาะไฟล์ตัวเอง  
  - image/jpeg, png (≤2 MB)  

---

### 17. การนำไปใช้งานและการติดตามผล (Deployment & Monitoring)
- **CI/CD (GitHub Actions → Vercel):**  
  - ติดตั้ง, build, test, deploy ทุก push ไป `main`/`staging`  
- **Environment Variables:**  
  - `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`, `SUPABASE_SERVICE_ROLE_KEY`  
- **Deployment Checklist:**  
  - รัน migrations, ตั้ง RLS + Storage Policies, ตั้ง Backup Policy  
  - ทดสอบ User Flows, A11y, Responsive, Logging  
- **Monitoring:**  
  - Sentry (Frontend/Backend errors)  
  - Supabase Dashboard (DB logs, usage)  
  - Vercel Dashboard (Build/deploy logs, analytics)
