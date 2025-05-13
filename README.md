# 📘 INKREALM.CO - เอกสารฉบับสมบูรณ์ (README)

## 🛠️ จุดเริ่มต้นการใช้งาน (Quick Start Checklist)

### ✅ 1. เตรียมพร้อมระบบพื้นฐาน

* [x] สมัคร Supabase Project (พร้อม Auth + Storage)
* [x] สร้าง Vercel Project แล้วเชื่อมกับ GitHub Repo
* [x] เพิ่ม `.env.local` บนเครื่อง dev:

  ```env
  NEXT_PUBLIC_SUPABASE_URL=https://xxxxx.supabase.co
  NEXT_PUBLIC_SUPABASE_ANON_KEY=xxxxx
  SUPABASE_SERVICE_ROLE_KEY=xxxxx
  ```
* [x] ติดตั้ง Dependencies ด้วยคำสั่ง:

  ```bash
  pnpm install
  # หรือ npm install / yarn install
  ```

### ✅ 2. สร้าง Database + Bucket ใน Supabase

#### 🗃️ ตาราง (PostgreSQL)

ใช้ SQL ด้านล่างเพื่อสร้างฐานข้อมูล:

```sql
-- roles
CREATE TABLE roles (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  permissions JSONB
);

-- profiles
CREATE TABLE profiles (
  id UUID PRIMARY KEY,
  display_name TEXT,
  avatar_url TEXT,
  role_id INT REFERENCES roles(id),
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now()
);

-- categories
CREATE TABLE categories (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  slug TEXT UNIQUE,
  created_at TIMESTAMPTZ DEFAULT now()
);

-- novels
CREATE TABLE novels (
  id SERIAL PRIMARY KEY,
  title TEXT,
  slug TEXT UNIQUE,
  synopsis TEXT,
  cover_image_url TEXT,
  author_id UUID REFERENCES profiles(id),
  category_id INT REFERENCES categories(id),
  original_language TEXT,
  visibility TEXT,
  default_chapter_status TEXT,
  default_chapter_access TEXT[],
  total_chapters INT DEFAULT 0,
  published_chapters INT DEFAULT 0,
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now(),
  last_published_at TIMESTAMPTZ
);

-- chapters
CREATE TABLE chapters (
  id SERIAL PRIMARY KEY,
  novel_id INT REFERENCES novels(id),
  chapter_number INT,
  title TEXT,
  slug TEXT,
  content_original TEXT,
  status TEXT,
  access_level TEXT[],
  created_by UUID REFERENCES profiles(id),
  word_count INT,
  read_by_users JSONB DEFAULT '{}',
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now(),
  published_at TIMESTAMPTZ
);

-- bookmarks
CREATE TABLE bookmarks (
  id SERIAL PRIMARY KEY,
  user_id UUID REFERENCES profiles(id),
  chapter_id INT REFERENCES chapters(id),
  created_at TIMESTAMPTZ DEFAULT now()
);

-- mark_lines
CREATE TABLE mark_lines (
  id SERIAL PRIMARY KEY,
  chapter_id INT REFERENCES chapters(id) NOT NULL,
  translator_id UUID REFERENCES profiles(id) NOT NULL,
  marked_lines_data JSONB DEFAULT '[]',
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now(),
  UNIQUE (chapter_id, translator_id)
);

-- chapter_versions (optional)
CREATE TABLE chapter_versions (
  id SERIAL PRIMARY KEY,
  chapter_id INT REFERENCES chapters(id),
  version_number INT,
  title TEXT,
  content_original TEXT,
  changed_by UUID REFERENCES profiles(id),
  created_at TIMESTAMPTZ DEFAULT now()
);

-- activity_logs
CREATE TABLE activity_logs (
  id SERIAL PRIMARY KEY,
  user_id UUID REFERENCES profiles(id),
  action_type TEXT NOT NULL,
  target_type TEXT,
  target_id TEXT,
  details JSONB,
  created_at TIMESTAMPTZ DEFAULT now()
);
```

#### 📂 Bucket (สำหรับเก็บไฟล์)

ใน Supabase Storage สร้าง bucket ตามนี้:

* `novel-covers`: สำหรับเก็บรูปปกนิยาย
* `chapter-files`: สำหรับเก็บไฟล์ .txt ที่อัปโหลด
* ตั้งสิทธิ์ให้ Public เฉพาะ `novel-covers` และใช้ RLS กับ `chapter-files`

---

## 📁 โครงสร้างโปรเจกต์ (Recommended Folder Structure)

```
/inkrealm-app
├── app/
│   ├── page.tsx
│   ├── layout.tsx
│   ├── novels/
│   │   ├── [slug]/
│   │   │   ├── page.tsx (Novel Detail Page)
│   │   │   └── [chapter]/
│   │   │       ├── page.tsx (Reading Mode)
│   │   │       └── translate/page.tsx (Translator Mode)
│   ├── bookmarks/
│   ├── dashboard/
│   │   ├── translator/
│   │   └── admin/
│   ├── profile/
│   └── auth/ (login, register, reset password)
├── components/
│   ├── Navbar.tsx
│   ├── ChapterCard.tsx
│   ├── ReaderToolbar.tsx
│   └── Modals/
├── lib/
│   ├── supabase.ts
│   ├── utils.ts
│   └── auth.ts
├── styles/
│   └── globals.css (Tailwind + DaisyUI)
├── public/
├── .env.local
├── tailwind.config.ts
├── tsconfig.json
└── README.md
```

---

## 🧑‍💻 เทคโนโลยีที่ใช้ (Tech Stack)

* **Next.js 14** + **App Router**
* **TailwindCSS** + **DaisyUI** (UI Library)
* **TypeScript**
* **Supabase** (PostgreSQL + Auth + Storage)
* **React Hook Form** + **Zod** (Form validation)
* **Vercel** (Deployment)
* **TipTap / Editor.js** (Rich text editor)
* **Sentry** (Error monitoring)
* **GitHub Actions** (CI/CD)

---

## 🚀 การ Deploy & Monitor (Deployment & Monitoring)

### 1. การ Deploy

* Push ไปยัง GitHub branch `main` (หรือ `staging`)
* GitHub Actions จะ Build และ Deploy ขึ้น Vercel อัตโนมัติ
* ตั้งค่าตัวแปร `.env` ใน Vercel > Settings > Environment Variables:

  * `NEXT_PUBLIC_SUPABASE_URL`
  * `NEXT_PUBLIC_SUPABASE_ANON_KEY`
  * `SUPABASE_SERVICE_ROLE_KEY`

### 2. การ Monitoring

* ใช้ Sentry SDK ใน client สำหรับการเก็บ error (เช่น API fail, render error)
* Dashboard Supabase ใช้ตรวจสอบการ Query, Log, และ Events ต่างๆ
* (Optional) ติดตั้ง Grafana + Prometheus ถ้าต้องการ monitor DB เพิ่มเติม

---

## 🎯 เป้าหมายหลักของ INKREALM

* ให้ประสบการณ์ที่ดีที่สุดสำหรับนักแปล, ผู้อ่าน, และแอดมิน
* รองรับนิยายหลายภาษา (zh, en, th)
* Responsive ทุกอุปกรณ์
* UI เป็นภาษาไทยทั้งหมด
* ปลอดภัยและขยายต่อยอดได้ง่าย

---

## ✅ Features Checklist (เฉพาะเวอร์ชัน V4.16)

* [x] ระบบล็อกอิน / ลงทะเบียน / ลืมรหัสผ่าน (UI ภาษาไทย)
* [x] หน้าอ่านนิยาย พร้อมโหมดนักแปล (Translator Mode)
* [x] บรรทัดมาร์ค + คัดลอกมาร์คทั้งหมด
* [x] Bookmark ตอน
* [x] แสดง/ซ่อนตอนตามสิทธิ์
* [x] Upload .txt เป็นตอนแบบ Batch
* [x] ตั้งค่า Default ตอนตอนสร้างนิยาย
* [x] Dashboard นักแปล + Dashboard แอดมิน
* [x] Version Control ตอน
* [x] สถิติและ Logs การใช้งานระบบ
* [x] Modal ทั้งหมดสวยงาม ใช้งานง่าย
* [x] แสดงสถานะตอน/ผู้ใช้ด้วยไอคอน
* [x] รองรับโหมด Light/Dark + ปรับฟอนต์/ธีม
* [x] Font Sarabun + สีทอง-เทา ทั้งระบบ

---

> © 2025 INKREALM.CO – สร้างด้วยหัวใจนักแปล และเครื่องมือแห่งอนาคต
