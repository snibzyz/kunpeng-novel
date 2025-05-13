# INKREALM.CO - README ฉบับสมบูรณ์ (V4.16)

เอกสารนี้เป็นคู่มือฉบับสมบูรณ์สำหรับโปรเจกต์ INKREALM.CO แพลตฟอร์มนิยายออนไลน์ที่ออกแบบมาเพื่อมอบประสบการณ์การอ่านที่หรูหราและเครื่องมือที่ทรงพลังสำหรับนักแปลโดยเฉพาะ

## สารบัญ

1.  [ปรัชญาการออกแบบโดยรวม](#1-ปรัชญาการออกแบบโดยรวม)
2.  [Tech Stack (ชุดเทคโนโลยีที่ใช้)](#2-tech-stack-ชุดเทคโนโลยีที่ใช้)
3.  [คุณสมบัติหลัก (Key Features)](#3-คุณสมบัติหลัก-key-features)
4.  [โทนสีและสไตล์ (Color Palette & Style)](#4-โทนสีและสไตล์-color-palette--style)
5.  [โครงสร้างโฟลเดอร์โปรเจกต์ (Project Folder Structure)](#5-โครงสร้างโฟลเดอร์โปรเจกต์-project-folder-structure)
6.  [บทบาทผู้ใช้และสิทธิ์ (User Roles & Permissions)](#6-บทบาทผู้ใช้และสิทธิ์-user-roles--permissions)
7.  [ประสบการณ์ผู้ใช้งาน (Leading UX & User Flows)](#7-ประสบการณ์ผู้ใช้งาน-leading-ux--user-flows)
8.  [ระบบความปลอดภัย (Security)](#8-ระบบความปลอดภัย-security)
9.  [การทดสอบและประกันคุณภาพ (Testing & QA)](#9-การทดสอบและประกันคุณภาพ-testing--qa)
10. [การปรับปรุงประสบการณ์ผู้ใช้ (UX Enhancements)](#10-การปรับปรุงประสบการณ์ผู้ใช้-ux-enhancements---responsive-focus)
11. [การจัดการข้อมูลและการสำรองข้อมูล (Data Management & Backup)](#11-การจัดการข้อมูลและการสำรองข้อมูล-data-management--backup)
12. [โครงสร้างฐานข้อมูล (Database Structure)](#12-โครงสร้างฐานข้อมูล-supabase---postgresql)
    * [SQL Schema สำหรับสร้างตาราง](#sql-schema-สำหรับสร้างตาราง)
    * [SQL สำหรับสร้าง View `admin_markline_overview`](#sql-สำหรับสร้าง-view-admin_markline_overview)
13. [Supabase Storage Buckets](#13-supabase-storage-buckets)
14. [การนำไปใช้งานและการติดตามผล (Deployment & Monitoring)](#14-การนำไปใช้งานและการติดตามผล-deployment--monitoring)
15. [การตั้งค่าเริ่มต้นสำหรับการพัฒนา (Getting Started - Local Development)](#15-การตั้งค่าเริ่มต้นสำหรับการพัฒนา-getting-started---local-development)

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

* **Frontend:** Next.js + TailwindCSS + DaisyUI + TypeScript
    * **DaisyUI:** ใช้ร่วมกับ TailwindCSS เพื่อเป็น Component Library ช่วยให้สร้าง UI ที่สวยงาม ใช้งานง่าย และสอดคล้องกันได้รวดเร็วขึ้น
* **Backend:** Supabase (PostgreSQL + Auth + Storage)
* **Deployment:** Vercel
* **CI/CD:** GitHub Actions
* **Monitoring:** Sentry + Grafana (หรือ Supabase Dashboard สำหรับการติดตามผลเบื้องต้น)
* **Form Handling & Validation:** React Hook Form + Zod
* **Rich Text Editor:** TipTap หรือ Editor.js (สำหรับ Modal เพิ่ม/แก้ไขตอน)

---

## 3. คุณสมบัติหลัก (Key Features)

* **ระบบสมาชิกและการยืนยันตัวตน:** ลงทะเบียน, เข้าสู่ระบบ, ลืมรหัสผ่าน, จัดการโปรไฟล์
* **การจัดการนิยาย:**
    * สร้าง, แก้ไข, ลบนิยาย
    * อัปโหลดปกนิยาย
    * กำหนดหมวดหมู่, ภาษาต้นฉบับ, สิทธิ์การมองเห็น
* **การจัดการตอน:**
    * เพิ่มตอนด้วย Rich Text Editor หรืออัปโหลดไฟล์ .txt (Batch Upload)
    * แก้ไข, ลบตอน
    * กำหนดสถานะ (เผยแพร่, ฉบับร่าง) และสิทธิ์การเข้าถึง
* **ระบบอ่านนิยาย:**
    * UI ที่สะอาดตา อ่านง่าย ปรับแต่งการแสดงผลได้ (ขนาดอักษร, ระยะห่างบรรทัด, สีพื้นหลัง)
    * แถบความคืบหน้าการอ่าน
    * ระบบบุ๊คมาร์คตอน
    * การนำทางระหว่างตอน (ก่อนหน้า/ถัดไป)
* **โหมดแปลสำหรับนักแปล:**
    * แสดงหมายเลขบรรทัดคู่กับเนื้อหาต้นฉบับ
    * ฟังก์ชัน "บรรทัดมาร์ค" (Mark Line) สำหรับบันทึกบรรทัดที่สนใจ
    * คัดลอกข้อความบรรทัดที่มาร์ค หรือข้อความทั้งหมดที่มาร์ค
    * ล้างมาร์คทั้งหมด
* **แดชบอร์ด:**
    * แดชบอร์ดนักแปล: จัดการนิยายที่สร้าง, ดูสถิติส่วนตัว
    * แดชบอร์ดผู้ดูแลระบบ: จัดการผู้ใช้, นิยาย, ตอน, ดูสถิติเว็บไซต์, ตั้งค่าระบบ, Log การทำงาน
* **การออกแบบที่ตอบสนองทุกอุปกรณ์ (Responsive Design):** ใช้งานได้ดีบน Desktop, Tablet, และ Mobile
* **ธีมสว่าง/มืด (Light/Dark Mode):** ผู้ใช้สามารถสลับธีมและระบบจะจดจำการตั้งค่า
* **ระบบ Breadcrumbs:** ช่วยให้ผู้ใช้ทราบตำแหน่งปัจจุบันในเว็บไซต์
* **การจัดการเวอร์ชัน (เบื้องต้น):** ผู้ดูแลระบบสามารถดูและย้อนกลับเวอร์ชันของตอนได้
* **ระบบ Log การทำงาน:** บันทึกกิจกรรมสำคัญในระบบสำหรับผู้ดูแลระบบ

---

## 4. โทนสีและสไตล์ (Color Palette & Style)

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
    * ไอคอนสถานะการอ่าน: ไอคอนรูปตาแบบมี Animation
    * ไอคอนสถานะตอน: เผยแพร่ (ลูกโลก), ฉบับร่าง (เอกสาร)
    * ไอคอนปรับแต่งการแสดงผล: "Aa" หรือรูปเฟือง
    * ไอคอนบุ๊คมาร์ค: ที่คั่นหนังสือ
* **ปุ่มสลับธีม (Theme Toggle):** DaisyUI Theme Controller (Toggle พร้อมไอคอนพระอาทิตย์/พระจันทร์)
* **Breadcrumbs Styling:** ใช้ DaisyUI Breadcrumbs component, สไตล์ให้เข้ากับธีมสีทอง-เทา

---

## 5. โครงสร้างโฟลเดอร์โปรเจกต์ (Project Folder Structure)

โครงสร้างนี้เป็นแนวทางสำหรับโปรเจกต์ Next.js ที่ใช้ TypeScript และ Supabase:

inkrealm-project/├── .github/                    # GitHub Actions Workflows│   └── workflows/│       └── deploy.yml          # CI/CD Workflow├── .vscode/                    # VSCode settings (optional)│   └── settings.json├── public/                     # Static assets│   ├── fonts/                  # (ถ้ามีฟอนต์ที่ host เอง)│   └── images/                 # รูปภาพ static อื่นๆ├── src/│   ├── app/                    # Next.js App Router│   │   ├── (auth)/             # Route group สำหรับหน้า Authentication│   │   │   ├── login/│   │   │   │   └── page.tsx│   │   │   ├── register/│   │   │   │   └── page.tsx│   │   │   └── forgot-password/│   │   │       └── page.tsx│   │   ├── (main)/             # Route group สำหรับหน้าหลักที่ต้องมี Layout (Navbar, Footer)│   │   │   ├── layout.tsx      # Layout หลัก (Navbar, Footer)│   │   │   ├── page.tsx        # หน้าแรก (Home page)│   │   │   ├── bookmarks/│   │   │   │   └── page.tsx│   │   │   ├── novels/│   │   │   │   ├── [novel_slug]/│   │   │   │   │   ├── page.tsx            # หน้าข้อมูลนิยายและสารบัญ│   │   │   │   │   └── (reader)/│   │   │   │   │       ├── [chapter_slug]/│   │   │   │   │       │   └── page.tsx    # หน้าอ่านนิยาย (สมาชิก)│   │   │   ├── translate/│   │   │   │   ├── [novel_slug]/│   │   │   │   │   ├── [chapter_slug]/│   │   │   │   │   │   └── page.tsx    # หน้าโหมดแปล (นักแปล)│   │   │   ├── profile/│   │   │   │   └── page.tsx│   │   │   ├── translator-dashboard/│   │   │   │   └── page.tsx│   │   │   └── admin-dashboard/│   │   │       ├── page.tsx│   │   │       ├── users/│   │   │       │   └── page.tsx│   │   │       ├── versions/│   │   │       │   └── page.tsx│   │   │       └── settings/│   │   │           └── page.tsx│   │   ├── api/                  # API Routes (Next.js Route Handlers)│   │   │   └── auth/│   │   │       └── callback/│   │   │           └── route.ts  # Supabase Auth callback│   │   ├── layout.tsx          # Root layout│   │   └── global.css          # Global styles (Tailwind directives)│   ├── components/             # React Components (UI)│   │   ├── auth/               # Components สำหรับ Auth pages│   │   ├── common/             # Components ที่ใช้ซ้ำทั่วไป (Button, Modal, Card, etc.)│   │   ├── layout/             # Navbar, Footer, Sidebar components│   │   ├── novel/              # Components เกี่ยวกับนิยาย (NovelCard, ChapterList)│   │   ├── reader/             # Components สำหรับหน้าอ่าน (ReadingToolbar, DisplaySettings)│   │   ├── translator/         # Components สำหรับโหมดแปล (MarkLine, TranslatorToolbar)│   │   └── admin/              # Components สำหรับ Admin Dashboard│   ├── contexts/               # React Contexts (e.g., ThemeContext, AuthContext)│   ├── hooks/                  # Custom React Hooks│   ├── lib/                    # Helper functions, Supabase client, utilities│   │   ├── supabase/│   │   │   ├── client.ts       # Supabase client instance (for client-side)│   │   │   ├── server.ts       # Supabase client instance (for server-side/Route Handlers)│   │   │   └── admin.ts        # Supabase admin client (for privileged operations)│   │   ├── zod-schemas.ts    # Zod schemas for validation│   │   └── utils.ts            # Utility functions│   ├── services/               # API call functions, data fetching logic│   ├── store/                  # State management (e.g., Zustand, Redux) - (optional)│   ├── styles/                 # Additional global styles or theme configurations│   ├── types/                  # TypeScript type definitions│   │   ├── database.types.ts   # Auto-generated types from Supabase schema│   │   └── index.ts            # Custom types│   └── middleware.ts           # Next.js Middleware (e.g., for auth protection)├── .env.local                  # Local environment variables (ignored by Git)├── .env.example                # Example environment variables├── .eslintrc.json              # ESLint configuration├── .gitignore                  # Git ignore file├── next.config.mjs             # Next.js configuration├── package.json├── postcss.config.js           # PostCSS configuration (for TailwindCSS)├── tailwind.config.ts          # TailwindCSS configuration└── tsconfig.json               # TypeScript configuration
---

## 6. บทบาทผู้ใช้และสิทธิ์ (User Roles & Permissions)

* **สมาชิก (Member):**
    * อ่านเนื้อหาสาธารณะและตอนที่เผยแพร่
    * บุ๊คมาร์คตอน
    * ปรับแต่งการแสดงผลการอ่าน
* **นักแปล (Translator):**
    * สิทธิ์ทั้งหมดของสมาชิก
    * เข้าถึง "โหมดแปล"
    * ใช้งาน "บรรทัดมาร์ค"
    * สร้างและจัดการนิยาย/ตอนของตนเอง
    * เข้าถึง "แดชบอร์ดนักแปล"
* **ผู้ดูแลระบบ (Admin):**
    * สิทธิ์ทั้งหมดของนักแปล
    * จัดการผู้ใช้ทั้งหมด (เลื่อนขั้น, ลดขั้น, ระงับ, ลบ)
    * จัดการนิยายและตอนทั้งหมด
    * ดูบรรทัดมาร์คของนักแปลคนอื่น
    * จัดการเวอร์ชันเนื้อหา
    * เข้าถึง "แดชบอร์ดผู้ดูแลระบบ" และตั้งค่าเว็บไซต์

การจัดการสิทธิ์จะใช้ Row-Level Security (RLS) ใน Supabase ร่วมกับ Logic ใน Frontend และ Backend API

---

## 7. ประสบการณ์ผู้ใช้งาน (Leading UX & User Flows)

(อ้างอิงรายละเอียดในเอกสารสเปค V4.16 หัวข้อ 8 ของเอกสารต้นฉบับ)

* **การเข้าสู่ระบบ และหน้าแรก:** UI ภาษาไทย, คลิกปกนิยายไปหน้าข้อมูลนิยาย
* **การเข้าดูนิยายและสารบัญ:** UI ภาษาไทย, สารบัญแบบ Collapse, บันทึกสถานะการอ่าน
* **การใช้งานบุ๊คมาร์ค:** UI ภาษาไทย
* **ขั้นตอนการแปลและการจัดการเส้นมาร์ค:**
    * สร้างตอนใหม่ -> ระบบสร้าง Record `mark_lines` อัตโนมัติ
    * เข้าโหมดแปล -> ถ้าไม่มี Record `mark_lines` จะสร้างให้
    * คลิกมาร์ค/ยกเลิกมาร์ค -> อัปเดต `marked_lines_data`
    * ล้างมาร์ค -> ตั้งค่า `marked_lines_data` เป็น `[]`
    * คัดลอกมาร์คทั้งหมด -> ดึง `marked_lines_data` และ `content_original` มาประมวลผล
    * ดับเบิลคลิก -> คัดลอกข้อความบรรทัดนั้น
* **การวางข้อมูลบรรทัดมาร์คเก่า (สำหรับ Admin):** แก้ไข Field `marked_lines_data` ในตาราง `mark_lines` โดยตรงผ่าน Supabase Studio หรือ Admin Interface

---

## 8. ระบบความปลอดภัย (Security)

* **Authentication:** JWT Auth พร้อม Refresh Token (Supabase Auth)
* **Transport Security:** HTTPS
* **API Security:**
    * Rate Limiting
    * ป้องกัน XSS (Sanitize input, CSP)
    * ป้องกัน CSRF (Anti-CSRF tokens)
    * ตรวจสอบสิทธิ์ (RLS, Middleware) ก่อนเรียก API
* **Data Security:**
    * Row-Level Security (RLS) ใน Supabase
* **Account Security:**
    * ยืนยันอีเมล
    * Login attempt throttling

---

## 9. การทดสอบและประกันคุณภาพ (Testing & QA)

* **Unit Test:** Jest หรือ Vitest + React Testing Library (RTL)
* **End-to-End (E2E) Test:** Cypress
* **Load Test:** k6 หรือ JMeter
* **Accessibility (A11y) Test:** axe-core, Lighthouse

---

## 10. การปรับปรุงประสบการณ์ผู้ใช้ (UX Enhancements - Responsive Focus)

* **Empty States:** แสดงสถานะว่างที่สวยงามพร้อม Call-to-action
* **Loading States:** Skeleton Loaders, Spinners/Progress Bars
* **Notifications/Toasts:** แจ้งผลการกระทำ (DaisyUI Alert/Toast)
* **Error Handling & Feedback:** ข้อความ Error ที่เป็นมิตร, หน้า 404/500 ที่ออกแบบเฉพาะ
* **Accessibility (A11y):** Contrast ratio, Keyboard navigation, ARIA attributes, Alt text
* **Transitions & Micro-interactions:** การเปลี่ยนหน้าที่นุ่มนวล, Hover/press effects

---

## 11. การจัดการข้อมูลและการสำรองข้อมูล (Data Management & Backup)

* **Supabase Built-in Backups:** ตรวจสอบนโยบาย PITR และ Retention Policy ของ Supabase Plan ที่เลือก
* **Supabase Storage Backups:** พิจารณาแผนสำรองข้อมูลแยกสำหรับไฟล์ใน Storage หรือทำสำเนาไปยัง Storage อื่น (AWS S3, Google Cloud Storage)
* **Manual/Scheduled Backups (Recommended):**
    * **Database Dumps:** ทำ `pg_dump` เป็นระยะ (รายวัน/รายสัปดาห์) เก็บไว้ในที่ปลอดภัย
    * **Scripting:** สร้าง Script สำหรับ Backup อัตโนมัติและแจ้งเตือน
* **Testing Restore Procedures:** ทดสอบการกู้คืนข้อมูลจาก Backup เป็นประจำ
* **Data Retention Policy:** กำหนดนโยบายการเก็บรักษาข้อมูลสำรองให้ชัดเจน

---

## 12. โครงสร้างฐานข้อมูล (Supabase - PostgreSQL)

ชื่อตารางและคอลัมน์เป็นภาษาอังกฤษ

### SQL Schema สำหรับสร้างตาราง

```sql
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
    avatar_url TEXT,
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
    translator_id UUID REFERENCES profiles(id) ON DELETE CASCADE NOT NULL,
    marked_lines_data JSONB DEFAULT '[]'::jsonb, -- Array ของหมายเลขบรรทัด เช่น [21, 40, 39]
    created_at TIMESTAMPTZ DEFAULT now(),
    updated_at TIMESTAMPTZ DEFAULT now(),
    UNIQUE (chapter_id, translator_id)
);

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
    action_type TEXT NOT NULL, -- e.g., 'CREATE_NOVEL', 'UPDATE_USER_ROLE'
    target_type TEXT, -- e.g., 'novel', 'user', 'chapter'
    target_id TEXT, -- ID ของสิ่งที่ถูกกระทำ (อาจเป็น INT หรือ UUID จึงใช้ TEXT)
    details JSONB, -- รายละเอียดเพิ่มเติมของการกระทำ
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

SQL สำหรับสร้าง View admin_markline_overviewCREATE VIEW admin_markline_overview AS
SELECT
    ml.id AS markline_id,
    p.display_name AS translator_name, -- ชื่อที่แสดงผลของนักแปล
    p.id AS translator_uuid, -- ID ของนักแปล
    n.title AS novel_title, -- ชื่อนิยาย
    n.id AS novel_id, -- ID นิยาย
    c.title AS chapter_title, -- ชื่อตอน
    c.chapter_number, -- เลขที่ตอน
    c.id AS chapter_id, -- ID ตอน
    ml.marked_lines_data, -- Array ของหมายเลขบรรทัดที่มาร์คไว้ (JSONB)
    jsonb_array_length(
        CASE
            WHEN ml.marked_lines_data IS NULL THEN '[]'::jsonb
            ELSE ml.marked_lines_data
        END
    ) AS total_marks_in_chapter, -- จำนวนบรรทัดที่มาร์คในตอนนี้
    ml.updated_at AS markline_last_updated
FROM
    mark_lines ml
JOIN
    profiles p ON ml.translator_id = p.id
JOIN
    chapters c ON ml.chapter_id = c.id
JOIN
    novels n ON c.novel_id = n.id
ORDER BY
    p.display_name, n.title, c.chapter_number;
หมายเหตุเกี่ยวกับ Database:profiles.id ควรเชื่อมโยงกับ auth.users.id ของ Supabase โดยอัตโนมัติเมื่อผู้ใช้สมัครสมาชิกการใช้ ON DELETE CASCADE และ ON DELETE SET NULL ถูกกำหนดตามความเหมาะสมของความสัมพันธ์พิจารณาเพิ่ม Indexes เพิ่มเติมตามความจำเป็นเพื่อประสิทธิภาพในการ QueryRLS (Row-Level Security) Policies จะต้องถูกกำหนดใน Supabase Dashboard เพื่อควบคุมการเข้าถึงข้อมูลตามบทบาทผู้ใช้13. Supabase Storage Bucketsจำเป็นต้องสร้าง Storage Buckets ต่อไปนี้ใน Supabase Project ของคุณ:novel-covers:วัตถุประสงค์: สำหรับเก็บไฟล์รูปปกนิยายสิทธิ์การเข้าถึง (Policies):ผู้ใช้ทั่วไป (Public/Anon): สามารถอ่าน (Read) ได้ผู้ใช้ที่ยืนยันตัวตนแล้ว (Authenticated Users) ที่มีบทบาท 'translator' หรือ 'admin': สามารถอัปโหลด (Create), แก้ไข (Update), ลบ (Delete) ได้ (ควรจำกัดตาม author_id ของนิยาย หรือผ่าน Functions เพื่อความปลอดภัย)ประเภทไฟล์ที่อนุญาต: image/jpeg, image/png, image/webpขนาดไฟล์สูงสุด: (ตามที่กำหนด เช่น 5MB)chapter-text-files:วัตถุประสงค์: สำหรับเก็บไฟล์ .txt ที่นักแปลอัปโหลดเพื่อสร้างตอนแบบ Batchสิทธิ์การเข้าถึง (Policies):ผู้ใช้ที่ยืนยันตัวตนแล้ว (Authenticated Users) ที่มีบทบาท 'translator' หรือ 'admin': สามารถอัปโหลด (Create) ได้ควรมีการลบไฟล์อัตโนมัติหลังจากประมวลผลเสร็จสิ้น หรือตั้งค่าให้เข้าถึงได้เฉพาะ Backend Functionsประเภทไฟล์ที่อนุญาต: text/plainขนาดไฟล์สูงสุด: (ตามที่กำหนด เช่น 2MB ต่อไฟล์)avatars: (ถ้าต้องการให้ผู้ใช้มี avatar ที่อัปโหลดเอง นอกเหนือจาก URL)วัตถุประสงค์: สำหรับเก็บไฟล์รูปโปรไฟล์ผู้ใช้สิทธิ์การเข้าถึง (Policies):ผู้ใช้ทั่วไป (Public/Anon): สามารถอ่าน (Read) ได้ผู้ใช้ที่ยืนยันตัวตนแล้ว (Authenticated Users): สามารถอัปโหลด (Create), แก้ไข (Update), ลบ (Delete) รูปของตนเองได้ประเภทไฟล์ที่อนุญาต: image/jpeg, image/pngขนาดไฟล์สูงสุด: (ตามที่กำหนด เช่น 2MB)14. การนำไปใช้งานและการติดตามผล (Deployment & Monitoring)DevOps (CI/CD):GitHub Actions:Workflow ใน .github/workflows/deploy.yml จะถูก trigger เมื่อมีการ push code ไปยัง branch ที่กำหนด (เช่น main สำหรับ Production, staging สำหรับ Staging)ขั้นตอนใน Workflow:Checkout codeSet up Node.jsInstall dependencies (npm ci หรือ pnpm install --frozen-lockfile)Build Next.js application (npm run build หรือ pnpm build)(Optional) Run tests (npm test หรือ pnpm test)Deploy to Vercel (ใช้ Vercel CLI หรือ GitHub Action ของ Vercel)Environments:Vercel Environment Variables:Production Environment: ตั้งค่า Environment Variables ใน Vercel Dashboard สำหรับ Production deployment (เชื่อมกับ main branch)NEXT_PUBLIC_SUPABASE_URLNEXT_PUBLIC_SUPABASE_ANON_KEYSUPABASE_SERVICE_ROLE_KEY (ถ้า Backend API Routes หรือ Server Components จำเป็นต้องใช้)Preview/Staging Environment: ตั้งค่า Environment Variables สำหรับ Preview deployments (เชื่อมกับ Pull Requests หรือ staging branch) อาจใช้ Supabase project แยกสำหรับ Staging หรือใช้ RLS/data separation strategiesDeployment Checklist (ก่อน Production ครั้งแรก):[ ] Supabase Project Setup:[ ] สร้าง Supabase Project สำหรับ Production[ ] ตั้งค่า Authentication (Email provider, Third-party logins ถ้ามี)[ ] รัน Database schema migrations ทั้งหมด (SQL จากข้อ 12)[ ] ตั้งค่า Row-Level Security (RLS) Policies ให้ครบถ้วนและถูกต้อง[ ] สร้าง Storage Buckets (ตามข้อ 13) พร้อม Policies ที่ถูกต้อง[ ] ตั้งค่านโยบาย Backup ของ Database[ ] Vercel Project Setup:[ ] สร้าง Project ใหม่บน Vercel และเชื่อมต่อกับ GitHub Repository[ ] กำหนด Production Branch (เช่น main)[ ] ตั้งค่า Environment Variables ทั้งหมดสำหรับ Production (Supabase URL, Keys, etc.)[ ] (Optional) ตั้งค่า Custom Domain[ ] Codebase & Application:[ ] ตรวจสอบว่า Environment Variables ถูกเรียกใช้ถูกต้อง (NEXT_PUBLIC_ สำหรับ client-side)[ ] ทดสอบ User Flows ทั้งหมดใน Staging/Preview environment (ถ้ามี)[ ] ตรวจสอบ Error Handling และ Logging[ ] ตรวจสอบ Responsive Design บนอุปกรณ์ต่างๆ[ ] ทำ A11y checks เบื้องต้น[ ] ลบ console.log ที่ไม่จำเป็น[ ] CI/CD Pipeline:[ ] ตรวจสอบว่า GitHub Actions workflow ทำงานถูกต้อง (Build, Test, Deploy)[ ] Monitoring Setup:[ ] เชื่อมต่อ Sentry (หรือเครื่องมืออื่น) สำหรับ Error Tracking[ ] ตั้งค่าการแจ้งเตือนสำหรับ Critical Errors[ ] (Optional) ตั้งค่า Grafana หรือตรวจสอบ Supabase Dashboard สำหรับ Performance Monitoringการ Deploy:เมื่อทุกอย่างพร้อม การ push code ไปยัง Production branch (เช่น main) จะ trigger CI/CD pipeline และ deploy ไปยัง Vercel โดยอัตโนมัติMonitoring หลัง Deploy:Sentry: ติดตาม Real-time errors และ exceptionsSupabase Dashboard:Logs: ตรวจสอบ API logs, Database query logs, Auth eventsUsage: ดูการใช้งาน Database, Storage, AuthReports: ดู Performance metrics ของ DatabaseVercel Dashboard: ดู Build logs, Deployment status, Analytics (ถ้ามี)(Optional) Grafana: ติดตาม Metrics ที่ตั้งค่าไว้15. การตั้งค่าเริ่มต้นสำหรับการพัฒนา (Getting Started - Local Development)ส่วนนี้จะอธิบายขั้นตอนการตั้งค่าโปรเจกต์เพื่อเริ่มพัฒนาบนเครื่อง localสิ่งที่ต้องมี (Prerequisites):Node.js และ npm/yarn/pnpm: (แนะนำ LTS version ล่าสุด)Git: สำหรับ Version ControlSupabase Account: สร้างโปรเจกต์ใหม่บน SupabaseSupabase CLI (Optional but Recommended): สำหรับจัดการ Database Migrations และ Local Development Supabase CLIIDE/Text Editor: เช่น VSCodeขั้นตอนการตั้งค่า:Clone Repository:git clone <your-repository-url>
cd inkrealm-project
Install Dependencies:npm install
# หรือ yarn install
# หรือ pnpm install
ตั้งค่า Environment Variables:คัดลอกไฟล์ .env.example ไปเป็น .env.local:cp .env.example .env.local
เปิดไฟล์ .env.local และกรอกค่าจาก Supabase Project ของคุณ:NEXT_PUBLIC_SUPABASE_URL: ไปที่ Project Settings > API > Project URL ใน Supabase DashboardNEXT_PUBLIC_SUPABASE_ANON_KEY: ไปที่ Project Settings > API > Project API Keys > anon public ใน Supabase DashboardSUPABASE_SERVICE_ROLE_KEY: (ถ้าจำเป็นสำหรับ Server-side operations หรือ Seed script) ไปที่ Project Settings > API > Project API Keys > service_role secret (ข้อควรระวัง: อย่า commit key นี้เข้า Git หากใช้ใน client-side code โดยตรง)DATABASE_URL: (ถ้าใช้ Supabase CLI สำหรับ local development หรือ migrations) ไปที่ Project Settings > Database > Connection string > URI(Optional) Supabase Local Development Setup (ใช้ Supabase CLI):Login Supabase CLI:supabase login
Link Project (ถ้ายังไม่ได้ทำ):supabase link --project-ref <your-project-id>
(<your-project-id> หาได้จาก URL ของ Supabase project เช่น https://app.supabase.com/project/<your-project-id>)Pull Database Schema (ถ้ามี schema อยู่แล้วบน remote):supabase db pull
(จะสร้างไฟล์ schema ใน supabase/migrations)Start Local Supabase Services:supabase start
(จะให้ URL และ Keys สำหรับ local instance)Run Database Migrations/Seed Data:SQL Schema: นำโค้ด SQL จาก SQL Schema สำหรับสร้างตาราง และ SQL สำหรับสร้าง View ไปรันใน SQL Editor ของ Supabase Dashboard (Database > SQL Editor) หรือผ่าน Supabase CLI migrationsSeed Data (ถ้ามี): สร้างไฟล์ seed script (เช่น supabase/seed.sql) และรันผ่าน Supabase CLI หรือ SQL EditorGenerate Supabase Types (แนะนำ):เพื่อให้ TypeScript รู้จักโครงสร้าง Database ของคุณnpx supabase gen types typescript --project-id <your-project-id> --schema public > src/types/database.types.ts
# หรือถ้าใช้ local dev กับ Supabase CLI
# npx supabase gen types typescript --local > src/types/database.types.ts
(อาจต้อง login Supabase CLI ก่อน)Run Development Server:npm run dev
# หรือ yarn dev
