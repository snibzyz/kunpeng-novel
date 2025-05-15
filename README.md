ยอดเยี่ยมเลยครับ! เพื่อให้ README.md นี้สมบูรณ์และพร้อมใช้งานที่สุดสำหรับ INKREALM.CO (V5.1) ผมจะจัดเรียงเนื้อหาใหม่ตามลำดับความสำคัญที่คุณต้องการ โดยเน้น Tech Stack, Database, และการตั้งค่า Supabase ก่อน จากนั้นจึงลงรายละเอียดของแต่ละหน้าและฟังก์ชัน พร้อมตัวอย่าง Code SQL สำหรับสร้างตาราง และข้อความ Toast Notification ภาษาไทยครับ

```markdown
# INKREALM.CO: ข้อกำหนดระบบและคุณสมบัติหลัก (V5.1) - README

เอกสารนี้เป็นข้อกำหนดระบบและคุณสมบัติหลักสำหรับแพลตฟอร์ม INKREALM.CO (V5.1) โดยเน้นฟังก์ชันที่จำเป็นตามการปรับปรุงล่าสุด ออกแบบมาเพื่อความสะดวกในการปรับใช้, ทดสอบ, และย้ายไปยังโดเมนต่างๆ

## 1. สรุป Tech Stack (Tech Stack Summary)

*   **Framework:** SvelteKit
*   **Styling:** TailwindCSS
*   **UI Library:** Skeleton UI
*   **Form Handling & Validation:** Felte และ Zod
*   **Icon Library:** Lucide-Svelte
*   **Backend & Authentication:** Supabase Platform
    *   Database: PostgreSQL
    *   Authentication: Supabase Auth
    *   Storage: Supabase Storage
    *   Serverless Functions: Supabase Edge Functions (สำหรับ Logic ฝั่ง Server ที่ซับซ้อน หากจำเป็น)
*   **ภาษา:** TypeScript

## 2. ปรัชญาการออกแบบโดยรวม

*   **การแสดงผลหรูหราและอ่านง่าย:**
    *   **สีหลัก:** สีทอง (Gold)
    *   **สีรอง:** สีเทา (Grey)
    *   **โหมดสว่าง (Light Mode):** พื้นหลังหลักสีขาวหรือเทาอ่อน, ตัวอักษรสีเทาเข้ม/ดำ, องค์ประกอบสีทอง
    *   **โหมดมืด (Dark Mode):** พื้นหลังหลักสีดำหรือเทาเข้ม, ตัวอักษรสีเทาอ่อน/ขาว, องค์ประกอบสีทอง
    *   **โลโก้:** อาจใช้ Gradient สีทอง-เทา หรือสีทองเด่น
    *   **ฟอนต์หลัก:** Sarabun (สำหรับเนื้อหาภาษาไทยและ UI ทั่วไป)
    *   การเปลี่ยนธีม (Theme Switching) จะถูกควบคุมผ่านกลไกของ Skeleton UI หรือ CSS Variables ที่กำหนดไว้ เพื่อให้สีสันมีความสอดคล้องและไม่ผิดเพี้ยนเมื่อสลับโหมด
*   **เครื่องมือสำหรับนักแปล:** มีฟังก์ชันสนับสนุนการทำงานของนักแปลโดยเฉพาะ
*   **ใช้งานง่าย ตอบโจทย์ทุกอุปกรณ์:** UI ใช้งานง่าย (Intuitive), Responsive Design, หน้าอ่านไม่มี Horizontal Scroll
*   **ความยืดหยุ่นและการทดสอบ:** โครงสร้างที่เอื้อต่อการเปลี่ยนแปลงโดเมนและการทดสอบระบบที่ง่าย

## 3. โครงสร้างฐานข้อมูล (Database Structure - Supabase PostgreSQL)

ฐานข้อมูลหลักจะถูกออกแบบโดยเน้นความสัมพันธ์ของข้อมูล, ความง่ายในการ Query, และการรองรับ Row-Level Security (RLS) ของ Supabase

### 3.1 SQL Schema (คำสั่งสร้างตาราง)

```sql
-- เปิดใช้งาน Extension ที่จำเป็น (หากยังไม่ได้เปิด)
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- ตารางบทบาทผู้ใช้
CREATE TABLE roles (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL UNIQUE -- เช่น 'Member', 'Translator', 'Admin'
);

-- ตารางข้อมูลผู้ใช้ (เชื่อมโยงกับ auth.users ของ Supabase)
CREATE TABLE profiles (
    id UUID PRIMARY KEY DEFAULT auth.uid() REFERENCES auth.users(id) ON DELETE CASCADE,
    display_name TEXT,
    avatar_url TEXT,
    role_id INTEGER REFERENCES roles(id) DEFAULT 1, -- ตั้งค่าเริ่มต้นเป็น Member
    role_expiry_date TIMESTAMP WITH TIME ZONE,
    is_suspended BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- ตารางหมวดหมู่นิยาย
CREATE TABLE categories (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL UNIQUE,
    slug TEXT NOT NULL UNIQUE,
    description TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- ตารางนิยาย
CREATE TABLE novels (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    title TEXT NOT NULL,
    slug TEXT NOT NULL UNIQUE,
    synopsis TEXT, -- สำหรับ Rich Text อาจเก็บเป็น HTML หรือ Markdown
    cover_image_url TEXT,
    category_id INTEGER REFERENCES categories(id) ON DELETE SET NULL,
    author_id UUID REFERENCES profiles(id) ON DELETE SET NULL, -- ผู้สร้าง/นักแปลหลัก
    original_language TEXT CHECK (original_language IN ('zh', 'en', 'th')), -- จีน, อังกฤษ, ไทย
    visibility TEXT[] DEFAULT ARRAY['ทุกคน']::TEXT[], -- เช่น ARRAY['ทุกคน', 'เฉพาะสมาชิก']
    status TEXT DEFAULT 'ต่อเนื่อง' CHECK (status IN ('ต่อเนื่อง', 'จบแล้ว', 'ยุติการลง')),
    default_chapter_status TEXT DEFAULT 'เผยแพร่' CHECK (default_chapter_status IN ('เผยแพร่', 'ฉบับร่าง')),
    default_chapter_access_level TEXT[] DEFAULT ARRAY['ทุกคน']::TEXT[],
    total_chapters INTEGER DEFAULT 0, -- อาจจะ update ด้วย trigger หรือ application logic
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- ตารางตอนของนิยาย
CREATE TABLE chapters (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    novel_id UUID NOT NULL REFERENCES novels(id) ON DELETE CASCADE,
    chapter_number INTEGER NOT NULL,
    title TEXT NOT NULL,
    content_original TEXT, -- เนื้อหาต้นฉบับ (อาจเป็น HTML จาก Rich Text Editor)
    status TEXT DEFAULT 'เผยแพร่' CHECK (status IN ('เผยแพร่', 'ฉบับร่าง')), -- 'เผยแพร่', 'ฉบับร่าง'
    access_level TEXT[] DEFAULT ARRAY['ทุกคน']::TEXT[], -- เช่น ARRAY['ทุกคน', 'เฉพาะสมาชิก']
    created_by UUID REFERENCES profiles(id) ON DELETE SET NULL,
    word_count INTEGER DEFAULT 0,
    -- read_by_users JSONB, -- อาจจะซับซ้อนไป พิจารณาแยกตารางถ้าจำเป็นมากๆ
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(novel_id, chapter_number) -- แต่ละตอนในนิยายต้องมีเลขที่ไม่ซ้ำกัน
);

-- ตารางบุ๊คมาร์คของผู้ใช้
CREATE TABLE user_bookmarks (
    id BIGSERIAL PRIMARY KEY,
    user_id UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
    chapter_id UUID NOT NULL REFERENCES chapters(id) ON DELETE CASCADE,
    novel_id UUID NOT NULL REFERENCES novels(id) ON DELETE CASCADE, -- เพื่อความสะดวกในการ query
    bookmarked_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    UNIQUE(user_id, chapter_id)
);

-- ตารางบรรทัดมาร์คสำหรับนักแปล (สำคัญมาก!)
CREATE TABLE mark_lines (
    id BIGSERIAL PRIMARY KEY,
    chapter_id UUID NOT NULL REFERENCES chapters(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE, -- ID ของนักแปลที่มาร์ค
    line_number INTEGER NOT NULL, -- เลขบรรทัดที่ถูกมาร์ค (นับจาก 1)
    marked_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    -- ข้อมูลเพิ่มเติมที่อาจช่วยในการ migrate หรือแสดงผล (ปกติจะ JOIN เอา)
    -- novel_title_cache TEXT, -- ชื่อนิยาย (ถ้าต้องการ denormalize)
    -- chapter_title_cache TEXT, -- ชื่อตอน (ถ้าต้องการ denormalize)
    -- user_display_name_cache TEXT, -- ชื่อผู้ใช้ (ถ้าต้องการ denormalize)
    UNIQUE(chapter_id, user_id, line_number) -- นักแปลคนหนึ่งมาร์คบรรทัดหนึ่งในตอนหนึ่งได้ครั้งเดียว
);
-- มุมมอง (View) สำหรับ mark_lines เพื่อให้ดูข้อมูลง่ายขึ้น (แนะนำ)
CREATE OR REPLACE VIEW detailed_mark_lines AS
SELECT
    ml.id AS mark_id,
    ml.line_number,
    ml.marked_at,
    ch.id AS chapter_id,
    ch.chapter_number,
    ch.title AS chapter_title,
    n.id AS novel_id,
    n.title AS novel_title,
    p.id AS user_id,
    p.display_name AS user_display_name
FROM
    mark_lines ml
JOIN
    chapters ch ON ml.chapter_id = ch.id
JOIN
    novels n ON ch.novel_id = n.id
JOIN
    profiles p ON ml.user_id = p.id;


-- ตารางประกาศ
CREATE TABLE announcements (
    id SERIAL PRIMARY KEY,
    title TEXT NOT NULL,
    content TEXT NOT NULL, -- เนื้อหาประกาศ (รองรับ Markdown)
    start_date TIMESTAMP WITH TIME ZONE NOT NULL,
    end_date TIMESTAMP WITH TIME ZONE NOT NULL,
    target_roles TEXT[] DEFAULT ARRAY['ทุกคน']::TEXT[], -- เช่น ARRAY['Member', 'Translator', 'Admin']
    target_pages TEXT[] DEFAULT ARRAY['หน้าแรก']::TEXT[], -- เช่น ARRAY['หน้าแรก', 'หน้านิยายทุกเรื่อง', 'หน้านิยายเฉพาะเรื่อง:[novel_slug]']
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- ตารางการตั้งค่าเว็บไซต์ (เก็บเป็น key-value)
CREATE TABLE site_settings (
    key TEXT PRIMARY KEY,
    value JSONB,
    description TEXT, -- คำอธิบายว่า setting นี้คืออะไร
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- ตาราง Changelog ของเว็บไซต์
CREATE TABLE site_changelogs (
    id SERIAL PRIMARY KEY,
    version_number TEXT NOT NULL UNIQUE,
    release_date DATE NOT NULL,
    summary TEXT NOT NULL, -- รองรับ Markdown
    is_published BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- ตารางเวอร์ชันของข้อมูลนิยาย (Metadata)
CREATE TABLE novel_versions (
    id SERIAL PRIMARY KEY,
    novel_id UUID NOT NULL REFERENCES novels(id) ON DELETE CASCADE,
    version_number INTEGER NOT NULL,
    metadata_snapshot JSONB NOT NULL, -- เก็บ snapshot ของข้อมูลนิยาย ณ เวลานั้น (title, synopsis, category, etc.)
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    created_by UUID REFERENCES profiles(id) ON DELETE SET NULL,
    notes TEXT, -- หมายเหตุสำหรับการเปลี่ยนแปลงเวอร์ชันนี้
    UNIQUE(novel_id, version_number)
);

-- ตารางเวอร์ชันของเนื้อหาตอนนิยาย
CREATE TABLE chapter_versions (
    id SERIAL PRIMARY KEY,
    chapter_id UUID NOT NULL REFERENCES chapters(id) ON DELETE CASCADE,
    version_number INTEGER NOT NULL,
    content_original_snapshot TEXT, -- เก็บ snapshot ของเนื้อหาต้นฉบับ ณ เวลานั้น
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    created_by UUID REFERENCES profiles(id) ON DELETE SET NULL,
    notes TEXT, -- หมายเหตุสำหรับการเปลี่ยนแปลงเวอร์ชันนี้
    UNIQUE(chapter_id, version_number)
);

-- Function to update 'updated_at' timestamp
CREATE OR REPLACE FUNCTION trigger_set_timestamp()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Triggers for 'updated_at'
CREATE TRIGGER set_timestamp_profiles
BEFORE UPDATE ON profiles
FOR EACH ROW
EXECUTE FUNCTION trigger_set_timestamp();

CREATE TRIGGER set_timestamp_categories
BEFORE UPDATE ON categories
FOR EACH ROW
EXECUTE FUNCTION trigger_set_timestamp();

CREATE TRIGGER set_timestamp_novels
BEFORE UPDATE ON novels
FOR EACH ROW
EXECUTE FUNCTION trigger_set_timestamp();

CREATE TRIGGER set_timestamp_chapters
BEFORE UPDATE ON chapters
FOR EACH ROW
EXECUTE FUNCTION trigger_set_timestamp();

CREATE TRIGGER set_timestamp_announcements
BEFORE UPDATE ON announcements
FOR EACH ROW
EXECUTE FUNCTION trigger_set_timestamp();

CREATE TRIGGER set_timestamp_site_settings
BEFORE UPDATE ON site_settings
FOR EACH ROW
EXECUTE FUNCTION trigger_set_timestamp();

CREATE TRIGGER set_timestamp_site_changelogs
BEFORE UPDATE ON site_changelogs
FOR EACH ROW
EXECUTE FUNCTION trigger_set_timestamp();

-- Initial data for roles (จำเป็นต้องมี)
INSERT INTO roles (name) VALUES ('Member'), ('Translator'), ('Admin')
ON CONFLICT (name) DO NOTHING;

-- Initial data for site_settings (ตัวอย่าง)
INSERT INTO site_settings (key, value, description) VALUES
('site_name', '"InkRealm.co"', 'ชื่อเว็บไซต์หลัก'),
('site_description', '"แพลตฟอร์มนิยายออนไลน์สำหรับนักอ่านและนักแปล"', 'คำอธิบายเว็บไซต์สำหรับ SEO'),
('site_logo_url', 'null', 'URL ของโลโก้เว็บไซต์'),
('site_favicon_url', 'null', 'URL ของ Favicon'),
('allow_new_registrations', 'true', 'เปิด/ปิดการลงทะเบียนใหม่'),
('allow_google_login', 'true', 'เปิด/ปิดการเข้าสู่ระบบด้วย Google'),
('default_items_per_page', '20', 'จำนวนรายการต่อหน้าเริ่มต้นสำหรับการแบ่งหน้า'),
('new_novel_default_chapter_status', '"เผยแพร่"', 'สถานะเริ่มต้นของตอนใหม่สำหรับนิยายที่สร้างใหม่'),
('new_novel_default_chapter_access_level', '["ทุกคน"]', 'สิทธิ์การเข้าถึงตอนเริ่มต้นสำหรับนิยายที่สร้างใหม่')
ON CONFLICT (key) DO UPDATE SET value = EXCLUDED.value, description = EXCLUDED.description;

```

### 3.2 การตั้งค่า Supabase และ Environment Variables

1.  **สร้าง Project ใน Supabase:**
    *   ไปที่ [app.supabase.io](https://app.supabase.io) และสร้าง Project ใหม่
    *   จดบันทึก Project URL และ `anon` public key (อยู่ใน Project Settings > API)

2.  **ตั้งค่า Environment Variables (ในไฟล์ `.env` หรือ `.env.local` สำหรับ SvelteKit):**
    ```bash
    # URL ของ Supabase Project ของคุณ
    PUBLIC_SUPABASE_URL="YOUR_SUPABASE_PROJECT_URL"

    # Anon Key (Public) ของ Supabase Project ของคุณ
    PUBLIC_SUPABASE_ANON_KEY="YOUR_SUPABASE_ANON_PUBLIC_KEY"

    # Service Role Key (Secret - ใช้เฉพาะฝั่ง Server เช่น ใน Edge Functions หรือ Server Loaders/Actions)
    # **ห้ามเปิดเผยคีย์นี้ในโค้ดฝั่ง Client เด็ดขาด**
    SUPABASE_SERVICE_ROLE_KEY="YOUR_SUPABASE_SERVICE_ROLE_KEY"

    # Base URL ของเว็บไซต์
    # สำหรับ Local Development:
    PUBLIC_BASE_URL="http://localhost:3000"
    # สำหรับ Production (ตัวอย่าง):
    # PUBLIC_BASE_URL="https://inkrealm.co"
    ```
    *   **สำคัญ:** เพิ่มไฟล์ `.env` หรือ `.env.local` เข้าใน `.gitignore` เพื่อไม่ให้ key ต่างๆ รั่วไหลไปยัง Git repository

3.  **Run SQL Schema:**
    *   นำ Code SQL จากข้อ 3.1 ไปรันใน SQL Editor ของ Supabase (Database > SQL Editor > New query) เพื่อสร้างตารางและข้อมูลเริ่มต้น

4.  **ตั้งค่า Supabase Storage Buckets:**
    *   ไปที่ Storage ใน Dashboard ของ Supabase
    *   สร้าง Buckets ตามรายการด้านล่าง และตั้งค่า Policies ให้เหมาะสม:
        *   `novel-covers`: สำหรับเก็บรูปปกนิยาย
            *   **Policy:** Public Read Access (เพื่อให้ทุกคนเห็นปกได้)
            *   ตัวอย่าง Policy (ตั้งค่าใน Bucket > Policies > `select` หรือสร้าง Policy ใหม่):
                ```json
                {
                  "allowed_origins": ["*"],
                  "allowed_methods": ["GET"],
                  "cache_control": "public, max-age=31536000, immutable"
                }
                ```
                (สำหรับการ `insert`, `update`, `delete` จะใช้ RLS หรือ Service Role Key ผ่าน Server)
        *   `avatars`: สำหรับเก็บรูปโปรไฟล์ผู้ใช้
            *   **Policy:** Public Read Access (คล้าย `novel-covers`)
        *   `site_assets`: สำหรับเก็บโลโก้, favicon ของเว็บไซต์
            *   **Policy:** Public Read Access (คล้าย `novel-covers`)
        *   `chapter-text-files`: สำหรับ Batch Upload ไฟล์ .txt (ใช้เป็น Temporary Storage)
            *   **Policy:** ควรตั้งค่าให้มีการจำกัดสิทธิ์การเข้าถึงอย่างเข้มงวด อาจจะอนุญาตให้อัปโหลดผ่าน authenticated users ที่มีบทบาท Translator/Admin และตั้งค่าให้ไฟล์ถูกลบอัตโนมัติหลังประมวลผลเสร็จสิ้น (ผ่าน Edge Function หรือ Logic ใน Application) หรือถ้าไม่สามารถลบอัตโนมัติได้ ให้เข้าถึงได้เฉพาะ Service Role Key

5.  **ตั้งค่า Row-Level Security (RLS):**
    *   RLS เป็นหัวใจสำคัญของความปลอดภัยใน Supabase
    *   **ต้องเปิดใช้งาน RLS สำหรับทุกตารางที่มีข้อมูลสำคัญ** (เช่น `novels`, `chapters`, `profiles`, `mark_lines`)
    *   สร้าง Policies สำหรับการ `SELECT`, `INSERT`, `UPDATE`, `DELETE` ให้สอดคล้องกับบทบาทและสิทธิ์ของผู้ใช้
    *   **ตัวอย่าง RLS Policy สำหรับตาราง `novels` (แนวคิด):**
        *   **SELECT:**
            *   `Admin` สามารถ SELECT ได้ทุก records
            *   `Translator` สามารถ SELECT ได้ทุก records ที่ `visibility` อนุญาต หรือเป็นเจ้าของ
            *   `Member` สามารถ SELECT ได้ทุก records ที่ `visibility` อนุญาต
            *   `Guest` (unauthenticated) สามารถ SELECT ได้เฉพาะ records ที่ `visibility` เป็น 'ทุกคน'
        *   **INSERT:**
            *   `Translator` และ `Admin` สามารถ INSERT ได้ (โดย `author_id` เป็น `auth.uid()`)
        *   **UPDATE:**
            *   `Admin` สามารถ UPDATE ได้ทุก records
            *   `Translator` สามารถ UPDATE ได้เฉพาะ novels ที่ตนเองเป็น `author_id`
        *   **DELETE:**
            *   `Admin` สามารถ DELETE ได้ทุก records
            *   `Translator` สามารถ DELETE ได้เฉพาะ novels ที่ตนเองเป็น `author_id`
    *   **คุณต้องสร้าง Policy เหล่านี้เองใน SQL Editor หรือผ่าน UI ของ Supabase**

## 4. โครงสร้าง URL ที่เป็นมิตรและการจัดการโดเมน

*   **โครงสร้าง URL หลัก:**
    *   หน้ารวมนิยาย (หน้าแรก): `/`
    *   หน้าข้อมูลนิยาย: `/novels/[novel_slug]`
    *   หน้าอ่านตอนนิยาย: `/novels/[novel_slug]/chapter/[chapter_number]`
    *   หน้าโปรไฟล์ผู้ใช้: `/profile`
    *   หน้าบุ๊คมาร์ค: `/bookmarks`
    *   แดชบอร์ดนักแปล: `/translator/dashboard`
    *   หน้าจัดการบรรทัดมาร์ค (นักแปล): `/translator/manage-marks/[novel_slug]`
    *   แดชบอร์ดผู้ดูแลระบบ: `/admin/dashboard` (และหน้ารายละเอียดภายใน เช่น `/admin/dashboard/users`)
*   **การสร้าง Slug:**
    *   `novel_slug`: สร้างอัตโนมัติจากชื่อเรื่อง (ตัวพิมพ์เล็ก, แทนที่ช่องว่างด้วย `-`, ตัดอักขระพิเศษ) ผู้สร้าง/Admin แก้ไขได้ (มีการตรวจสอบความซ้ำซ้อนกับ `novels.slug`)
    *   `chapter`: ใช้ `chapter_number` โดยตรง
*   **การจัดการโดเมนและ Base URL:**
    *   ใช้ `PUBLIC_BASE_URL` จาก Environment Variable สำหรับสร้างลิงก์สมบูรณ์
    *   ลิงก์ภายในใช้ Relative Paths หรือ `PUBLIC_BASE_URL`

## 5. บทบาทผู้ใช้และสิทธิ์ (User Roles & Permissions)

ควบคุมโดย `profiles.role_id` และ Supabase Row-Level Security (RLS)

| บทบาท         | คำอธิบาย                                                                                                                                                                                                                                                          |
| :------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **สมาชิก (Member)** | อ่านนิยาย, บุ๊คมาร์คตอน, แก้ไขโปรไฟล์ส่วนตัว. แถบเครื่องมืออ่าน: สารบัญ, ปรับแต่งการแสดงผล, บุ๊คมาร์ค, สลับธีม, โหมดอ่านต่อเนื่อง, นำทางตอน.                                                                                                                               |
| **นักแปล (Translator)** | สิทธิ์ทั้งหมดของสมาชิก. +เปิด/ปิด "โหมดนักแปล" สำหรับนิยายที่ตนมีสิทธิ์, เข้าถึงโหมดแปล, ใช้งานบรรทัดมาร์ค, สร้าง/จัดการนิยายและตอนของตนเอง (ผ่าน Modal), เข้าถึงแดชบอร์ดนักแปลและหน้าจัดการบรรทัดมาร์ค. แถบเครื่องมือพิเศษในโหมดแปล: เพิ่ม "คัดลอกมาร์คทั้งหมด". |
| **ผู้ดูแลระบบ (Admin)** | สิทธิ์ทั้งหมดของนักแปล. +เข้าถึงแดชบอร์ดผู้ดูแลระบบ, จัดการผู้ใช้, ดูข้อมูลมาร์คทั้งหมด, จัดการเว็บไซต์, จัดการหมวดหมู่, จัดการประกาศ, จัดการเวอร์ชัน.                                                                                                                       |

## 6. การเข้าสู่ระบบ / ลงทะเบียน / ลืมรหัสผ่าน (Authentication)

*   **UI:** แสดงผลผ่าน Modal เป็นหลัก (เรียกจาก Navbar) และมีหน้าเฉพาะ (`/login`, `/register`, `/forgot-password`)
*   **Tech Stack:** Supabase Auth, Felte + Zod (สำหรับ Validation), Skeleton UI (Modal, Input, Button)
*   **Toast Notifications (มุมล่างขวา):**
    *   ลงทะเบียนสำเร็จ: `Toast: "ลงทะเบียนเรียบร้อย กรุณายืนยันอีเมลของคุณ"`
    *   เข้าสู่ระบบสำเร็จ: `Toast: "เข้าสู่ระบบสำเร็จ ยินดีต้อนรับ!"`
    *   คำขอรีเซ็ตรหัสผ่านถูกส่ง: `Toast: "คำขอรีเซ็ตรหัสผ่านถูกส่งไปยังอีเมลของคุณแล้ว"`
    *   ยืนยันอีเมล (เมื่อคลิกลิงก์ในอีเมลแล้วกลับมา): `Toast: "ยืนยันอีเมลสำเร็จแล้ว คุณสามารถเข้าสู่ระบบได้เลย"`
    *   ข้อผิดพลาด: `Toast: "เกิดข้อผิดพลาด: [รายละเอียดข้อผิดพลาดสั้นๆ]"`

## 7. โครงสร้างแถบนำทาง (Navbar Structure)

*   **ความสามารถ:** แสดงเมนูตามบทบาทผู้ใช้ (Dynamic), Responsive
*   **Tech Stack:** SvelteKit (Layout), Skeleton UI (App Bar, Menu, Button), TailwindCSS
*   **โลโก้:** (ซ้ายสุด) คลิกแล้วไปหน้าแรก (`/`)
*   **ปุ่มสลับธีม:** (ไอคอน `Sun` / `Moon` จาก Lucide-Svelte)
*   **เมนู (เรียงจากซ้ายไปขวาหลังโลโก้):**

    | ผู้ใช้         | เมนู                                                                                                                                                                                             |
    | :------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | **Guest**      | (โลโก้), ปุ่มสลับธีม, ปุ่ม "เข้าสู่ระบบ" (เปิด Login Modal)                                                                                                                                         |
    | **Member**     | (โลโก้), "หน้าแรก" (`/`), "บุ๊คมาร์ค" (`/bookmarks`), ปุ่มสลับธีม, โปรไฟล์ (Dropdown: แสดงชื่อ & สิทธิ์, ลิงก์ "หน้าโปรไฟล์" (`/profile`), "ออกจากระบบ")                                                     |
    | **Translator** | เหมือน Member เพิ่ม: "แดชบอร์ดนักแปล" (`/translator/dashboard`)                                                                                                                                     |
    | **Admin**      | เหมือน Translator เพิ่ม: "แดชบอร์ดผู้ดูแลระบบ" (`/admin/dashboard`)                                                                                                                                |

## 8. รายละเอียดหน้าเว็บไซต์, Modals, และการจัดการเนื้อหา

---

### 8.1 หน้าแรก (Home Page - `/`)

*   **ส่วนแสดงประกาศ (Announcements Display Area):**
    *   **UI:** แบนเนอร์ด้านบนสุดของเนื้อหาหลัก (ใต้ Navbar), มีปุ่มปิดประกาศ (ไอคอน `X` จาก Lucide-Svelte)
    *   **Logic:** ดึงข้อมูลจาก `announcements` table. แสดงหาก `is_active = true`, อยู่ใน `start_date` ถึง `end_date`, `target_pages` มี 'หน้าแรก', และ `target_roles` ตรงกับบทบาทผู้ใช้ปัจจุบัน
*   **ส่วนเครื่องมือหลัก:**
    *   **ช่องค้นหานิยาย:** `Input` จาก Skeleton UI, placeholder "ค้นหานิยาย..."
    *   **ฟิลเตอร์หมวดหมู่ (Category Filter):** `Select` (Dropdown) จาก Skeleton UI. รายการหมวดหมู่ดึงจาก `categories` table
    *   **ฟิลเตอร์สถานะนิยาย (Novel Status Filter):** `Select` (Dropdown) จาก Skeleton UI. ตัวเลือก: "ทั้งหมด", "ต่อเนื่อง", "จบแล้ว"
    *   **ปุ่มสร้างนิยาย (Create Novel Button):** (สำหรับ Translator/Admin)
        *   **UI:** ปุ่ม "[+ สร้างนิยาย]" (ไอคอน `PlusSquare` จาก Lucide-Svelte) จัดวางใกล้เครื่องมือค้นหา
        *   **Action:** คลิกเปิด "Modal สร้างนิยาย" (ดูข้อ 8.1.1)
*   **การแสดงรายการนิยาย:**
    *   **UI:** Grid Layout (Skeleton UI Card). การ์ดแสดง: รูปปก (อัตราส่วน 3:4), ชื่อเรื่อง, จำนวนตอน (ดึงจาก `novels.total_chapters` หรือ `COUNT(chapters)`). Mobile: 2 คอลัมน์, Desktop: 3-5 คอลัมน์
    *   **Database:** Query จาก `novels` table, JOIN `categories` (ถ้าจำเป็น), Filter ตามคำค้นหา, หมวดหมู่, สถานะ
*   **ระบบแบ่งหน้า (Pagination):**
    *   **UI:** Skeleton UI Pager. แสดงเมื่อผลลัพธ์มีมากกว่าจำนวนที่ตั้งค่าไว้ต่อหน้า (จาก `site_settings.default_items_per_page`)
*   **การทำงานเริ่มต้น (สำหรับ Guest):**
    *   หากเป็น Guest ที่เข้าชมครั้งแรก (อาจใช้ `localStorage` เช็ค) แสดง Login Modal อัตโนมัติ
*   **Tech Stack:** Supabase (query `novels`, `categories`, `announcements`), Skeleton UI (Card, Input, Select, Button, Pager)

---

#### 8.1.1 Modal สร้างนิยาย (Create Novel Modal)

*   **การเรียกใช้งาน:** คลิกปุ่ม "[+ สร้างนิยาย]" บนหน้าแรก
*   **UI ภายใน Modal (Skeleton UI Modal):**
    *   **หัวเรื่อง Modal:** "สร้างนิยายใหม่"
    *   **ฟอร์ม (Felte + Zod validation):**
        *   **ชื่อเรื่อง (Title):** `Input text`, จำเป็น (`required`)
        *   **คำโปรย (Synopsis):** `Textarea` (รองรับ Rich Text Editor เช่น TipTap หรือ Markdown editor ที่แปลงเป็น HTML ก่อนบันทึก)
        *   **หมวดหมู่ (Category):** `Select` (Dropdown) ดึงจาก `categories` table (`required`)
        *   **รูปปกนิยาย (Cover Image):** `File Input` พื้นที่อัปโหลดรูปใหม่
            *   **เครื่องมือ Crop:** JS library (เช่น Cropper.js) บังคับอัตราส่วน 3:4
            *   **ขนาดไฟล์:** จัดการขนาดไฟล์ไม่เกิน 3MB (ย่อขนาดอัตโนมัติฝั่ง Client หรือ Server)
        *   **ภาษาต้นฉบับ (Original Language):** `Select` (Dropdown) ตัวเลือก: "จีน (zh)", "อังกฤษ (en)", "ไทย (th)" (`required`)
        *   **สิทธิ์ในการมองเห็นนิยาย (Novel Visibility):** `Checkbox Group` (Skeleton UI) เลือกได้หลายอัน: "ทุกคน", "เฉพาะสมาชิก", "เฉพาะนักแปล" (ค่าเริ่มต้น: "ทุกคน")
        *   **การตั้งค่าเริ่มต้นสำหรับตอนใหม่ของนิยายนี้:**
            *   **สถานะเริ่มต้นของตอน:** `Radio Group` (Skeleton UI) "เผยแพร่" (ค่าเริ่มต้น), "ฉบับร่าง"
            *   **สิทธิ์การเข้าถึงตอนเริ่มต้น:** `Checkbox Group` "ทุกคน" (ค่าเริ่มต้น), "เฉพาะสมาชิก", "เฉพาะนักแปล"
    *   **ปุ่มดำเนินการ (Skeleton UI Button):**
        *   "เผยแพร่นิยาย":
            *   **Action:** สร้าง record ใน `novels` (status: 'ต่อเนื่อง', visibility, default chapter settings ตามที่เลือก), อัปโหลดปกไป `novel-covers` bucket, สร้าง record ใน `novel_versions`
            *   **Toast:** `"[ชื่อนิยาย]' ถูกสร้างและเผยแพร่เรียบร้อยแล้ว!"` (สีเขียว)
        *   "บันทึกเป็นฉบับร่าง":
            *   **Action:** คล้าย "เผยแพร่" แต่ตั้ง `novels.status` เป็น 'ฉบับร่าง' (หรือมี field แยกสำหรับ draft status ของ novel เอง) หรือตั้ง `visibility` เป็นแบบจำกัด
            *   **Toast:** `"[ชื่อนิยาย]' ถูกบันทึกเป็นฉบับร่างแล้ว"` (สีเหลือง/ส้ม)
        *   "ยกเลิก": ปิด Modal
*   **Database:** `INSERT INTO novels`, `INSERT INTO novel_versions`. อัปโหลดไฟล์ไป Supabase Storage (`novel-covers`).

---

### 8.2 หน้าข้อมูลนิยายและสารบัญ (Novel Detail Page - `/novels/[novel_slug]`)

*   **การดึงข้อมูล:** ใช้ `novel_slug` จาก URL เพื่อ query ข้อมูลจาก `novels` table และ `chapters` table ที่เกี่ยวข้อง
*   **ส่วนแสดงประกาศ:** คล้ายหน้าแรก แต่ `target_pages` อาจเป็น 'หน้านิยาย (ทุกเรื่อง)' หรือ 'หน้านิยาย (เฉพาะเรื่องนี้):[novel_slug]'
*   **ส่วนข้อมูลนิยาย (Novel Information Section):**
    *   **UI:** แสดงรูปปก (จาก `novels.cover_image_url`), ชื่อเรื่อง (`novels.title`), ผู้แต่ง (จาก `novels.author_id` JOIN `profiles.display_name`), หมวดหมู่ (จาก `novels.category_id` JOIN `categories.name`), คำโปรย (`novels.synopsis` - แสดงผล Rich Text), จำนวนตอนทั้งหมด (`novels.total_chapters`), สถานะนิยาย (`novels.status`), ภาษาต้นฉบับ (`novels.original_language` พร้อม `lang` attribute ใน HTML)
    *   **(สำหรับนักแปล/Admin ที่มีสิทธิ์ในนิยาย): ปุ่มสลับ "โหมดนักแปล" (Translator Mode Toggle):**
        *   **UI:** `Toggle Switch` (Skeleton UI) ข้อความ "เปิด/ปิดโหมดนักแปล"
        *   **Logic:** สถานะ (เปิด/ปิด) บันทึกใน `localStorage` หรือ `user_settings` table (scoped by user_id and novel_id). **ไม่เปลี่ยนโหมดจากหน้าอ่าน ต้องกลับมาหน้านี้เปลี่ยน**
*   **ปุ่มจัดการนิยาย (สำหรับผู้สร้างนิยาย หรือ Admin):**
    *   **ปุ่ม "แก้ไขข้อมูลนิยาย":** (ไอคอน `Edit3` จาก Lucide-Svelte)
        *   **Action:** เปิด "Modal แก้ไขข้อมูลนิยาย" (ดูข้อ 8.2.1)
    *   **ปุ่ม "ลบนิยาย":** (ไอคอน `Trash2` จาก Lucide-Svelte, สีแดง)
        *   **Action:** เปิด Modal ยืนยัน (Skeleton UI Confirm) "คุณแน่ใจหรือไม่ว่าต้องการลบนิยาย '[ชื่อนิยาย]'? การกระทำนี้ไม่สามารถย้อนกลับได้ และจะลบตอนทั้งหมดของนิยายนี้ด้วย"
        *   **ยืนยัน:** `DELETE FROM novels WHERE id = [novel_id]` (RLS ป้องกันการลบโดยไม่ได้รับอนุญาต)
        *   **Toast (สำเร็จ):** `"ลบนิยาย '[ชื่อนิยาย]' เรียบร้อยแล้ว"` (สีแดง)
        *   **Toast (ล้มเหลว):** `"ไม่สามารถลบนิยายได้: [เหตุผล]"` (สีแดง)
*   **ส่วนเพิ่มตอน (Add Chapter Section - สำหรับผู้สร้าง/Admin):**
    *   **UI:** จัดวางด้านขวาของข้อมูลนิยาย หรือใกล้สารบัญ
    *   **ปุ่ม "เพิ่มตอนด้วย .txt":** (ไอคอน `FileText` จาก Lucide-Svelte) เปิด "Modal เพิ่มตอนด้วย .txt" (ดูข้อ 8.2.2)
    *   **ปุ่ม "เพิ่มตอนใหม่":** (ไอคอน `PlusCircle` จาก Lucide-Svelte) เปิด "Modal เพิ่ม/แก้ไขตอน" (ดูข้อ 8.2.3, ในโหมด "เพิ่มตอน")
*   **ส่วนสารบัญ (Table of Contents Section):**
    *   **UI:** `Accordion` (Skeleton UI). ปรับจำนวนตอนต่อกลุ่ม (Dropdown: 10, 20, 25, 30, 50 (ค่าเริ่มต้น), 100 ตอน). กลุ่มแรกขยายเสมอ.
    *   **รายการตอน (แต่ละแถว):**
        *   **(ทุกคน):**
            *   ไอคอนบุ๊คมาร์ค (มุมซ้ายบนของชื่อตอน, ไอคอน `Bookmark` จาก Lucide-Svelte, เติมสี/เปลี่ยนไอคอนถ้าบุ๊คมาร์คแล้ว)
            *   ชื่อตอน (`chapters.title`) - เป็นลิงก์ไปยังหน้าอ่าน `/novels/[novel_slug]/chapter/[chapter_number]`
            *   สถานะการอ่าน: (PC) เปลี่ยนสีพื้นหลัง + ไอคอน `Eye` + "อ่านแล้ว"; (Mobile) เปลี่ยนสีพื้นหลัง + ไอคอน `Eye`. (เช็คจาก `user_read_progress` table หรือ `localStorage`)
        *   **(นักแปล/Admin ที่มีสิทธิ์):** เพิ่มเติมจากด้านบน:
            *   ไอคอนสถานะตอน: ไอคอน `CheckCircle` (เผยแพร่) หรือ `Edit` (ร่าง) จาก Lucide-Svelte.
            *   ปุ่ม "แก้ไขตอน": (ไอคอน `Edit` จาก Lucide-Svelte) เปิด "Modal เพิ่ม/แก้ไขตอน" (ข้อ 8.2.3, ในโหมด "แก้ไขตอน")
            *   ปุ่ม "ลบตอน": (ไอคอน `Trash` จาก Lucide-Svelte, สีแดง) เปิด Modal ยืนยัน "คุณแน่ใจหรือไม่ว่าต้องการลบตอน '[ชื่อตอน]'?"
                *   **ยืนยัน:** `DELETE FROM chapters WHERE id = [chapter_id]`. อัปเดต `novels.total_chapters`.
                *   **Toast (สำเร็จ):** `"ลบตอน '[ชื่อตอน]' เรียบร้อยแล้ว"`
            *   ปุ่ม "คัดลอกมาร์คทั้งหมด": (ไอคอน `CopyCheck` จาก Lucide-Svelte) (เฉพาะเมื่อ "โหมดนักแปล" สำหรับนิยายนี้เปิดอยู่ และผู้ใช้เป็นนักแปลของตอนนี้)
                *   **Action:** Query `mark_lines` สำหรับตอนนี้และ user ปัจจุบัน, คัดลอกเลขบรรทัดที่มาร์คทั้งหมดไปยัง Clipboard.
                *   **Toast:** `"คัดลอกมาร์คของตอน '[ชื่อตอน]' ทั้งหมดแล้ว"`
*   **Tech Stack:** Supabase (query `novels`, `chapters`, `profiles`, `categories`), Skeleton UI (Accordion, Button, Toggle, Modal), Svelte logic

---

#### 8.2.1 Modal แก้ไขข้อมูลนิยาย (Edit Novel Modal)

*   **การเรียกใช้งาน:** คลิกปุ่ม "แก้ไขข้อมูลนิยาย" ในหน้าข้อมูลนิยาย
*   **UI ภายใน Modal:** ฟอร์มจะถูกเติมด้วยข้อมูลปัจจุบันของนิยาย (`novels` table)
    *   **หัวเรื่อง Modal:** "แก้ไขข้อมูลนิยาย: [ชื่อนิยายปัจจุบัน]"
    *   **ฟิลด์ที่แก้ไขได้ (Felte + Zod):** ชื่อเรื่อง, คำโปรย (Rich Text Editor), หมวดหมู่ (Dropdown), รูปปก (อัปโหลดใหม่ + Crop 3:4, ย่อขนาดถ้าเกิน 3MB), ภาษาต้นฉบับ (Dropdown: zh, en, th), สถานะนิยาย (`Radio Group`: "ต่อเนื่อง", "จบแล้ว", "ยุติการลง"), สิทธิ์ในการมองเห็นนิยาย (`Checkbox Group`: "ทุกคน", "เฉพาะสมาชิก", "เฉพาะนักแปล")
    *   **ค่าเริ่มต้นสำหรับการสร้างตอนใหม่ (Default Settings for New Chapters of this Novel):**
        *   **สถานะเริ่มต้นของตอน:** `Radio Group` ("เผยแพร่", "ฉบับร่าง")
        *   **สิทธิ์การเข้าถึงตอนเริ่มต้น:** `Checkbox Group` ("ทุกคน", "เฉพาะสมาชิก", "เฉพาะนักแปล")
        *   **คำอธิบาย:** "ค่าเริ่มต้นของเว็บสำหรับตอนใหม่คือ 'เผยแพร่' และ 'ทุกคน'. คุณสามารถปรับค่านี้สำหรับนิยายเรื่องนี้ได้ที่นี่"
    *   **ปุ่มดำเนินการ:**
        *   "บันทึกการเปลี่ยนแปลง":
            *   **Action:** `UPDATE novels SET ... WHERE id = [novel_id]`. อัปโหลดปกใหม่ (ถ้ามี). สร้าง record ใหม่ใน `novel_versions` หากมีการเปลี่ยนแปลงข้อมูลสำคัญ (เช่น title, synopsis, category, cover).
            *   **Toast:** `"บันทึกข้อมูลนิยาย '[ชื่อนิยาย]' สำเร็จแล้ว"`
        *   "ยกเลิก"
        *   ปุ่ม "ลบนิยาย" (ภายใน Modal, ไอคอน `Trash2` สีแดง):
            *   **Action:** เหมือนปุ่มลบนิยายด้านนอก (เปิด Modal ยืนยันอีกชั้น)
            *   **Toast (สำเร็จ):** `"ลบนิยาย '[ชื่อนิยาย]' เรียบร้อยแล้ว"` (และปิด Modal แก้ไข)
*   **Database:** `UPDATE novels`, `INSERT INTO novel_versions`, Supabase Storage (`novel-covers`)

---

#### 8.2.2 Modal เพิ่มตอนด้วย .txt (Batch Upload Modal)

*   **การเรียกใช้งาน:** คลิกปุ่ม "เพิ่มตอนด้วย .txt" ในหน้าข้อมูลนิยาย
*   **UI ภายใน Modal:**
    *   **หัวเรื่อง Modal:** "เพิ่มหลายตอนด้วยไฟล์ .txt สำหรับนิยาย: [ชื่อนิยาย]"
    *   **พื้นที่ Drag & Drop ไฟล์:** หรือปุ่ม "เลือกไฟล์" (`File Input` `multiple`)
    *   **ข้อจำกัด:** "รองรับสูงสุด 500 ไฟล์. ชื่อไฟล์จะถูกใช้เป็นชื่อตอน. ไฟล์ที่มีชื่อซ้ำจะถูกถามว่าจะเขียนทับหรือข้าม."
    *   **ตัวเลือก (Checkbox/Radio):** "หากไฟล์ซ้ำ: เขียนทับ / ข้าม"
    *   **Progress Bar:** แสดงความคืบหน้าการอัปโหลดและประมวลผล
    *   **ปุ่มดำเนินการ:** "เริ่มอัปโหลด", "ยกเลิก"
*   **Logic:**
    1.  ผู้ใช้อัปโหลดไฟล์ .txt (เก็บใน `chapter-text-files` bucket ชั่วคราว)
    2.  Client หรือ Edge Function ประมวลผล:
        *   อ่านเนื้อหาจากแต่ละไฟล์.
        *   ใช้ชื่อไฟล์เป็น `chapters.title`.
        *   กำหนด `chapter_number` (อาจเรียงตามชื่อไฟล์ หรือให้ผู้ใช้จัดลำดับก่อนอัปโหลด).
        *   ตรวจสอบ `chapter_number` และ `title` ซ้ำกับที่มีอยู่แล้วในนิยายนี้.
        *   `INSERT` หรือ `UPDATE` ข้อมูลใน `chapters` table.
        *   ดึงค่า `default_chapter_status` และ `default_chapter_access_level` จาก `novels` table มาใช้สำหรับตอนใหม่.
        *   สร้าง record ว่างใน `mark_lines` สำหรับแต่ละตอนใหม่และ `author_id` ของนิยาย.
        *   สร้าง record เริ่มต้นใน `chapter_versions` สำหรับแต่ละตอนใหม่.
        *   ลบไฟล์ .txt ออกจาก `chapter-text-files` bucket หลังประมวลผล.
*   **Toast (เมื่อสำเร็จ):**
    *   `"เพิ่มตอนสำเร็จ: เพิ่ม X ตอน, เขียนทับ Y ตอน, ข้าม Z ตอน"`
*   **Database:** `INSERT/UPDATE chapters`, `INSERT INTO mark_lines` (ว่าง), `INSERT INTO chapter_versions`. Supabase Storage (`chapter-text-files` ชั่วคราว).

---

#### 8.2.3 Modal เพิ่ม/แก้ไขตอน (Create/Edit Chapter Modal)

*   **การเรียกใช้งาน:**
    *   **เพิ่มตอน:** จากปุ่ม "เพิ่มตอนใหม่" ในหน้าข้อมูลนิยาย
    *   **แก้ไขตอน:** จากปุ่ม "แก้ไขตอน" ในส่วนสารบัญ
*   **UI ภายใน Modal:**
    *   **หัวเรื่อง:** "เพิ่มตอนใหม่สำหรับ: [ชื่อนิยาย]" หรือ "แก้ไขตอน: [ชื่อตอนปัจจุบัน]"
    *   **ฟอร์ม (Felte + Zod):**
        *   **เลขที่ตอน (Chapter Number):** (สำหรับ "เพิ่มตอนใหม่") `Input number`. ถ้า "แก้ไขตอน" จะเป็น `Read-only` หรือซ่อนไว้. (ต้องตรวจสอบไม่ให้ซ้ำกับ `chapter_number` ที่มีอยู่แล้วในนิยายนี้)
        *   **ชื่อตอน (Chapter Title):** `Input text`, จำเป็น
        *   **เนื้อหา (Content):** `Rich Text Editor` (เช่น TipTap)
            *   **ฟอนต์:** Sarabun เป็นหลัก
            *   **เครื่องมือ:** ตัวหนา, ตัวเอียง, ขีดเส้นใต้, รายการ, ลิงก์
            *   **การวาง:** รองรับการคัดลอก/วางจาก Word โดยพยายามรักษาการจัดรูปแบบพื้นฐาน
        *   **สถานะตอน (Chapter Status):** `Radio Group` ("เผยแพร่", "ฉบับร่าง"). ค่าเริ่มต้นมาจาก `novels.default_chapter_status` (สำหรับตอนใหม่) หรือค่าปัจจุบัน (สำหรับแก้ไข)
        *   **สิทธิ์การเข้าถึงตอน (Chapter Access Level):** `Checkbox Group` ("ทุกคน", "เฉพาะสมาชิก", "เฉพาะนักแปล"). ค่าเริ่มต้นมาจาก `novels.default_chapter_access_level` (สำหรับตอนใหม่) หรือค่าปัจจุบัน (สำหรับแก้ไข)
    *   **ปุ่มดำเนินการ:**
        *   **(เพิ่มตอนใหม่):** "เผยแพร่ตอน", "บันทึกเป็นฉบับร่าง"
            *   **Action (เผยแพร่):** `INSERT INTO chapters` (status: 'เผยแพร่'). สร้าง `mark_lines` ว่าง, สร้าง `chapter_versions` เริ่มต้น. อัปเดต `novels.total_chapters`.
            *   **Action (ร่าง):** `INSERT INTO chapters` (status: 'ฉบับร่าง'). สร้าง `mark_lines` ว่าง, สร้าง `chapter_versions` เริ่มต้น. อัปเดต `novels.total_chapters`.
            *   **Toast:** `"บันทึกตอน '[ชื่อตอน]' และเผยแพร่เรียบร้อยแล้ว"` หรือ `"บันทึกตอน '[ชื่อตอน]' เป็นฉบับร่างเรียบร้อยแล้ว"`
        *   **(แก้ไขตอน):** "บันทึกการเปลี่ยนแปลง"
            *   **Action:** `UPDATE chapters SET ... WHERE id = [chapter_id]`. สร้าง `chapter_versions` ใหม่หาก `content_original` เปลี่ยนแปลง.
            *   **Toast:** `"อัปเดตตอน '[ชื่อตอน]' เรียบร้อยแล้ว"`
        *   (ทั้งสองกรณี): "ยกเลิก"
*   **Database:** `INSERT/UPDATE chapters`, `INSERT INTO mark_lines` (ถ้าสร้างใหม่), `INSERT INTO chapter_versions`. อัปเดต `novels`.

---

### 8.3 หน้าบุ๊คมาร์ค (Bookmarks Page - `/bookmarks`)

*   **การดึงข้อมูล:** Query `user_bookmarks` table โดย `JOIN` กับ `novels` และ `chapters` เพื่อดึงข้อมูล `user_id` ปัจจุบัน.
*   **UI:**
    *   **หัวเรื่อง:** "บุ๊คมาร์คของฉัน"
    *   **รายการนิยายที่บุ๊คมาร์ค:** (เรียงตาม `user_bookmarks.bookmarked_at` ล่าสุดของนิยายนั้นๆ)
        *   **แต่ละรายการนิยาย (Card):** รูปปก (ซ้าย), ชื่อนิยาย, "ตอนล่าสุดที่บุ๊คมาร์ค: [ชื่อตอน]" (ลิงก์ไปตอนนั้น), ปุ่ม "ไปยังบุ๊คมาร์คล่าสุด" (ไอคอน `BookOpen` จาก Lucide-Svelte) (ขวา)
        *   ปุ่ม "จัดการบุ๊คมาร์คของเรื่องนี้" (ไอคอน `Settings` จาก Lucide-Svelte)
            *   **Action:** เปิด Modal "จัดการบุ๊คมาร์คสำหรับเรื่อง: [ชื่อนิยาย]"
                *   **ภายใน Modal:**
                    *   **ตัวเลือกเรียงลำดับ:** Dropdown ("ตามลำดับตอน", "ตามวันที่บุ๊คมาร์คล่าสุด")
                    *   **รายการบุ๊คมาร์คของเรื่องนั้น (Table/List):** `Checkbox` (สำหรับเลือก), ชื่อตอน, วันที่/เวลาบุ๊คมาร์ค. ไฮไลท์บุ๊คมาร์คล่าสุดของเรื่อง.
                    *   **ปุ่ม:**
                        *   "ลบที่เลือก":
                            *   **Action:** `DELETE FROM user_bookmarks WHERE id IN (...)`
                            *   **Toast:** `"ลบบุ๊คมาร์ค X รายการสำเร็จ"`
                        *   "ล้างบุ๊คมาร์คทั้งหมดของเรื่องนี้": (PC: ไอคอน `Trash2`+ข้อความ, Mobile: ไอคอน `Trash2`)
                            *   **Action:** เปิด Modal ยืนยัน. ยืนยันแล้ว `DELETE FROM user_bookmarks WHERE novel_id = [novel_id] AND user_id = [user_id]`
                            *   **Toast:** `"ลบบุ๊คมาร์คทั้งหมดของเรื่อง '[ชื่อนิยาย]' สำเร็จ"`
                        *   "บันทึก/เสร็จสิ้น", "ยกเลิก"
    *   **ปุ่ม "ลบบุ๊คมาร์คทั้งหมด (ทุกเรื่อง)":** (ด้านบนของหน้า หรือท้ายรายการ)
        *   **Action:** เปิด Modal ยืนยัน "คุณแน่ใจหรือไม่ว่าต้องการลบบุ๊คมาร์คทั้งหมดของคุณ?"
        *   **ยืนยัน:** `DELETE FROM user_bookmarks WHERE user_id = [user_id]`
        *   **Toast:** `"ลบบุ๊คมาร์คทั้งหมด (ทุกเรื่อง) เรียบร้อยแล้ว"`
*   **Tech Stack:** Supabase (query `user_bookmarks`, `novels`, `chapters`), Skeleton UI (Card, Button, Modal, Table, Checkbox, Dropdown), Lucide-Svelte

---

### 8.4 แดชบอร์ดนักแปล (Translator Dashboard - `/translator/dashboard`)

*   **การดึงข้อมูล:** Query `novels` table ที่ `author_id` คือ `user_id` ปัจจุบัน และ `chapters`, `mark_lines` ที่เกี่ยวข้องเพื่อคำนวณความคืบหน้า
*   **UI:**
    *   **หัวเรื่อง:** "แดชบอร์ดนักแปล"
    *   **รายการนิยายที่นักแปลจัดการ (Grid of Cards):**
        *   **แต่ละ Card:** รูปปก, ชื่อนิยาย
        *   **ความคืบหน้าการมาร์ค:** "[X]% (Y/Z ตอนที่มาร์คแล้ว)" + `Progress Bar` (Skeleton UI). คำนวณจากจำนวนตอนที่มี `mark_lines` อย่างน้อย 1 บรรทัด เทียบกับจำนวนตอนทั้งหมดของนิยาย.
        *   ปุ่ม "จัดการบรรทัดมาร์ค" (ไอคอน `ListChecks` จาก Lucide-Svelte)
            *   **Action:** นำทางไปหน้า "หน้าจัดการบรรทัดมาร์ค" (`/translator/manage-marks/[novel_slug]`)
*   **Tech Stack:** Supabase (query `novels`, `chapters`, `mark_lines`), Skeleton UI (Card, Progress Bar, Button), Lucide-Svelte

---

#### 8.4.1 หน้าจัดการบรรทัดมาร์ค (Mark Line Management Page - `/translator/manage-marks/[novel_slug]`)

*   **การดึงข้อมูล:** ใช้ `novel_slug` เพื่อ query `novels`, `chapters`, และ `mark_lines` (สำหรับ `user_id` ปัจจุบัน)
*   **UI:**
    *   **หัวเรื่อง:** "จัดการบรรทัดมาร์คสำหรับ: [ชื่อนิยาย]"
    *   **ส่วนควบคุมหลัก:**
        *   ปุ่ม "เลือกตอนเพื่อคัดลอก/ดาวน์โหลด" (Toggle Switch หรือ Button)
            *   **Action:** Toggle โหมดเลือก (แสดง/ซ่อน Checkbox หน้าแต่ละตอน และ Batch Actions Toolbar)
    *   **การแสดงผลสารบัญตอน (Accordion):**
        *   **UI:** เหมือนสารบัญในหน้าข้อมูลนิยาย แต่เน้นข้อมูลการมาร์ค
        *   **หัวกลุ่ม (Accordion Item Header):** ชื่อกลุ่ม (เช่น "ตอนที่ 1-50"), (ถ้าโหมดเลือกเปิด) `Checkbox` "เลือกทั้งหมดในกลุ่ม"
        *   **รายการตอน:**
            *   (ถ้าโหมดเลือกเปิด) `Checkbox` เลือกตอน
            *   เลขลำดับตอน, ชื่อตอน
            *   จำนวนบรรทัดมาร์ค: "[N] บรรทัด" (จาก `COUNT(mark_lines) WHERE chapter_id = ... AND user_id = ...`)
            *   สถานะมาร์ค: ไอคอน `CheckCircle2` (มาร์คครบทุกบรรทัด - สมมติว่ามีข้อมูลจำนวนบรรทัดทั้งหมดของตอน) หรือ `Circle` (ยังไม่ครบ/ยังไม่เริ่ม) หรือ `TrendingUp` (กำลังดำเนินการ)
            *   ปุ่ม "คัดลอกมาร์คตอนนี้": (ไอคอน `Copy` จาก Lucide-Svelte)
                *   **Action:** Query `mark_lines.line_number` สำหรับตอนนี้, คัดลอกไป Clipboard (เช่น "1, 5, 10-15")
                *   **Toast:** `"คัดลอกมาร์คของตอน '[ชื่อตอน]' แล้ว"`
    *   **Batch Actions Toolbar (แสดงเมื่อเลือกตอนอย่างน้อย 1 ตอน และโหมดเลือกเปิด):**
        *   **UI:** แถบเครื่องมือลอยตัว หรืออยู่ด้านบน/ล่างของรายการ
        *   ปุ่ม "คัดลอกบรรทัดมาร์คของตอนที่เลือก":
            *   **Action:** รวบรวม `mark_lines.line_number` จากทุกตอนที่เลือก คัดลอกไป Clipboard
            *   **Toast:** `"คัดลอกมาร์คของ X ตอนที่เลือกแล้ว"`
        *   ปุ่ม "ดาวน์โหลดไฟล์บรรทัดมาร์คของตอนที่เลือก":
            *   **Action:** เปิด Modal ตัวเลือก:
                *   "รวมเป็นไฟล์ .txt ไฟล์เดียว (แต่ละตอนคั่นด้วยชื่อตอน)"
                *   "แยกเป็นหลายไฟล์ .txt (อยู่ใน .zip)"
            *   **ดาวน์โหลด:** สร้างไฟล์ .txt หรือ .zip แล้วให้ User ดาวน์โหลด
            *   **Toast:** `"กำลังเตรียมไฟล์ดาวน์โหลด..."` และ `"ดาวน์โหลดไฟล์มาร์คสำเร็จ"`
*   **Tech Stack:** SvelteKit, Skeleton UI (Accordion, Checkbox, Button, Modal), Lucide-Svelte, Supabase (query `chapters`, `mark_lines`), JS Logic (สร้างไฟล์/ZIP)

---

### 8.5 หน้าอ่านนิยายและโหมดแปล (Reading & Translator Mode UI - `/novels/[novel_slug]/chapter/[chapter_number]`)

*   **การดึงข้อมูล:** `novel_slug` และ `chapter_number` เพื่อ query `chapters.content_original`. ตรวจสอบ `localStorage` หรือ `user_settings` สำหรับสถานะ "โหมดนักแปล" ของนิยายนี้สำหรับ User ปัจจุบัน.
*   **การแสดงผลเนื้อหา (Content Display):**
    *   **ชื่อตอน (`chapters.title`):** แสดงชัดเจนกลางบนของพื้นที่เนื้อหา
    *   **เนื้อหานิยาย (`chapters.content_original`):**
        *   **การย่อหน้า (Indentation):** `text-indent` สำหรับทุกย่อหน้า
        *   **PC Layout:** เนื้อหาใน Container ความกว้างจำกัด, จัดกลางจอ
        *   **Mobile Layout:** เนื้อหาเต็มความกว้างจอ (Edge-to-edge)
        *   **No Horizontal Scroll**
    *   **ปุ่ม "Arrow Up" (กลับไปด้านบนสุด):** (ไอคอน `ArrowUpCircle` จาก Lucide-Svelte)
        *   **UI:** กึ่งโปร่งแสง, มุมขวาล่าง, แสดงเมื่อ Scroll ลงมาระยะหนึ่ง
    *   **Keyboard Controls (PC Only):**
        *   `ArrowLeft`: ตอนก่อนหน้า
        *   `ArrowRight`: ตอนถัดไป
        *   `Space Bar`: Scroll down
*   **โหมดการอ่าน (Member/Translator เมื่อ "โหมดนักแปล" ปิด):**
    *   **แถบเครื่องมือหลักด้านบน (Top Reading Toolbar - Skeleton UI App Bar, แสดงเสมอ):**
        *   **PC Items (ซ้ายไปขวา):**
            *   "สารบัญ" (ไอคอน `List`): เปิด Modal สารบัญ (คล้ายในหน้าข้อมูลนิยายแต่แสดงใน Modal)
            *   "ปรับแต่งการแสดงผล (Aa)" (ไอคอน `Settings2`): เปิด Popover (Skeleton UI) ตั้งค่า: ขนาดฟอนต์, ระยะห่างบรรทัด, สีพื้นหลังหน้าอ่าน (ขาว, ครีม, เทา, ดำ)
            *   "บุ๊คมาร์ค" (ไอคอน `Bookmark`): Toggle `user_bookmarks`
                *   **Toast (เพิ่ม):** `"บุ๊คมาร์คตอน '[ชื่อตอน]' แล้ว"`
                *   **Toast (ลบ):** `"ยกเลิกบุ๊คมาร์คตอน '[ชื่อตอน]' แล้ว"`
            *   "สลับธีมเว็บไซต์" (ไอคอน `Sun`/`Moon`)
            *   "โหมดอ่านต่อเนื่อง" (ไอคอน `Infinity`): Toggle.
                *   **UI:** เปิด: ไอคอนมี Animation สีรุ้งช้าๆ. ปิด: ไอคอนสีตาม Theme.
                *   **Toast (เปิด):** `"เปิดโหมดอ่านต่อเนื่อง"`
                *   **Toast (ปิด):** `"ปิดโหมดอ่านต่อเนื่อง"`
            *   ปุ่มนำทาง: `< ตอนก่อนหน้า` (ไอคอน `ChevronLeft`), `ตอนต่อไป >` (ไอคอน `ChevronRight`)
        *   **Mobile Items (Top Reading Toolbar):** คล้าย PC แต่ปุ่มนำทางตอนจะไปอยู่แถบด้านล่าง
            *   ไอคอน: `List`, `Settings2`, `Bookmark`, `Sun`/`Moon`, `Infinity`
    *   **การนำทางตอน (Chapter Navigation - เฉพาะ Mobile):**
        *   **แถบเครื่องมือด้านล่าง (Bottom Navigation Toolbar - Skeleton UI App Bar):**
            *   **UI:** ปรากฏเมื่อ "แตะ" เนื้อหา, ซ่อนเมื่อ Scroll หรือแตะอีกครั้ง
            *   **Items:** `< ตอนก่อนหน้า` (ไอคอน `ChevronLeft`), `ตอนต่อไป >` (ไอคอน `ChevronRight`)
    *   **เมื่อเลื่อนถึงท้ายบท:**
        *   **(ถ้า "โหมดอ่านต่อเนื่อง" ปิด):** แสดงปุ่มขนาดใหญ่ "ตอนต่อไป: [ชื่อตอนถัดไป]". คลิกแล้วรีเฟรชหน้าโหลดตอนใหม่.
        *   **(ถ้า "โหมดอ่านต่อเนื่อง" เปิด):** โหลดเนื้อหาตอนถัดไปอัตโนมัติ (Infinite Scroll). ชื่อตอนด้านบนอัปเดตตาม.
*   **โหมดนักแปล (Translator Mode - เมื่อ "โหมดนักแปล" สำหรับนิยายนี้เปิด):**
    *   **แถบเครื่องมือหลักด้านบน (Top Reading Toolbar):**
        *   **PC Items:** เหมือนโหมดอ่าน + ปุ่ม "คัดลอกมาร์คทั้งหมด" (ไอคอน `CopyCheck`)
            *   **Action:** คัดลอก `mark_lines.line_number` ของตอนนี้ที่ `user_id` ปัจจุบันมาร์คไว้
            *   **Toast:** `"คัดลอกมาร์คทั้งหมดของตอนนี้แล้ว"`
        *   **Mobile Items:** เหมือนโหมดอ่าน + ปุ่ม "คัดลอกมาร์คทั้งหมด" (ไอคอน `CopyCheck`)
    *   **การแสดงผลเนื้อหา:**
        *   **แถบเลขบรรทัด (Line Number Gutter):** ซ้ายสุดของเนื้อหา. สีพื้นหลัง/ตัวอักษรปรับตาม Theme (ตัวเลขสีไม่เด่นเท่าเนื้อหาหลัก)
        *   **เนื้อหาต้นฉบับ (`chapters.content_original`):** ถัดจากแถบเลขบรรทัด, ย่อหน้าทุกย่อหน้า
        *   **Checkbox สำหรับมาร์ค:** (Skeleton UI Checkbox) ขวาสุดของแต่ละบรรทัด (แต่ละ `div` หรือ `p` ของเนื้อหา)
    *   **MarkLine Interaction:**
        *   **คลิก Checkbox:** มาร์ค/ยกเลิกมาร์คบรรทัดนั้นๆ (`INSERT`/`DELETE` ใน `mark_lines`)
            *   **Toast (มาร์ค):** `"มาร์คบรรทัดที่ X แล้ว"`
            *   **Toast (ยกเลิก):** `"ยกเลิกมาร์คบรรทัดที่ X แล้ว"`
        *   **ดับเบิลคลิกที่บรรทัดเนื้อหา:**
            1.  ไฮไลท์บรรทัดชั่วครู่ (CSS animation)
            2.  คัดลอกข้อความต้นฉบับของบรรทัดนั้นไป Clipboard
            3.  ทำการ "มาร์ค" บรรทัดนั้นอัตโนมัติ (`INSERT` ใน `mark_lines`)
            *   **Toast:** `"คัดลอกและมาร์คบรรทัดที่ X แล้ว"`
    *   **เมื่อเลื่อนถึงท้ายบทในโหมดนักแปล:**
        *   **ส่วนสรุปข้อมูลการมาร์ค:**
            *   ข้อความ: "มีมาร์คทั้งหมด: [จำนวน `mark_lines`] บรรทัด"
            *   ข้อความ: "บรรทัดที่มาร์คได้แก่: [เลขบรรทัด 1], [เลขบรรทัด 2], ..." (แสดง 5 บรรทัดแรก + ปุ่ม "ดูทั้งหมด" เปิด Modal แสดงรายการทั้งหมด)
            *   ปุ่ม "ล้างมาร์คทั้งหมดของตอนนี้": (ไอคอน `Eraser` จาก Lucide-Svelte)
                *   **Action:** Modal ยืนยัน. `DELETE FROM mark_lines WHERE chapter_id = ... AND user_id = ...`
                *   **Toast:** `"ล้างมาร์คทั้งหมดของตอนนี้แล้ว"`
            *   ปุ่ม "คัดลอกมาร์คทั้งหมดของตอนนี้": (ไอคอน `CopyCheck` จาก Lucide-Svelte)
                *   **Action:** เหมือนปุ่มบน Toolbar
                *   **Toast:** `"คัดลอกมาร์คทั้งหมดของตอนนี้แล้ว"`
*   **Tech Stack:** Svelte store (จัดการสถานะ UI), JavaScript/Svelte logic, Skeleton UI (App Bar, Modal, Popover, Checkbox, Button), Supabase (query `chapters`, CRUD `user_bookmarks`, CRUD `mark_lines`)

---

### 8.6 แดชบอร์ดผู้ดูแลระบบ (Admin Dashboard - `/admin/dashboard`)

*   **Layout:** Sidebar ซ้าย (Skeleton UI Navigation Rail หรือ List), พื้นที่แสดงเนื้อหาหลักขวา
*   **Sidebar นำทาง:**
    *   "ภาพรวม" (`/admin/dashboard/overview`)
    *   "จัดการผู้ใช้" (`/admin/dashboard/users`)
    *   "จัดการหมวดหมู่" (`/admin/dashboard/categories`)
    *   "จัดการประกาศ" (`/admin/dashboard/announcements`)
    *   "จัดการเว็บไซต์" (`/admin/dashboard/site-settings`)
    *   "จัดการเวอร์ชัน" (`/admin/dashboard/versions`)

---

#### 8.6.1 หน้าภาพรวมแดชบอร์ด (Dashboard Overview Page - `/admin/dashboard/overview`)

*   **หัวเรื่อง:** "ภาพรวมแดชบอร์ดผู้ดูแลระบบ"
*   **UI:** แสดงการ์ดข้อมูล (Skeleton UI Card)
    *   จำนวนนิยายทั้งหมด (จาก `COUNT(novels)`)
    *   จำนวนผู้ใช้ทั้งหมด (จาก `COUNT(profiles)`)
    *   จำนวนนักแปล (จาก `COUNT(profiles WHERE role_id = 'Translator')`)
    *   จำนวนประกาศที่ใช้งานอยู่ (จาก `COUNT(announcements WHERE is_active = true)`)
*   **Tech Stack:** SvelteKit (Routing), Skeleton UI (Card), Supabase (Aggregate queries)

---

#### 8.6.2 หน้าจัดการผู้ใช้ (User Management Page - `/admin/dashboard/users`)

*   **หัวเรื่อง:** "จัดการผู้ใช้ทั้งหมด"
*   **ส่วนควบคุม:**
    *   ช่องค้นหาผู้ใช้ (Input, กรอง `profiles.display_name` หรือ `auth.users.email` real-time)
    *   แสดงจำนวนผู้ใช้ทั้งหมด / ตามบทบาท
*   **ตารางผู้ใช้ (Skeleton UI Table):**
    *   **คอลัมน์:** #, Avatar (`profiles.avatar_url`), Email (`auth.users.email`), ชื่อที่แสดง (`profiles.display_name`), สิทธิ์ (`roles.name`), วันที่สมัคร (`profiles.created_at`), วันที่สิทธิ์หมดอายุ (`profiles.role_expiry_date`), สถานะ (`profiles.is_suspended`), ปุ่ม "จัดการ" (ไอคอน `Settings`, เปิด Modal จัดการผู้ใช้)
    *   **การเรียง:** คลิกหัวคอลัมน์เพื่อเรียงข้อมูล
*   **Modal จัดการผู้ใช้:**
    *   **UI:**
        *   Avatar (แสดง + ปุ่มเปลี่ยนรูป - Admin อัปโหลดให้)
        *   Email (Read-only)
        *   ชื่อที่แสดง (`Input text` + แก้ไข)
        *   สิทธิ์ (`Select` Dropdown ดึงจาก `roles` + แก้ไข)
        *   วันที่สมัคร (Read-only)
        *   วันที่สิทธิ์หมดอายุ (`Date Picker` + แก้ไข, ปุ่มช่วย: "+1 เดือน", "+3 เดือน", "+6 เดือน", "ไม่มีวันหมดอายุ" (ตั้งเป็น NULL))
        *   ระงับบัญชี (`Toggle Switch` เปลี่ยน `profiles.is_suspended`)
    *   **ปุ่ม:** "บันทึกการเปลี่ยนแปลง", "ยกเลิก"
        *   **Action (บันทึก):** `UPDATE profiles SET ... WHERE id = ...`
        *   **Toast:** `"อัปเดตข้อมูลผู้ใช้ [ชื่อผู้ใช้] สำเร็จแล้ว"`
*   **Tech Stack:** SvelteKit, Skeleton UI (Table, Input, Select, Date Picker, Toggle, Modal), Supabase (CRUD `profiles`, query `auth.users`, `roles`)

---

#### 8.6.3 หน้าจัดการหมวดหมู่นิยาย (Category Management Page - `/admin/dashboard/categories`)

*   **หัวเรื่อง:** "จัดการหมวดหมู่นิยาย"
*   **ส่วนเพิ่ม:**
    *   ปุ่ม "เพิ่มหมวดหมู่ใหม่" (ไอคอน `PlusSquare`)
        *   **Action:** เปิด Modal:
            *   **ฟิลด์:** ชื่อหมวดหมู่ (`categories.name`), Slug (`categories.slug` - auto-generate จากชื่อ และแก้ไขได้, เช็ค unique), คำอธิบาย (`categories.description` - Textarea)
            *   **ปุ่ม:** "บันทึก", "ยกเลิก"
            *   **Action (บันทึก):** `INSERT INTO categories ...`
            *   **Toast:** `"เพิ่มหมวดหมู่ '[ชื่อหมวดหมู่]' สำเร็จแล้ว"`
*   **รายการหมวดหมู่ (Accordion หรือ Table with Expandable Rows):**
    *   **แถวหลัก:** ชื่อหมวดหมู่, จำนวนนิยายในหมวด (จาก `COUNT(novels WHERE category_id = ...)`), ปุ่ม "แก้ไขชื่อ/Slug/คำอธิบาย" (ไอคอน `Edit`, เปิด Modal คล้ายเพิ่มหมวดหมู่แต่เป็นการแก้ไข), ปุ่ม "ลบ" (ไอคอน `Trash`, สีแดง)
        *   **Action (ลบ):** เปิด Modal ยืนยัน "การลบหมวดหมู่ '[ชื่อหมวดหมู่]' จะทำให้นิยาย X เรื่องในหมวดนี้ถูกตั้งเป็น 'ไม่มีหมวดหมู่' คุณต้องการดำเนินการต่อหรือไม่?" หรือ "เลือกหมวดหมู่ใหม่สำหรับนิยายในหมวดนี้:" (Dropdown `categories` + ตัวเลือก "ไม่มีหมวดหมู่")
            *   **ยืนยัน:** `UPDATE novels SET category_id = NULL (หรือ new_category_id) WHERE category_id = ...; DELETE FROM categories WHERE id = ...`
            *   **Toast:** `"ลบหมวดหมู่ '[ชื่อหมวดหมู่]' และย้ายนิยายเรียบร้อยแล้ว"`
    *   **(ถ้ามี) ส่วนขยาย:** รายการนิยายในหมวดนั้นๆ (ชื่อนิยาย + ลิงก์ไปหน้าข้อมูลนิยาย)
*   **Tech Stack:** SvelteKit, Skeleton UI (Table/Accordion, Button, Modal, Input, Textarea), Supabase (CRUD `categories`, UPDATE `novels`)

---

#### 8.6.4 หน้าจัดการประกาศ (Announcement Management Page - `/admin/dashboard/announcements`)

*   **หัวเรื่อง:** "จัดการประกาศ"
*   **UI:**
    *   ปุ่ม "สร้างประกาศใหม่" (ไอคอน `PlusSquare`)
        *   **Action:** เปิด Modal สร้าง/แก้ไขประกาศ
    *   **ตารางประกาศ (Skeleton UI Table):**
        *   **คอลัมน์:** หัวข้อ (`announcements.title`), สถานะ (`announcements.is_active` - แสดงเป็น Badge เขียว/เทา), หน้าที่แสดง (สรุปจาก `announcements.target_pages`), กลุ่มผู้รับ (สรุปจาก `announcements.target_roles`), วันที่เริ่ม (`announcements.start_date`), วันที่สิ้นสุด (`announcements.end_date`), ปุ่มดำเนินการ: "แก้ไข" (ไอคอน `Edit`), "ลบ" (ไอคอน `Trash`), "สลับสถานะ" (Toggle Switch หรือปุ่ม)
            *   **Action (ลบ):** Modal ยืนยัน. `DELETE FROM announcements WHERE id = ...`
                *   **Toast:** `"ลบประกาศ '[หัวข้อประกาศ]' สำเร็จแล้ว"`
            *   **Action (สลับสถานะ):** `UPDATE announcements SET is_active = NOT is_active WHERE id = ...`
                *   **Toast:** `"เปลี่ยนสถานะประกาศ '[หัวข้อประกาศ]' เป็น '[ใช้งาน/ไม่ใช้งาน]' แล้ว"`
*   **Modal สร้าง/แก้ไขประกาศ:**
    *   **ฟิลด์:**
        *   หัวข้อ (`announcements.title` - Input text)
        *   เนื้อหา (`announcements.content` - Textarea รองรับ Markdown, มี Preview)
        *   วันที่เริ่มแสดง (`Date Picker` + `Time Picker` - `announcements.start_date`)
        *   วันที่สิ้นสุดการแสดง (`Date Picker` + `Time Picker` - `announcements.end_date`)
        *   หน้าที่แสดง (`Checkbox Group`: "หน้าแรก", "หน้านิยาย (ทุกเรื่อง)", "หน้านิยาย (เฉพาะเรื่อง)" (ถ้าเลือกอันนี้มี Input ให้ใส่ novel_slug), ...)
        *   กลุ่มผู้รับ (`Checkbox Group` ดึงจาก `roles.name`: "ทุกคน", "Member", "Translator", "Admin")
        *   สถานะ "ใช้งาน" (`Toggle Switch` - `announcements.is_active`)
    *   **ปุ่ม:** "บันทึกประกาศ", "ยกเลิก"
        *   **Action (บันทึก):** `INSERT` หรือ `UPDATE announcements ...`
        *   **Toast (สร้าง):** `"สร้างประกาศ '[หัวข้อประกาศ]' สำเร็จแล้ว"`
        *   **Toast (แก้ไข):** `"อัปเดตประกาศ '[หัวข้อประกาศ]' สำเร็จแล้ว"`
*   **Tech Stack:** SvelteKit, Skeleton UI (Table, Button, Modal, Input, Textarea, Date Picker, Checkbox, Toggle), Supabase (CRUD `announcements`)

---

#### 8.6.5 หน้าจัดการเว็บไซต์ (Site Management Page - `/admin/dashboard/site-settings`)

*   **หัวเรื่อง:** "จัดการเว็บไซต์"
*   **Layout:** หน้าเพจปกติภายใน Admin Dashboard, แบ่งเป็น Section ชัดเจน
*   **UI (ดึง/บันทึกข้อมูลจาก `site_settings` table):**
    *   **ส่วนทั่วไป (General Settings):**
        *   ชื่อเว็บไซต์: `Input text` (key: `site_name`)
        *   คำอธิบายเว็บไซต์ (SEO): `Textarea` (key: `site_description`)
        *   โลโก้เว็บไซต์: `File Input` (อัปโหลดไป `site_assets` bucket, เก็บ URL ใน key: `site_logo_url`), Preview รูปปัจจุบัน
            *   **Toast (อัปโหลดสำเร็จ):** `"อัปเดตโลโก้เว็บไซต์แล้ว"`
        *   Favicon: `File Input` (อัปโหลดไป `site_assets`, เก็บ URL ใน key: `site_favicon_url`), Preview รูปปัจจุบัน
            *   **Toast (อัปโหลดสำเร็จ):** `"อัปเดต Favicon แล้ว"`
    *   **ส่วนการลงทะเบียนและการเข้าสู่ระบบ (Registration & Login Settings):**
        *   เปิด/ปิดการลงทะเบียนใหม่: `Toggle Switch` (key: `allow_new_registrations`, value: true/false)
        *   เปิด/ปิดการเข้าสู่ระบบด้วย Google: `Toggle Switch` (key: `allow_google_login`, value: true/false)
    *   **ส่วนเนื้อหา (Content Settings):**
        *   จำนวนรายการต่อหน้าเริ่มต้น (Pagination): `Input number` (key: `default_items_per_page`)
        *   การตั้งค่าเริ่มต้นสำหรับนิยายใหม่ (เมื่อ Admin สร้างจาก Backend หรือค่า Fallback):
            *   สถานะตอนเริ่มต้น: `Select` ("เผยแพร่", "ฉบับร่าง") (key: `new_novel_default_chapter_status`)
            *   สิทธิ์เข้าถึงตอนเริ่มต้น: `Checkbox Group` ("ทุกคน", "เฉพาะสมาชิก", "เฉพาะนักแปล") (key: `new_novel_default_chapter_access_level`)
    *   **ปุ่มดำเนินการ:** "บันทึกการตั้งค่าทั้งหมด"
        *   **Action:** `UPDATE site_settings SET value = ... WHERE key = ...` สำหรับทุก key ที่แก้ไข
        *   **Toast:** `"บันทึกการตั้งค่าเว็บไซต์เรียบร้อยแล้ว"`
*   **Tech Stack:** SvelteKit, Skeleton UI (Input, Textarea, File Input, Toggle, Select, Checkbox, Button), Supabase (CRUD `site_settings`), Supabase Storage (`site_assets`)

---

#### 8.6.6 หน้าจัดการเวอร์ชัน (Version Management Page - `/admin/dashboard/versions`)

*   **หัวเรื่อง:** "จัดการเวอร์ชัน"
*   **Layout:** `Tabs` (Skeleton UI) แยกประเภทเวอร์ชัน
*   **Tab 1: เวอร์ชันเว็บไซต์ / Changelog (`site_changelogs` table):**
    *   **UI:**
        *   ปุ่ม "เพิ่ม Changelog ใหม่" (ไอคอน `PlusSquare`)
            *   **Action:** เปิด Modal เพิ่ม/แก้ไข Changelog:
                *   **ฟิลด์:** หมายเลขเวอร์ชัน (`site_changelogs.version_number` - text), วันที่เผยแพร่ (`site_changelogs.release_date` - Date Picker), สรุปการเปลี่ยนแปลง (`site_changelogs.summary` - Textarea รองรับ Markdown, มี Preview), สถานะ (`site_changelogs.is_published` - Toggle "เผยแพร่/ฉบับร่าง")
                *   **ปุ่ม:** "บันทึก", "ยกเลิก"
                *   **Toast (สร้าง):** `"เพิ่ม Changelog เวอร์ชัน [หมายเลขเวอร์ชัน] สำเร็จ"`
                *   **Toast (แก้ไข):** `"อัปเดต Changelog เวอร์ชัน [หมายเลขเวอร์ชัน] สำเร็จ"`
        *   **ตาราง Changelog:**
            *   **คอลัมน์:** หมายเลขเวอร์ชัน, วันที่เผยแพร่, สรุป (แสดงส่วนหนึ่ง), สถานะ (Badge), ปุ่ม: "แก้ไข", "ลบ" (Modal ยืนยัน, Toast `"ลบ Changelog เวอร์ชัน [หมายเลขเวอร์ชัน] สำเร็จ"`)
*   **Tab 2: เวอร์ชันนิยายและตอน (`novel_versions`, `chapter_versions` tables):**
    *   **ส่วนค้นหานิยาย:** `Input text` ค้นหาตามชื่อเรื่องนิยาย
    *   **รายการนิยาย (หลังค้นหา หรือแสดงทั้งหมดแบบแบ่งหน้า):**
        *   **แต่ละรายการ:** ชื่อนิยาย, ปุ่ม "ดูประวัติเวอร์ชันนิยาย" (ไอคอน `History`), ปุ่ม "ดูประวัติเวอร์ชันตอน" (ไอคอน `FileClock`)
    *   **เมื่อคลิก "ดูประวัติเวอร์ชันนิยาย" (สำหรับนิยายที่เลือก):**
        *   **แสดง Modal หรือ Section ใหม่:**
            *   **หัวเรื่อง:** "ประวัติเวอร์ชันข้อมูลของนิยาย: [ชื่อนิยาย]"
            *   **ตาราง `novel_versions`:** เวอร์ชัน (`version_number`), วันที่ (`created_at`), ผู้แก้ไข (จาก `created_by` JOIN `profiles.display_name`), หมายเหตุ (`notes`), ปุ่ม: "ดูรายละเอียด" (เปิด Modal แสดง `metadata_snapshot` แบบสวยงาม), "กู้คืนเวอร์ชันนี้"
                *   **Action (กู้คืน):** Modal ยืนยัน "คุณต้องการกู้คืนข้อมูลนิยายเป็นเวอร์ชัน [X] หรือไม่? ข้อมูลปัจจุบันจะถูกเก็บเป็นเวอร์ชันใหม่"
                    *   **ยืนยัน:** อ่าน `metadata_snapshot` ของเวอร์ชันที่เลือก, `UPDATE novels SET ...` ด้วยข้อมูลนั้น, สร้าง `novel_versions` record ใหม่สำหรับข้อมูล *ก่อน* การกู้คืน.
                    *   **Toast:** `"กู้คืนข้อมูลนิยายเป็นเวอร์ชัน [X] สำเร็จแล้ว"`
    *   **เมื่อคลิก "ดูประวัติเวอร์ชันตอน" (สำหรับนิยายที่เลือก):**
        *   **แสดง Modal หรือ Section ใหม่:**
            *   **หัวเรื่อง:** "ประวัติเวอร์ชันเนื้อหาตอนของนิยาย: [ชื่อนิยาย]"
            *   `Select` (Dropdown) เลือกตอนของนิยายนั้น (จาก `chapters` table)
            *   **เมื่อเลือกตอน:**
                *   **ตาราง `chapter_versions` (สำหรับตอนที่เลือก):** เวอร์ชัน (`version_number`), วันที่ (`created_at`), ผู้แก้ไข (จาก `created_by`), หมายเหตุ (`notes`), ปุ่ม: "ดูเนื้อหา" (เปิด Modal แสดง `content_original_snapshot`), "กู้คืนเวอร์ชันนี้"
                    *   **Action (กู้คืน):** Modal ยืนยัน. `UPDATE chapters SET content_original = ...` ด้วย `content_original_snapshot` นั้น, สร้าง `chapter_versions` record ใหม่สำหรับเนื้อหา *ก่อน* การกู้คืน.
                    *   **Toast:** `"กู้คืนเนื้อหาตอน '[ชื่อตอน]' เป็นเวอร์ชัน [X] สำเร็จแล้ว"`
*   **Tech Stack:** Skeleton UI (Tabs, Table, Modal, Input, Textarea, Date Picker, Dropdown, Button), Supabase (CRUD `site_changelogs`, query `novel_versions`, `chapter_versions`, `novels`, `chapters`)

---

### 8.7 หน้าโปรไฟล์ (Profile Page - `/profile`)

*   **การนำทาง:** จากเมนู Dropdown ของ Avatar ผู้ใช้ใน Navbar
*   **หัวเรื่อง:** "โปรไฟล์ของฉัน"
*   **Layout:** หน้าเพจเดียว, ใช้ Card จัดกลุ่มข้อมูล
*   **UI (ดึง/บันทึกข้อมูลจาก `profiles` และ `auth.users`):**
    *   **รูปโปรไฟล์ (Avatar):**
        *   แสดง Avatar ปัจจุบัน (`profiles.avatar_url`) (Skeleton UI Avatar)
        *   ปุ่ม "เปลี่ยนรูปโปรไฟล์" (ไอคอน `Camera` จาก Lucide-Svelte)
            *   **Action:** เปิด `File Input`. เมื่อเลือกรูป, แสดงเครื่องมือ Crop (JS library, บังคับ 1:1).
            *   **บันทึก:** อัปโหลดรูปที่ Crop แล้วไป `avatars` bucket, อัปเดต `profiles.avatar_url`.
            *   **Toast:** `"เปลี่ยนรูปโปรไฟล์สำเร็จแล้ว"`
    *   **ชื่อที่แสดง (Display Name):**
        *   แสดงชื่อปัจจุบัน (`profiles.display_name`)
        *   `Input text` สำหรับแก้ไข + ปุ่ม "บันทึกชื่อ"
            *   **Action:** `UPDATE profiles SET display_name = ... WHERE id = auth.uid()`
            *   **Toast:** `"อัปเดตชื่อที่แสดงเป็น '[ชื่อใหม่]' เรียบร้อยแล้ว"`
    *   **สิทธิ์ผู้ใช้งาน (Role):**
        *   แสดงบทบาทปัจจุบัน (จาก `profiles.role_id` JOIN `roles.name`) ด้วย `Badge` (Skeleton UI)
            *   Member: สีเขียว (`bg-green-500 text-white`)
            *   Translator: สีเหลือง/ทอง (`bg-yellow-500 text-black` หรือ `bg-amber-500`)
            *   Admin: สีแดง (`bg-red-500 text-white`)
        *   (Read-only ส่วนนี้)
    *   **อีเมล (Email):**
        *   แสดงอีเมล (`auth.users.email`) (Read-only)
    *   **เปลี่ยนรหัสผ่าน (Change Password):**
        *   ปุ่ม "เปลี่ยนรหัสผ่าน"
            *   **Action:** เปิด Modal เปลี่ยนรหัสผ่าน:
                *   **ฟิลด์:** รหัสผ่านปัจจุบัน, รหัสผ่านใหม่, ยืนยันรหัสผ่านใหม่ (Felte + Zod validation)
                *   **ปุ่ม:** "บันทึกรหัสผ่านใหม่", "ยกเลิก"
                *   **Action (บันทึก):** ใช้ Supabase Auth `updateUser({ password: newPassword })` (ต้องมีการ re-authenticate หรือใส่ current password ถ้า API ต้องการ)
                *   **Toast (สำเร็จ):** `"เปลี่ยนรหัสผ่านสำเร็จแล้ว กรุณาเข้าสู่ระบบใหม่อีกครั้ง"` (บังคับ Logout และ Redirect ไป Login)
                *   **Toast (ล้มเหลว):** `"ไม่สามารถเปลี่ยนรหัสผ่านได้: [เหตุผล]"`
    *   **ออกจากระบบ (Logout):**
        *   ปุ่ม "ออกจากระบบ" (ไอคอน `LogOut` จาก Lucide-Svelte)
            *   **Action:** เปิด Modal ยืนยัน "คุณต้องการออกจากระบบหรือไม่?"
            *   **ยืนยัน:** `supabase.auth.signOut()`
            *   **Toast:** `"ออกจากระบบแล้ว แล้วพบกันใหม่!"` (Redirect ไปหน้าแรก)
*   **Tech Stack:** SvelteKit, Skeleton UI (Card, Avatar, Input, Button, Modal, Badge), Felte + Zod, Supabase Auth, Supabase DB (`profiles`, `roles`), JS library (Crop)

## 9. Key User Experience Flows & Enhancements

*   **การล็อกอิน:** ผ่าน Modal ไม่ต้องเปลี่ยนหน้า
*   **การนำทาง:** ชัดเจน, Responsive
*   **ประสบการณ์การอ่าน:** ปรับแต่งได้, โหมดอ่านต่อเนื่อง, ไม่มี Horizontal Scroll
*   **เครื่องมือช่วยแปล:** บรรทัดมาร์ค, คัดลอกง่าย
*   **การตอบสนองของระบบ:**
    *   **Empty States:** เมื่อไม่มีข้อมูล (เช่น ไม่มีนิยาย, ไม่มีบุ๊คมาร์ค) ให้แสดงข้อความพร้อมภาพประกอบหรือไอคอนที่สื่อความหมาย และอาจมี Call-to-Action (เช่น ปุ่ม "สร้างนิยายเรื่องแรกของคุณ")
    *   **Loading States:** แสดง Skeleton loaders หรือ Spinners ขณะโหลดข้อมูล
    *   **Error Handling:** (ดูข้อ 10)
*   **การออกแบบ:** สวยงามด้วยสีทอง-เทา, ฟอนต์ Sarabun, ตอบสนองทุกอุปกรณ์
*   **ปุ่ม Arrow Up:** ช่วยเลื่อนกลับไปบนสุดของหน้ายาวๆ
*   **การแจ้งเตือนการกระทำ (Action Confirmations):**
    *   ใช้ Toast notifications (Skeleton UI) แสดงผลมุมล่างขวาของจอ
    *   สีของ Toast สื่อถึงประเภทการแจ้งเตือน: เขียว (สำเร็จ), เหลือง/ส้ม (คำเตือน/ข้อมูล), แดง (ข้อผิดพลาด)
*   **การสร้าง `mark_lines` อัตโนมัติ:** เมื่อสร้างตอนใหม่ (Manual/Batch) ระบบจะสร้าง record ว่างๆ ใน `mark_lines` สำหรับตอนนั้นและ `author_id` ของนิยายโดยอัตโนมัติ เพื่อเตรียมพร้อมให้ User นั้นเริ่มมาร์คได้เลย

## 10. การจัดการข้อผิดพลาดเบื้องต้น (Basic Error Handling)

*   **Client-Side Validation (Felte + Zod):** ตรวจสอบข้อมูลในฟอร์มก่อนส่ง, แสดงข้อผิดพลาดใต้ฟิลด์
*   **Server-Side Validation (SvelteKit actions/loaders, Supabase Edge Functions):** ตรวจสอบข้อมูลอีกครั้งด้วย Zod หรือ Logic ของ Supabase (เช่น RLS, constraints)
*   **User-Facing Error Messages (Toast Notifications):**
    *   ทั่วไป: `Toast: "เกิดข้อผิดพลาดบางอย่าง กรุณาลองใหม่อีกครั้ง"`
    *   เฉพาะเจาะจง (ถ้าเป็นไปได้): `Toast: "ไม่สามารถบันทึกข้อมูลได้: ชื่อเรื่องซ้ำ"`
    *   หลีกเลี่ยง Technical details
*   **Code-Level Error Handling:** `try...catch` ใน SvelteKit `load` / `actions` และ Edge Functions. คืน HTTP status code ที่เหมาะสม.
*   **Error Logging:** `console.error` ระหว่างพัฒนา. Supabase มี Logging ในตัว.

## 11. มาตรการความปลอดภัยเบื้องต้น (Basic Security Measures)

*   **Authentication:** Supabase Auth, เปิด Email Verification.
*   **Authorization (RLS):** เปิดใช้งานสำหรับทุกตารางสำคัญ. กำหนด Policy เข้มงวดตามบทบาทและ ownership.
*   **Input Validation:** Zod ทั้ง Client และ Server. Sanitize HTML output (ถ้าแสดงผล HTML ที่ User ป้อนโดยตรง, เช่น TipTap content ควรมีการ sanitize ก่อนบันทึกหรือก่อนแสดงผล).
*   **HTTPS:** ให้บริการผ่าน HTTPS (Supabase/Hosting จัดการให้)
*   **Environment Variables:** เก็บ Keys ใน `.env.local`, ห้าม commit. ใช้ Service Role Key เฉพาะฝั่ง Server.
*   **Data Backups:** Supabase มีระบบสำรองข้อมูลอัตโนมัติ. พิจารณา Manual Backups เป็นระยะสำหรับข้อมูลสำคัญมากๆ.
*   **Principle of Least Privilege:** ผู้ใช้มีสิทธิ์เท่าที่จำเป็น.
*   **Software Updates:** หมั่นอัปเดต SvelteKit, Supabase client, และ Dependencies อื่นๆ.
