# INKREALM.CO: ข้อกำหนดระบบและคุณสมบัติหลัก (V5.1) - ฉบับปรับปรุง (Next.js, Supabase, Chakra UI, Font Awesome)

เอกสารนี้เป็นข้อกำหนดระบบและคุณสมบัติหลักสำหรับแพลตฟอร์ม INKREALM.CO (V5.1) โดยเน้นฟังก์ชันที่จำเป็นตามการปรับปรุงล่าสุด โดยอ้างอิง Tech Stack ใหม่ (Next.js (React), Supabase, **Chakra UI**, **Font Awesome Icons**) และออกแบบมาเพื่อความสะดวกในการปรับใช้, ทดสอบ, และย้ายไปยังโดเมนต่างๆ

## 1. ลำดับความสำคัญในการพิจารณาและพัฒนา (โดยรวม)

1.  **ความปลอดภัยของระบบและข้อมูล (Security & Data Integrity):** การยืนยันตัวตน, การกำหนดสิทธิ์ (RLS), การป้องกันข้อมูลรั่วไหล, การตรวจสอบข้อมูลนำเข้า, การสำรองข้อมูล
2.  **โครงสร้างฐานข้อมูล (Database Structure):** ความถูกต้อง, ความครบถ้วน, ความสัมพันธ์ของข้อมูล, การรองรับการขยายตัว และการ query ที่มีประสิทธิภาพ (รวมถึงการ denormalize ที่จำเป็นอย่าง `mark_lines`)
3.  **ระบบหลัก (Core Systems):** ระบบยืนยันตัวตน, ระบบจัดการเนื้อหานิยาย (CMS), ระบบการอ่านและบุ๊คมาร์ค, ระบบเครื่องมือสำหรับนักแปล
4.  **ประสบการณ์ผู้ใช้ (User Experience - UX):** การใช้งานง่าย, ความต่อเนื่อง, การตอบสนองของระบบ, การจัดการข้อผิดพลาดที่เป็นมิตร
5.  **การออกแบบส่วนติดต่อผู้ใช้ (User Interface - UI):** ความสวยงาม, การอ่านง่าย, ความสอดคล้อง, Responsive Design (ใช้ **Chakra UI**)
6.  **คุณสมบัติเสริมและส่วนอำนวยความสะดวก (Enhancements & Utilities):** ระบบประกาศ, แดชบอร์ด, การจัดการเว็บไซต์, การจัดการเวอร์ชัน

## 2. ปรัชญาการออกแบบโดยรวม

-   **การแสดงผลหรูหราและอ่านง่าย:**
    -   **โทนสีหลัก:** สีทอง (เช่น `#FFD700`, `#B8860B` หรือโทนใกล้เคียงที่ให้ความรู้สึกหรูหรา – กำหนดใน `theme` ของ Chakra UI)
    -   **สีรอง:** สีเทา (หลายเฉด เช่น `#F0F0F0`, `#CCCCCC`, `#808080`, `#333333` – กำหนดใน `theme` ของ Chakra UI)
    -   **โหมดสว่าง (Light Mode):** เน้นสีทอง, เทาอ่อน, และขาว เพื่อความสบายตา ดูสะอาด และหรูหรา
    -   **โหมดมืด (Dark Mode):** เน้นสีทอง (อาจปรับให้สว่างขึ้นเล็กน้อยเพื่อคอนทราสต์), เทาเข้ม, และดำ เพื่อลดแสงสะท้อนและช่วยให้อ่านง่ายในที่มืด (ใช้ `useColorMode` hook ของ Chakra UI)
    -   **โลโก้:** พิจารณาใช้การไล่ระดับสี (gradient) ที่ผสมผสานสีทองและสีเทาอย่างลงตัว หรือสีทองล้วนบนพื้นหลังที่เหมาะสม เพื่อความโดดเด่นและทันสมัย
    -   **ฟอนต์หลัก:** Sarabun สำหรับการแสดงผลภาษาไทยที่สวยงามและอ่านง่าย; พิจารณาฟอนต์สำรองสำหรับภาษาอังกฤษที่เข้ากัน (เช่น Noto Sans, Open Sans) – กำหนดใน `theme` ของ Chakra UI
-   **เครื่องมือสำหรับนักแปล:** มีฟังก์ชันสนับสนุนการทำงานของนักแปลโดยเฉพาะ เน้นความชัดเจนและประสิทธิภาพ
-   **ใช้งานง่าย ตอบโจทย์ทุกอุปกรณ์:**
    -   UI ใช้งานง่าย (Intuitive) และสอดคล้องกันทั้งระบบ
    -   Responsive Design เพื่อประสบการณ์ที่ดีในทุกขนาดหน้าจอ (ใช้ Style Props และ Layout Components ของ Chakra UI เช่น `Box`, `Flex`, `Grid`)
    -   หน้าอ่านนิยายไม่มี Horizontal Scroll โดยเด็ดขาด
-   **ความยืดหยุ่นและการทดสอบ:** โครงสร้างที่เอื้อต่อการเปลี่ยนแปลงโดเมนและการทดสอบระบบที่ง่ายดาย
-   **การเปลี่ยนธีม:**
    -   การสลับระหว่างโหมดสว่างและโหมดมืดจะต้องราบรื่น (ใช้ `useColorMode` hook ของ Chakra UI)
    -   สีที่แสดงผลในแต่ละโหมดต้องไม่ผิดเพี้ยนจากที่ออกแบบไว้ และมีคอนทราสต์ที่เหมาะสมสำหรับการอ่าน
    -   ควบคุมผ่านองค์ประกอบ UI ที่ชัดเจน (เช่น `IconButton` ของ Chakra UI พร้อมไอคอนพระอาทิตย์/พระจันทร์จาก **Font Awesome** ที่มีการเปลี่ยนแปลงสถานะชัดเจน) และสถานะธีมต้องถูกบันทึกไว้ในการตั้งค่าของผู้ใช้ (`localStorage` และ/หรือ `profiles.user_settings` ใน Supabase)

## 3. Tech Stack (ชุดเทคโนโลยี)

-   **Frontend Framework:** **Next.js (React)**
-   **Styling & UI Component Library:** **Chakra UI**
-   **Form Handling & Validation:** **React Hook Form** และ **Zod**
-   **Icon Library:** **Font Awesome (React components, e.g., `@fortawesome/react-fontawesome`, `@fortawesome/free-solid-svg-icons`, etc.)**
-   **State Management (Global):** **Zustand** (หรือ React Context API ตามความเหมาะสม)
-   **Data Fetching & Server State Management:** **React Query (TanStack Query)**
-   **Backend & Database Platform:** **Supabase**
    -   **Database:** PostgreSQL
    -   **Authentication:** Supabase Auth
    -   **File Storage:** Supabase Storage
    -   **Serverless Functions:** Supabase Edge Functions (หรือ Next.js API Routes ตามความเหมาะสม)
-   **ภาษา:** **TypeScript**
-   **Rich Text Editor:** **TipTap**
-   **Image Cropping:** **`react-image-crop`**
-   **ZIP file generation (Client-side):** **JSZip**
-   **Toast Notifications:** **Chakra UI Toast**

## 4. โครงสร้างฐานข้อมูล (Supabase - PostgreSQL)

ฐานข้อมูลถูกออกแบบมาเพื่อรองรับฟังก์ชันการทำงานหลักของ INKREALM.CO โดยมีการจัดการข้อมูลผู้ใช้, นิยาย, ตอน, บุ๊คมาร์ค, การตั้งค่า, และอื่นๆ

### 4.1 ตารางใน Schema `auth`

-   **`users`**:
    -   **วัตถุประสงค์:** จัดการข้อมูลผู้ใช้หลักสำหรับการยืนยันตัวตน (จัดการโดย Supabase Auth)
    -   **คอลัมน์สำคัญ:**
        -   `id` (UUID, PK): รหัสเฉพาะของผู้ใช้
        -   `email` (VARCHAR): อีเมลผู้ใช้
        -   `encrypted_password` (VARCHAR): รหัสผ่านที่เข้ารหัส
        -   `role` (VARCHAR): บทบาท (อาจไม่ถูกใช้โดยตรงถ้า `profiles.role_id` คือแหล่งข้อมูลหลัก)
        -   `created_at`, `updated_at` (TIMESTAMPTZ)
        -   และคอลัมน์อื่นๆ ที่เกี่ยวข้องกับการยืนยันตัวตน (confirmation tokens, recovery tokens, etc.)
    -   **Constraints:** `users_pkey` (PK on `id`), `users_phone_key` (UNIQUE on `phone`), `users_email_change_confirm_status_check`.

### 4.2 ตารางใน Schema `public`

-   **`roles`**:
    -   **วัตถุประสงค์:** กำหนดบทบาทต่างๆ ภายในระบบ (เช่น Member, Translator, Admin)
    -   **คอลัมน์สำคัญ:**
        -   `id` (INTEGER, PK, Auto-increment): รหัสเฉพาะของบทบาท
        -   `name` (TEXT, NOT NULL, UNIQUE): ชื่อบทบาท (เช่น 'Guest', 'Member', 'Translator', 'Admin')
    -   **Constraints:** `roles_pkey` (PK on `id`), `roles_name_key` (UNIQUE on `name`).

-   **`profiles`**:
    -   **วัตถุประสงค์:** จัดเก็บข้อมูลโปรไฟล์เพิ่มเติมของผู้ใช้ ขยายจาก `auth.users`
    -   **คอลัมน์สำคัญ:**
        -   `id` (UUID, PK, FK to `auth.users.id` ON DELETE CASCADE): รหัสผู้ใช้, เชื่อมโยงกับตาราง `auth.users`
        -   `display_name` (TEXT): ชื่อที่แสดงของผู้ใช้
        -   `avatar_url` (TEXT): URL รูปโปรไฟล์
        -   `role_id` (INTEGER, NOT NULL, FK to `roles.id`): รหัสบทบาทของผู้ใช้ (Default เป็น Member)
        -   `role_expiry_date` (TIMESTAMPTZ): วันหมดอายุของบทบาท (ถ้ามี)
        -   `is_suspended` (BOOLEAN, DEFAULT FALSE): สถานะการระงับบัญชี
        -   `user_settings` (JSONB, DEFAULT '{}'): การตั้งค่าส่วนตัวของผู้ใช้ (เช่น ธีม, การตั้งค่าการอ่าน)
        -   `email` (TEXT): อีเมลของผู้ใช้ (อาจ denormalized มาเพื่อความสะดวก)
        -   `created_at`, `updated_at` (TIMESTAMPTZ, DEFAULT CURRENT_TIMESTAMP)
    -   **Constraints:** `profiles_pkey` (PK on `id`), `profiles_id_fkey` (FK to `auth.users`), `profiles_role_id_fkey` (FK to `roles`).

-   **`categories`**:
    -   **วัตถุประสงค์:** จัดการหมวดหมู่นิยาย
    -   **คอลัมน์สำคัญ:**
        -   `id` (INTEGER, PK, Auto-increment): รหัสเฉพาะของหมวดหมู่
        -   `name` (TEXT, NOT NULL, UNIQUE): ชื่อหมวดหมู่
        -   `slug` (TEXT, NOT NULL, UNIQUE): Slug สำหรับ URL ที่เป็นมิตร
        -   `description` (TEXT): คำอธิบายหมวดหมู่
        -   `created_at`, `updated_at` (TIMESTAMPTZ, DEFAULT CURRENT_TIMESTAMP)
    -   **Constraints:** `categories_pkey` (PK on `id`), `categories_name_key` (UNIQUE on `name`), `categories_slug_key` (UNIQUE on `slug`).

-   **`novels`**:
    -   **วัตถุประสงค์:** จัดเก็บข้อมูลหลักของนิยายแต่ละเรื่อง
    -   **คอลัมน์สำคัญ:**
        -   `id` (UUID, PK, DEFAULT `gen_random_uuid()`): รหัสเฉพาะของนิยาย
        -   `title` (TEXT, NOT NULL): ชื่อเรื่องนิยาย
        -   `slug` (TEXT, NOT NULL, UNIQUE): Slug สำหรับ URL
        -   `synopsis` (TEXT): คำโปรยเรื่องย่อ
        -   `cover_image_url` (TEXT): URL รูปปกนิยาย
        -   `category_id` (INTEGER, FK to `categories.id` ON DELETE SET NULL): รหัสหมวดหมู่
        -   `original_language` (TEXT, NOT NULL): ภาษาต้นฉบับ (เช่น 'zh', 'en', 'th')
        -   `status` (TEXT, DEFAULT 'ongoing'): สถานะนิยาย (เช่น 'ongoing', 'completed', 'dropped')
        -   `visibility` (JSONB, DEFAULT '["everyone"]'): สิทธิ์การมองเห็นนิยาย (เช่น 'everyone', 'members', 'translators')
        -   `default_chapter_status` (TEXT, DEFAULT 'published'): สถานะเริ่มต้นของตอนใหม่
        -   `default_chapter_access` (JSONB, DEFAULT '["everyone"]'): สิทธิ์การเข้าถึงเริ่มต้นของตอนใหม่
        -   `created_by` (UUID, NOT NULL, FK to `profiles.id`): ผู้สร้างนิยาย
        -   `total_chapters` (INTEGER, DEFAULT 0): จำนวนตอนทั้งหมด
        -   `last_chapter_updated_at` (TIMESTAMPTZ): เวลาอัปเดตตอนล่าสุด
        -   `created_at`, `updated_at` (TIMESTAMPTZ, DEFAULT CURRENT_TIMESTAMP)
    -   **Constraints:** `novels_pkey` (PK on `id`), `novels_slug_key` (UNIQUE on `slug`), `novels_category_id_fkey` (FK to `categories`), `novels_created_by_fkey` (FK to `profiles`).

-   **`chapters`**:
    -   **วัตถุประสงค์:** จัดเก็บข้อมูลของแต่ละตอนในนิยาย
    -   **คอลัมน์สำคัญ:**
        -   `id` (UUID, PK, DEFAULT `gen_random_uuid()`): รหัสเฉพาะของตอน
        -   `novel_id` (UUID, NOT NULL, FK to `novels.id` ON DELETE CASCADE): รหัสนิยายที่ตอนนี้สังกัด
        -   `chapter_number` (TEXT, NOT NULL): หมายเลขตอน (อาจมี padding เช่น '001', '123.5')
        -   `title` (TEXT, NOT NULL): ชื่อตอน
        -   `content_original` (TEXT): เนื้อหาต้นฉบับของตอน (อาจเป็น HTML จาก Rich Text Editor)
        -   `status` (TEXT, DEFAULT 'published'): สถานะตอน (เช่น 'published', 'draft')
        -   `access_level` (JSONB, DEFAULT '["everyone"]'): สิทธิ์การเข้าถึงตอน
        -   `created_by` (UUID, NOT NULL, FK to `profiles.id`): ผู้สร้างตอน
        -   `word_count` (INTEGER): จำนวนคำในตอนนี้
        -   `created_at`, `updated_at` (TIMESTAMPTZ, DEFAULT CURRENT_TIMESTAMP)
    -   **Constraints:** `chapters_pkey` (PK on `id`), `chapters_novel_id_fkey` (FK to `novels`), `chapters_created_by_fkey` (FK to `profiles`), `chapters_novel_id_chapter_number_key` (UNIQUE on `novel_id`, `chapter_number`).

-   **`user_bookmarks`**:
    -   **วัตถุประสงค์:** จัดเก็บบุ๊คมาร์คที่ผู้ใช้อ่านค้างไว้ในแต่ละตอน
    -   **คอลัมน์สำคัญ:**
        -   `id` (INTEGER, PK, Auto-increment): รหัสเฉพาะของบุ๊คมาร์ค
        -   `user_id` (UUID, NOT NULL, FK to `profiles.id` ON DELETE CASCADE): รหัสผู้ใช้ที่บุ๊คมาร์ค
        -   `chapter_id` (UUID, NOT NULL, FK to `chapters.id` ON DELETE CASCADE): รหัสตอนที่ถูกบุ๊คมาร์ค
        -   `novel_id` (UUID, NOT NULL, FK to `novels.id` ON DELETE CASCADE): รหัสนิยาย (เพื่อความสะดวกในการ query)
        -   `bookmarked_at` (TIMESTAMPTZ, DEFAULT CURRENT_TIMESTAMP): เวลาที่บุ๊คมาร์ค
    -   **Constraints:** `user_bookmarks_pkey` (PK on `id`), `user_bookmarks_user_id_fkey`, `user_bookmarks_chapter_id_fkey`, `user_bookmarks_novel_id_fkey`, `user_bookmarks_user_id_chapter_id_key` (UNIQUE on `user_id`, `chapter_id`).

-   **`user_chapter_reads`**:
    -   **วัตถุประสงค์:** บันทึกว่าผู้ใช้อ่านตอนไหนไปแล้วบ้าง
    -   **คอลัมน์สำคัญ:**
        -   `user_id` (UUID, PK, FK to `profiles.id` ON DELETE CASCADE): รหัสผู้ใช้
        -   `chapter_id` (UUID, PK, FK to `chapters.id` ON DELETE CASCADE): รหัสตอน
        -   `read_at` (TIMESTAMPTZ, DEFAULT CURRENT_TIMESTAMP): เวลาที่อ่าน
    -   **Constraints:** `user_chapter_reads_pkey` (PK on (`user_id`, `chapter_id`)), `user_chapter_reads_user_id_fkey`, `user_chapter_reads_chapter_id_fkey`.

-   **`mark_lines`**:
    -   **วัตถุประสงค์:** จัดเก็บบรรทัดที่นักแปลมาร์คไว้ในแต่ละตอน (Denormalized เพื่อประสิทธิภาพ)
    -   **คอลัมน์สำคัญ:**
        -   `id` (UUID, PK, DEFAULT `gen_random_uuid()`): รหัสเฉพาะของรายการมาร์ค
        -   `user_id` (UUID, NOT NULL, FK to `profiles.id` ON DELETE CASCADE): รหัสผู้ใช้ (นักแปล)
        -   `chapter_id` (UUID, NOT NULL, FK to `chapters.id` ON DELETE CASCADE): รหัสตอนที่ถูกมาร์ค
        -   `user_email` (TEXT): อีเมลผู้ใช้ (denormalized)
        -   `user_display_name` (TEXT): ชื่อที่แสดงของผู้ใช้ (denormalized)
        -   `novel_id` (UUID, NOT NULL, FK to `novels.id` ON DELETE CASCADE): รหัสนิยาย (denormalized)
        -   `novel_title` (TEXT): ชื่อนิยาย (denormalized)
        -   `chapter_number` (TEXT): หมายเลขตอน (denormalized)
        -   `chapter_title` (TEXT): ชื่อตอน (denormalized)
        -   `marked_line_numbers` (JSONB, NOT NULL, DEFAULT '[]'): Array ของหมายเลขบรรทัดที่มาร์คไว้
        -   `created_at`, `updated_at` (TIMESTAMPTZ, DEFAULT CURRENT_TIMESTAMP)
    -   **Constraints:** `mark_lines_pkey` (PK on `id`), `mark_lines_user_id_fkey`, `mark_lines_chapter_id_fkey`, `mark_lines_novel_id_fkey`, `mark_lines_user_id_chapter_id_key` (UNIQUE on `user_id`, `chapter_id`).
    -   **หมายเหตุ:** การ denormalize ข้อมูลในตารางนี้ (เช่น `user_email`, `novel_title`) จำเป็นต้องมีกลไกการอัปเดตเมื่อข้อมูลต้นทาง (source) เปลี่ยนแปลง อาจใช้ Triggers ใน PostgreSQL หรือ Logic ใน Application Level (เช่น ผ่าน Supabase Edge Functions หรือ Next.js API Routes)

-   **`novel_versions`**:
    -   **วัตถุประสงค์:** จัดเก็บประวัติการเปลี่ยนแปลงของข้อมูลเมตานิยาย (metadata)
    -   **คอลัมน์สำคัญ:**
        -   `id` (INTEGER, PK, Auto-increment): รหัสเฉพาะของเวอร์ชัน
        -   `novel_id` (UUID, NOT NULL, FK to `novels.id` ON DELETE CASCADE): รหัสนิยาย
        -   `version_number` (INTEGER, NOT NULL): หมายเลขเวอร์ชัน
        -   `metadata_snapshot` (JSONB, NOT NULL): Snapshot ของข้อมูลนิยาย ณ เวลานั้น
        -   `created_by` (UUID, FK to `profiles.id`): ผู้ที่ทำให้เกิดการเปลี่ยนแปลงนี้
        -   `notes` (TEXT): หมายเหตุเกี่ยวกับการเปลี่ยนแปลง
        -   `created_at` (TIMESTAMPTZ, DEFAULT CURRENT_TIMESTAMP)
    -   **Constraints:** `novel_versions_pkey` (PK on `id`), `novel_versions_novel_id_fkey`, `novel_versions_created_by_fkey`, `novel_versions_novel_id_version_number_key` (UNIQUE on `novel_id`, `version_number`).

-   **`chapter_versions`**:
    -   **วัตถุประสงค์:** จัดเก็บประวัติการเปลี่ยนแปลงของเนื้อหาตอน
    -   **คอลัมน์สำคัญ:**
        -   `id` (INTEGER, PK, Auto-increment): รหัสเฉพาะของเวอร์ชัน
        -   `chapter_id` (UUID, NOT NULL, FK to `chapters.id` ON DELETE CASCADE): รหัสตอน
        -   `version_number` (INTEGER, NOT NULL): หมายเลขเวอร์ชัน
        -   `content_original_snapshot` (TEXT): Snapshot ของเนื้อหาตอน ณ เวลานั้น
        -   `created_by` (UUID, FK to `profiles.id`): ผู้ที่ทำให้เกิดการเปลี่ยนแปลงนี้
        -   `notes` (TEXT): หมายเหตุเกี่ยวกับการเปลี่ยนแปลง
        -   `created_at` (TIMESTAMPTZ, DEFAULT CURRENT_TIMESTAMP)
    -   **Constraints:** `chapter_versions_pkey` (PK on `id`), `chapter_versions_chapter_id_fkey`, `chapter_versions_created_by_fkey`, `chapter_versions_chapter_id_version_number_key` (UNIQUE on `chapter_id`, `version_number`).

-   **`announcements`**:
    -   **วัตถุประสงค์:** จัดการประกาศต่างๆ ของเว็บไซต์
    -   **คอลัมน์สำคัญ:**
        -   `id` (INTEGER, PK, Auto-increment): รหัสเฉพาะของประกาศ
        -   `title` (TEXT, NOT NULL): หัวข้อประกาศ
        -   `content` (TEXT, NOT NULL): เนื้อหาประกาศ (อาจเป็น Markdown)
        -   `start_date` (TIMESTAMPTZ): วันที่เริ่มแสดงประกาศ
        -   `end_date` (TIMESTAMPTZ): วันที่สิ้นสุดการแสดงประกาศ
        -   `target_roles` (JSONB, DEFAULT '["Guest", "Member", "Translator", "Admin"]'): กลุ่มผู้ใช้เป้าหมาย
        -   `target_pages` (JSONB, DEFAULT '["homepage"]'): หน้าที่ต้องการให้แสดงประกาศ
        -   `is_active` (BOOLEAN, DEFAULT TRUE): สถานะการใช้งานประกาศ
        -   `created_by` (UUID, FK to `profiles.id`): ผู้สร้างประกาศ
        -   `created_at`, `updated_at` (TIMESTAMPTZ, DEFAULT CURRENT_TIMESTAMP)
    -   **Constraints:** `announcements_pkey` (PK on `id`), `announcements_created_by_fkey` (FK to `profiles`).

-   **`site_settings`**:
    -   **วัตถุประสงค์:** จัดเก็บการตั้งค่าต่างๆ ของเว็บไซต์
    -   **คอลัมน์สำคัญ:**
        -   `key` (TEXT, PK): Key ของการตั้งค่า (เช่น 'site_name', 'allow_registration')
        -   `value` (JSONB): Value ของการตั้งค่า
        -   `description` (TEXT): คำอธิบายการตั้งค่า
        -   `created_at`, `updated_at` (TIMESTAMPTZ, DEFAULT CURRENT_TIMESTAMP)
    -   **Constraints:** `site_settings_pkey` (PK on `key`).

-   **`site_changelogs`**:
    -   **วัตถุประสงค์:** บันทึกการเปลี่ยนแปลง (Changelog) ของเว็บไซต์
    -   **คอลัมน์สำคัญ:**
        -   `id` (INTEGER, PK, Auto-increment): รหัสเฉพาะของ Changelog
        -   `version_number` (TEXT, NOT NULL, UNIQUE): หมายเลขเวอร์ชัน (เช่น '1.0.0', '1.0.1-beta')
        -   `release_date` (DATE, NOT NULL): วันที่เผยแพร่เวอร์ชัน
        -   `summary` (TEXT): สรุปการเปลี่ยนแปลง (อาจเป็น Markdown)
        -   `is_published` (BOOLEAN, DEFAULT FALSE): สถานะการเผยแพร่ Changelog
        -   `created_by` (UUID, FK to `profiles.id`): ผู้สร้าง Changelog
        -   `created_at`, `updated_at` (TIMESTAMPTZ, DEFAULT CURRENT_TIMESTAMP)
    -   **Constraints:** `site_changelogs_pkey` (PK on `id`), `site_changelogs_version_number_key` (UNIQUE on `version_number`), `site_changelogs_created_by_fkey` (FK to `profiles`).

### 4.3 การรักษาความปลอดภัยระดับแถว (Row Level Security - RLS)

RLS จะถูกนำมาใช้กับทุกตารางใน `public` schema เพื่อควบคุมการเข้าถึงข้อมูลตามบทบาทและสิทธิ์ของผู้ใช้ โดยอ้างอิงจาก `auth.uid()` (รหัสผู้ใช้ที่ล็อกอินปัจจุบัน) และข้อมูลจากตาราง `profiles` (โดยเฉพาะ `role_id`) นโยบาย RLS ตัวอย่าง:
*   ผู้ใช้สามารถอ่านและแก้ไขโปรไฟล์ของตนเองได้ (`profiles.id = auth.uid()`)
*   Admin สามารถดำเนินการ CRUD กับข้อมูลส่วนใหญ่ได้
*   นักแปลสามารถ CRUD นิยาย/ตอนที่ตนเองสร้าง (`created_by = auth.uid()`)
*   การเข้าถึงนิยาย/ตอนสาธารณะ (`visibility` / `access_level` มี 'everyone')
*   การเข้าถึงสำหรับสมาชิก (`visibility` / `access_level` มี 'members' และ `auth.uid()` IS NOT NULL)
*   รายละเอียดของ Policy ถูกกำหนดไว้ใน `database.json` ส่วนของ `policies` และจะถูกนำไปใช้ใน Supabase

## 5. Supabase Storage Buckets

-   **`novel-covers`**:
    -   **Public:** True
    -   **วัตถุประสงค์:** จัดเก็บรูปปกนิยาย
    -   **สิทธิ์การเข้าถึง:** อ่านได้ทั่วไป, เขียนได้เฉพาะผู้ใช้ที่ได้รับอนุญาต (ผ่าน RLS บนตาราง `novels` และ Logic ใน API Route)
-   **`chapter-text-files`**:
    -   **Public:** False
    -   **วัตถุประสงค์:** จัดเก็บไฟล์ .txt ที่อัปโหลดสำหรับตอนนิยาย (อาจใช้ชั่วคราวหรือเป็น backup)
    -   **สิทธิ์การเข้าถึง:** จำกัดเฉพาะผู้ใช้ที่ได้รับอนุญาต (นักแปล/Admin) ผ่าน Logic ใน API Route
-   **`avatars`**:
    -   **Public:** True
    -   **วัตถุประสงค์:** จัดเก็บรูปโปรไฟล์ผู้ใช้
    -   **สิทธิ์การเข้าถึง:** อ่านได้ทั่วไป, เขียนได้เฉพาะเจ้าของโปรไฟล์หรือ Admin (ผ่าน RLS บนตาราง `profiles` และ Logic ใน API Route)
-   **`site_assets`**:
    -   **Public:** True
    -   **วัตถุประสงค์:** จัดเก็บทรัพย์สินของเว็บไซต์ เช่น โลโก้, Favicon
    -   **สิทธิ์การเข้าถึง:** อ่านได้ทั่วไป, เขียนได้เฉพาะ Admin (ผ่าน Logic ใน API Route)

## 6. บทบาทผู้ใช้และสิทธิ์ (User Roles & Permissions)

ระบบมีบทบาทหลัก 4 บทบาท โดยสิทธิ์การเข้าถึงจะถูกควบคุมโดย Supabase Row Level Security (RLS) และ Logic ใน Next.js API Routes/Components

1.  **Guest (ผู้เยี่ยมชม):**
    -   อ่านนิยาย/ตอนที่ตั้งค่าเป็น `visibility = 'everyone'` / `access_level = 'everyone'`
    -   ดูประกาศสาธารณะ
    -   ไม่สามารถบุ๊คมาร์ค, มาร์คบรรทัด, หรือสร้างเนื้อหาได้
    -   สามารถลงทะเบียน/เข้าสู่ระบบ

2.  **Member (สมาชิก):**
    -   สิทธิ์ทั้งหมดของ Guest
    -   อ่านนิยาย/ตอนที่ตั้งค่า `visibility` / `access_level` เป็น 'members'
    -   จัดการโปรไฟล์ของตนเอง (ชื่อที่แสดง, รูปโปรไฟล์, รหัสผ่าน, การตั้งค่า)
    -   บุ๊คมาร์คตอนที่อ่าน
    -   ดูประวัติการอ่าน
    -   **แถบเครื่องมือสำหรับอ่าน (Reading Toolbar Component - ใช้ Chakra UI `Flex`):**
        -   ปุ่ม "สารบัญ" (ไอคอน **Font Awesome** `faList`): เปิด **Chakra UI Modal** แสดงรายการตอนของนิยายปัจจุบัน
        -   ปุ่ม "ปรับแต่งการแสดงผล" (ไอคอน **Font Awesome** `faCog` หรือ `faSlidersH`): เปิด **Chakra UI Popover** หรือ **Menu** สำหรับปรับขนาดฟอนต์, font-family (ตัวเลือกพื้นฐาน), ระยะห่างบรรทัด
        -   ปุ่ม "บุ๊คมาร์คตอนปัจจุบัน" (ไอคอน **Font Awesome** `faBookmark` / `faBookmarkSolid`): สลับสถานะการบุ๊คมาร์คของตอนปัจจุบัน
        -   ปุ่ม "สลับธีมเว็บ (Light/Dark)" (ไอคอน **Font Awesome** `faSun` / `faMoon`): สลับโหมดสีของเว็บไซต์
        -   ปุ่ม "เปิด/ปิดโหมดอ่านต่อเนื่อง" (ไอคอน **Font Awesome** `faInfinity`): สลับการโหลดตอนถัดไปอัตโนมัติเมื่อเลื่อนถึงท้ายหน้า
        -   ปุ่ม "นำทางตอนก่อนหน้า/ถัดไป" (ไอคอน **Font Awesome** `faChevronLeft` / `faChevronRight`): ไปยังตอนก่อนหน้าหรือถัดไป

3.  **Translator (นักแปล):**
    -   สิทธิ์ทั้งหมดของ Member
    -   อ่านนิยาย/ตอนที่ตั้งค่า `visibility` / `access_level` เป็น 'translators'
    -   สร้าง, แก้ไข, และลบนิยายและตอนที่ตนเองเป็นผู้สร้าง (`created_by = auth.uid()`)
    -   จัดการเวอร์ชันของนิยายและตอนที่ตนเองสร้าง
    -   เข้าถึง "โหมดนักแปล" (Translator Mode) สำหรับนิยายที่ตนเองจัดการ:
        -   แสดงเลขบรรทัดในหน้าอ่าน/แปล
        -   ใช้ **Chakra UI Checkbox** ข้างแต่ละบรรทัดเพื่อมาร์ค/ยกเลิกการมาร์ค
        -   ดับเบิลคลิกที่บรรทัดเนื้อหาเพื่อมาร์คและคัดลอกบรรทัดนั้นไปยัง Clipboard
        -   ข้อมูลการมาร์คถูกบันทึกลงในตาราง `mark_lines` แบบ Real-time (หรือใกล้เคียง)
    -   เข้าถึงแดชบอร์ดนักแปล (`/translator/dashboard`) เพื่อดูภาพรวมนิยายที่จัดการ
    -   เข้าถึงหน้าจัดการบรรทัดมาร์ค (`/translator/manage-marks/[novel_id]`)
    -   **แถบเครื่องมือพิเศษในโหมดแปล:** เพิ่มปุ่ม "คัดลอกมาร์คทั้งหมด" (ไอคอน **Font Awesome** `faCopy`) เพื่อคัดลอกทุกบรรทัดที่มาร์คไว้ในตอนปัจจุบัน

4.  **Admin (ผู้ดูแลระบบ):**
    -   สิทธิ์ทั้งหมดของ Translator
    -   เข้าถึงแดชบอร์ดผู้ดูแลระบบ (`/admin/dashboard`)
    -   จัดการผู้ใช้ทั้งหมด: ดู, แก้ไขข้อมูล (บทบาท, สถานะระงับ, วันหมดอายุบทบาท), ลบ (soft delete หรือตามนโยบาย)
    -   จัดการหมวดหมู่นิยาย: สร้าง, แก้ไข, ลบ
    -   จัดการนิยายและตอนทั้งหมด: แก้ไข, ลบ, เปลี่ยนผู้สร้าง (ถ้าจำเป็น)
    -   จัดการประกาศ: สร้าง, แก้ไข, ลบ, เผยแพร่/ยกเลิกการเผยแพร่
    -   จัดการการตั้งค่าเว็บไซต์ (`site_settings`)
    -   จัดการ Changelog ของเว็บไซต์ (`site_changelogs`)
    -   ดูและจัดการเวอร์ชันของนิยายและตอนทั้งหมด

## 7. โครงสร้าง URL ที่เป็นมิตรและการจัดการโดเมน

-   **โครงสร้าง URL หลัก (ตัวอย่างการจัดวางใน `pages` directory ของ Next.js):**
    -   หน้ารวมนิยาย (หน้าแรก): `/` (ไฟล์ `pages/index.tsx`)
    -   หน้าข้อมูลนิยาย: `/novels/[novel_slug]` (ไฟล์ `pages/novels/[novel_slug].tsx`, `novel_slug` คือ slug จากตาราง `novels`)
        -   *ทางเลือก:* `/novels/[novel_id]` (ไฟล์ `pages/novels/[novel_id].tsx`, `novel_id` คือ UUID) หากต้องการใช้ ID โดยตรง
    -   หน้าอ่านตอนนิยาย: `/novels/[novel_slug]/[chapter_number]` (ไฟล์ `pages/novels/[novel_slug]/[chapter_number].tsx`, `chapter_number` คือหมายเลขตอน (TEXT) จากตาราง `chapters`)
        -   *ทางเลือก:* `/novels/[novel_id]/[chapter_number]`
    -   หน้าโปรไฟล์ผู้ใช้ (สำหรับผู้ใช้ปัจจุบัน): `/profile` (ไฟล์ `pages/profile.tsx`)
    -   หน้าโปรไฟล์ผู้ใช้ (สำหรับ Admin ดูผู้ใช้อื่น): `/admin/dashboard/users/[user_id]` (ไฟล์ `pages/admin/dashboard/users/[user_id].tsx`)
    -   หน้าบุ๊คมาร์ค: `/bookmarks` (ไฟล์ `pages/bookmarks.tsx`)
    -   แดชบอร์ดนักแปล: `/translator/dashboard` (ไฟล์ `pages/translator/dashboard.tsx`)
    -   หน้าจัดการบรรทัดมาร์ค (นักแปล): `/translator/manage-marks/[novel_slug]` (ไฟล์ `pages/translator/manage-marks/[novel_slug].tsx`) หรือ `/[novel_id]`
    -   แดชบอร์ดผู้ดูแลระบบ: `/admin/dashboard` (ไฟล์ `pages/admin/dashboard/index.tsx` หรือใช้ `_layout.tsx` สำหรับ layout เฉพาะส่วน admin)
    -   หน้ารวมผู้ใช้ในแดชบอร์ดผู้ดูแลระบบ: `/admin/dashboard/users` (ไฟล์ `pages/admin/dashboard/users/index.tsx`)
    -   หน้ารวมหมวดหมู่ในแดชบอร์ดผู้ดูแลระบบ: `/admin/dashboard/categories` (ไฟล์ `pages/admin/dashboard/categories.tsx`)
    -   หน้าอื่นๆ ในแดชบอร์ดผู้ดูแลระบบจะตามรูปแบบ `/admin/dashboard/[section_name]`
-   **การสร้าง Slug สำหรับนิยาย (`novels.slug`):**
    -   เมื่อสร้างนิยายใหม่, ระบบจะแนะนำ slug จากชื่อเรื่อง (แปลงเป็น kebab-case, ตัดอักขระพิเศษ)
    -   ผู้ใช้ (นักแปล/Admin) สามารถแก้ไข slug ที่แนะนำได้
    -   ระบบต้องตรวจสอบความซ้ำซ้อนของ slug ก่อนบันทึก (UNIQUE constraint ใน `novels.slug`)
-   **สำหรับตอนนิยาย:** จะใช้ `chapters.chapter_number` (TEXT) โดยตรงใน URL ซึ่งมีความยืดหยุ่นสำหรับตอนพิเศษ (เช่น '10.5', 'บทนำ')
-   **การจัดการโดเมนและ Base URL:**
    -   ระบบต้องสามารถทำงานได้ไม่ว่า Base URL จะเป็นอะไร (เช่น `localhost:3000`, `dev.inkrealm.co`, `www.inkrealm.co`)
    -   การอ้างอิง URL ภายในแอปพลิเคชันควรเป็นแบบ relative path หรือใช้ Next.js `<Link>` component และ `useRouter` อย่างถูกต้อง
    -   ค่า Environment Variable (เช่น `NEXT_PUBLIC_SITE_URL`) จะถูกใช้สำหรับสร้าง full URL เมื่อจำเป็น (เช่น สำหรับ SEO, sitemap, social sharing, email links)

## 8. การยืนยันตัวตน: เข้าสู่ระบบ / ลงทะเบียน / ลืมรหัสผ่าน

การดำเนินการทั้งหมดนี้จะใช้ Supabase Auth เป็นหลัก และแสดงผลผ่าน **Chakra UI Modal** เพื่อประสบการณ์ผู้ใช้ที่ไม่สะดุด หรือผ่านหน้าเว็บเฉพาะหากผู้ใช้เข้าถึง URL โดยตรง

-   **การแสดงผล:**
    -   **Modal:** ปุ่ม "เข้าสู่ระบบ" หรือ "ลงทะเบียน" บน Navbar จะเปิด **Chakra UI Modal** ที่เกี่ยวข้อง
    -   **หน้าเว็บเฉพาะ:** หากผู้ใช้เข้าถึง URL เช่น `/login`, `/register`, `/forgot-password`, `/reset-password` โดยตรง จะแสดงหน้าเว็บนั้นๆ
-   **การพัฒนา Frontend (Next.js):**
    -   สร้าง Forms ด้วย React Components และ **Chakra UI** (`Input`, `Button`, `FormControl`, `FormLabel`, `FormErrorMessage`, `Checkbox` สำหรับ "จำฉันไว้", `Stack` หรือ `Flex` สำหรับ layout)
    -   จัดการ Form State และ Validation ด้วย **React Hook Form** และ **Zod** schemas สำหรับแต่ละฟอร์ม (เช่น `LoginSchema`, `RegisterSchema`)
    -   **การเข้าสู่ระบบ (Login):**
        -   ฟิลด์: อีเมล, รหัสผ่าน
        -   ปุ่ม: "เข้าสู่ระบบด้วยอีเมล", "เข้าสู่ระบบด้วย Google" (ใช้ Supabase Auth provider)
        -   ลิงก์: "ลืมรหัสผ่าน?", "ยังไม่มีบัญชี? ลงทะเบียน"
    -   **การลงทะเบียน (Register):**
        -   ฟิลด์: อีเมล, รหัสผ่าน, ยืนยันรหัสผ่าน, (อาจมี ชื่อที่แสดง - `display_name` ถ้าต้องการให้ตั้งค่าทันที)
        -   ปุ่ม: "ลงทะเบียน"
        -   ลิงก์: "มีบัญชีแล้ว? เข้าสู่ระบบ"
        -   เมื่อลงทะเบียนสำเร็จ (และยืนยันอีเมล ถ้าเปิดใช้งาน), ระบบจะสร้าง record ใน `auth.users` (โดย Supabase Auth) และ trigger การสร้าง record ใน `public.profiles` (ผ่าน Next.js API Route หรือ Supabase Edge Function ที่ subscribe กับ `onAuthStateChange` หรือ trigger ใน DB) โดยกำหนด `role_id` เริ่มต้นเป็น 'Member' (จาก `roles` table) และ `email` จาก `auth.users`.
    -   **การลืมรหัสผ่าน (Forgot Password):**
        -   ฟิลด์: อีเมล
        -   ปุ่ม: "ส่งลิงก์รีเซ็ตรหัสผ่าน"
    -   **การรีเซ็ตรหัสผ่าน (Reset Password - จากลิงก์ในอีเมล):**
        -   ฟิลด์: รหัสผ่านใหม่, ยืนยันรหัสผ่านใหม่
        -   ปุ่ม: "ตั้งรหัสผ่านใหม่"
-   **การพัฒนา Backend (Supabase Auth + API Routes/Edge Functions):**
    -   ใช้ Supabase Client Library (`supabase.auth.signInWithPassword()`, `supabase.auth.signUp()`, `supabase.auth.signInWithOAuth()`, `supabase.auth.resetPasswordForEmail()`, `supabase.auth.updateUser()`)
    -   API Route/Edge Function สำหรับการซิงค์ข้อมูลไปยัง `profiles` หลังการลงทะเบียนหรืออัปเดตข้อมูลจาก Social OAuth
-   **การแจ้งเตือน (User Feedback):**
    -   แสดง **Chakra UI Toast** สำหรับการยืนยันการกระทำ (เช่น "ส่งอีเมลรีเซ็ตรหัสผ่านแล้ว"), แจ้งเตือนข้อผิดพลาด (เช่น "อีเมลหรือรหัสผ่านไม่ถูกต้อง"), หรือความสำเร็จ (เช่น "เข้าสู่ระบบสำเร็จ")

## 9. โครงสร้างแถบนำทาง (Navbar Structure)

Navbar จะเป็นส่วนประกอบหลักที่ปรากฏในทุกหน้า (ยกเว้นหน้าที่อาจต้องการ layout พิเศษ) และจะปรับเปลี่ยนการแสดงผลตามสถานะการล็อกอินและบทบาทของผู้ใช้ ใช้ **Chakra UI Components** เช่น `Flex`, `Box`, `Link` (จาก Next.js), `Button`, `Menu`, `Avatar`, `IconButton` และ **Font Awesome Icons**

-   **ความสามารถ:** Responsive Design
    -   **Desktop:** แสดงเมนูเต็มรูปแบบ
    -   **Mobile:** อาจยุบเมนูบางส่วนไว้ใน Hamburger Menu (ใช้ **Chakra UI `Menu`** หรือ **`Drawer`**)
-   **องค์ประกอบทั่วไป:**
    -   **โลโก้ INKREALM:** (คลิกเพื่อกลับหน้าแรก)
    -   **ปุ่มสลับธีม (Light/Dark):** **Chakra UI `IconButton`** พร้อมไอคอน **Font Awesome** `faSun` / `faMoon`
-   **แสดงผลตามบทบาท:**

    -   **Guest (ผู้เยี่ยมชม):**
        -   โลโก้
        -   (อาจมีลิงก์สำคัญ เช่น "นิยายยอดนิยม", "หมวดหมู่" - พิจารณาตามความเหมาะสม)
        -   ปุ่มสลับธีม
        -   ปุ่ม "เข้าสู่ระบบ" (**Chakra UI `Button`**): เปิด Modal เข้าสู่ระบบ
        -   ปุ่ม "ลงทะเบียน" (**Chakra UI `Button`** variant="outline"): เปิด Modal ลงทะเบียน

    -   **Member (สมาชิก):**
        -   โลโก้
        -   (อาจมีลิงก์ เช่น "บุ๊คมาร์คของฉัน")
        -   ปุ่มสลับธีม
        -   **เมนูโปรไฟล์ผู้ใช้:** แสดงด้วย **Chakra UI `Avatar`** (รูปโปรไฟล์ผู้ใช้) และคลิกเพื่อเปิด **Chakra UI `Menu`** หรือ **`Popover`** ที่มีรายการ:
            -   แสดงชื่อที่แสดง (Display Name) และบทบาท (เช่น "สมาชิก")
            -   ลิงก์ไปยังหน้าโปรไฟล์: `<Link href="/profile">หน้าโปรไฟล์</Link>` (ใช้ **Font Awesome** `faUserCircle` ข้างหน้า)
            -   ลิงก์ไปยังหน้าบุ๊คมาร์ค: `<Link href="/bookmarks">บุ๊คมาร์คของฉัน</Link>` (ใช้ **Font Awesome** `faBookmark` ข้างหน้า)
            -   ปุ่ม "ออกจากระบบ" (ใช้ **Font Awesome** `faSignOutAlt` ข้างหน้า): เรียกใช้ `supabase.auth.signOut()` และแสดง **Chakra UI AlertDialog** เพื่อยืนยัน

    -   **Translator (นักแปล):**
        -   เหมือน Member
        -   เพิ่มในเมนูโปรไฟล์ผู้ใช้:
            -   ลิงก์ไปยังแดชบอร์ดนักแปล: `<Link href="/translator/dashboard">แดชบอร์ดนักแปล</Link>` (ใช้ **Font Awesome** `faTachometerAlt` ข้างหน้า)

    -   **Admin (ผู้ดูแลระบบ):**
        -   เหมือน Translator
        -   เพิ่มในเมนูโปรไฟล์ผู้ใช้:
            -   ลิงก์ไปยังแดชบอร์ดผู้ดูแลระบบ: `<Link href="/admin/dashboard">แดชบอร์ดผู้ดูแลระบบ</Link>` (ใช้ **Font Awesome** `faUserShield` ข้างหน้า)

## 10. รายละเอียดหน้าเว็บไซต์, Modals, และการจัดการเนื้อหา

### 10.1 หน้าแรก (Home Page - `pages/index.tsx`)

-   **วัตถุประสงค์:** แสดงรายการนิยาย, เครื่องมือค้นหา/ฟิลเตอร์, และประกาศล่าสุด
-   **ส่วนประกอบ:**
    1.  **ส่วนแสดงประกาศ (Announcements Display Area - React Component):**
        -   ดึงข้อมูลจากตาราง `announcements` ที่ `is_active = true` และตรงตาม `target_roles` / `target_pages` ('homepage')
        -   แสดงผลด้วย **Chakra UI `Alert`** (สามารถมี `status` ต่างๆ เช่น 'info', 'warning') หรือ **Chakra UI `Card`** ที่ออกแบบมาเฉพาะ
        -   แต่ละประกาศมีปุ่ม "X" (**Font Awesome** `faTimes` หรือ **Chakra UI `CloseButton`**) เพื่อให้ผู้ใช้ปิดได้ (สถานะการปิดอาจบันทึกใน `localStorage`)
        -   สามารถแสดงหลายประกาศพร้อมกัน (เช่น Carousel หรือ Stack) หรือแสดงเฉพาะประกาศล่าสุด/สำคัญที่สุด
    2.  **ส่วนเครื่องมือหลัก (Main Tools Section - React Component ใช้ Chakra UI `Flex` หรือ `Stack`):**
        -   **ช่องค้นหานิยาย:** **Chakra UI `Input`** พร้อม `InputGroup` และ `InputLeftElement` ที่มีไอคอน **Font Awesome** `faSearch`. ค้นหาจาก `novels.title`, `novels.synopsis` (อาจรวมชื่อผู้แต่งถ้ามี).
        -   **ฟิลเตอร์หมวดหมู่ (Category Filter):** **Chakra UI `Select`**. ดึงข้อมูลจากตาราง `categories`. ตัวเลือกแรกคือ "ทุกหมวดหมู่".
        -   **ฟิลเตอร์สถานะนิยาย (Novel Status Filter):** **Chakra UI `Select`**. ตัวเลือก: "ทั้งหมด", "กำลังดำเนินการ" (ongoing), "จบแล้ว" (completed), "ยุติการลง" (dropped).
        -   **(สำหรับ Translator/Admin) ปุ่มสร้างนิยาย (Create Novel Button):** **Chakra UI `Button`** พร้อม `leftIcon` เป็น **Font Awesome Icon** เช่น `faPlusSquare` หรือ `faFileMedical` และข้อความ "สร้างนิยายใหม่". เมื่อคลิก จะเปิด Modal สร้างนิยาย (ดูข้อ 10.1.1).
    3.  **การแสดงรายการนิยาย (Novel List Display - React Component):**
        -   ดึงข้อมูลจากตาราง `novels` ตามเงื่อนไขการค้นหาและฟิลเตอร์, และสิทธิ์การมองเห็นของผู้ใช้
        -   รูปแบบ Grid Layout (ใช้ **Chakra UI `SimpleGrid`** หรือ `Grid`) ที่ responsive
        -   **การ์ดนิยาย (Novel Card - Custom React Component ใช้ Chakra UI `Box` หรือ `Card`):**
            -   แสดงรูปปกนิยาย (`novels.cover_image_url`) โดยใช้ `next/image`
            -   ชื่อเรื่องนิยาย (`novels.title`) เป็น **Chakra UI `Heading`** และเป็น Link ไปยังหน้าข้อมูลนิยาย (`/novels/[novel_slug]`)
            -   จำนวนตอนทั้งหมด (`novels.total_chapters`) แสดงด้วย **Chakra UI `Text`** หรือ `Badge`
            -   (Optional) หมวดหมู่, สถานะ, วันที่อัปเดตล่าสุด
    4.  **ระบบแบ่งหน้า (Pagination):**
        -   หากรายการนิยายมีจำนวนมาก จะแสดงระบบแบ่งหน้า
        -   ใช้ **Chakra UI `Button`** และ `IconButton` (สำหรับ "ก่อนหน้า", "ถัดไป", หมายเลขหน้า) หรือ custom component ที่สร้างจาก Chakra UI elements.
        -   จัดการ state การแบ่งหน้า (current page) และ query ข้อมูลใหม่เมื่อเปลี่ยนหน้า
-   **การพัฒนา:**
    -   Data fetching ด้วย React Query (`useQuery` สำหรับรายการนิยาย, หมวดหมู่; `useInfiniteQuery` ถ้าต้องการ infinite scroll แทน pagination)
    -   API Routes สำหรับการค้นหา/ฟิลเตอร์ที่ซับซ้อน

### 10.1.1 Modal สร้างนิยาย (Create Novel Modal)

-   **การเรียกใช้งาน:** คลิกปุ่ม "สร้างนิยายใหม่" บนหน้าแรก (สำหรับ Translator/Admin)
-   **UI ภายใน Modal (ใช้ **Chakra UI `Modal`**, `ModalOverlay`, `ModalContent`, `ModalHeader`, `ModalBody`, `ModalFooter`, `ModalCloseButton`):**
    -   **หัวเรื่อง Modal:** "สร้างนิยายใหม่"
    -   **ฟอร์ม (จัดการด้วย React Hook Form, Validation ด้วย Zod Schema สำหรับ `novels`):**
        -   **ชื่อเรื่อง (Title):** **Chakra UI `Input`**. (`novels.title`, Required)
        -   **คำโปรย (Synopsis):** **Rich Text Editor (TipTap)** ที่ผสานเข้ากับ Chakra UI form control. (`novels.synopsis`)
        -   **หมวดหมู่ (Category):** **Chakra UI `Select`**. ดึงข้อมูล `id` และ `name` จากตาราง `categories`. (`novels.category_id`)
        -   **รูปปกนิยาย (Cover Image):** **Chakra UI `FormControl`** ที่มี `Input type="file"` (อาจซ่อนไว้และใช้ `Button` สวยๆ แทน) และส่วนแสดงตัวอย่างรูปภาพ. เมื่อเลือกไฟล์, แสดงเครื่องมือ Crop (ใช้ `react-image-crop`) ให้ผู้ใช้ปรับขนาด/ตำแหน่งก่อนอัปโหลด. (`novels.cover_image_url`)
        -   **ภาษาต้นฉบับ (Original Language):** **Chakra UI `Select`**. ตัวเลือก: "จีน" (value: `zh`), "อังกฤษ" (value: `en`), "ไทย" (value: `th`). (`novels.original_language`, Required)
        -   **Slug นิยาย (`novel_slug`):** **Chakra UI `Input`**. ระบบอาจสร้างค่าเริ่มต้นจากชื่อเรื่อง และผู้ใช้สามารถแก้ไขได้. ต้องตรวจสอบความซ้ำซ้อน. (`novels.slug`, Required, Unique)
        -   **สถานะนิยาย (Novel Status):** **Chakra UI `Select`** หรือ **`RadioGroup`** กับ **`Radio`**. ตัวเลือก: "กำลังดำเนินการ" (ongoing), "จบแล้ว" (completed), "ยุติการลง" (dropped). (`novels.status`)
        -   **สิทธิ์ในการมองเห็นนิยาย (Novel Visibility):** **Chakra UI `Select`** (multiple) หรือ **`CheckboxGroup`** กับ **`Checkbox`**. ตัวเลือก: "ทุกคน" (everyone), "เฉพาะสมาชิก" (members), "เฉพาะนักแปล" (translators). (`novels.visibility`)
        -   **การตั้งค่าเริ่มต้นสำหรับตอนใหม่ของนิยายนี้:**
            -   **สถานะเริ่มต้นของตอน (Default Chapter Status):** **Chakra UI `RadioGroup`**. ตัวเลือก: "เผยแพร่" (published), "ฉบับร่าง" (draft). (`novels.default_chapter_status`)
            -   **สิทธิ์การเข้าถึงตอนเริ่มต้น (Default Chapter Access):** **Chakra UI `Select`** (multiple) หรือ **`CheckboxGroup`**. ตัวเลือก: "ทุกคน" (everyone), "เฉพาะสมาชิก" (members), "เฉพาะนักแปล" (translators). (`novels.default_chapter_access`)
    -   **ปุ่มดำเนินการ (ใน `ModalFooter`):**
        -   "เผยแพร่นิยาย" (**Chakra UI `Button`** colorScheme="blue"): บันทึกนิยายและตั้งค่าสถานะที่เหมาะสม.
        -   "บันทึกเป็นฉบับร่าง" (ถ้ามีสถานะร่างสำหรับนิยาย): (**Chakra UI `Button`**)
        -   "ยกเลิก" (**Chakra UI `Button`** variant="ghost"): ปิด Modal.
-   **การทำงานเบื้องหลัง (เมื่อ Submit Form):**
    -   เรียก Next.js API Route
    -   API Route ทำการ Validate ข้อมูลด้วย Zod อีกครั้ง
    -   อัปโหลดรูปปกนิยายไปยัง Supabase Storage (`novel-covers` bucket) และรับ URL กลับมา
    -   บันทึกข้อมูลนิยายลงในตาราง `novels` (กำหนด `created_by` เป็น `auth.uid()` ของผู้ใช้ปัจจุบัน)
    -   สร้าง record แรกในตาราง `novel_versions` เพื่อเก็บ snapshot ของข้อมูล metadata เริ่มต้น
    -   แสดง **Chakra UI Toast** แจ้งผล (สำเร็จ/ล้มเหลว) และปิด Modal (ถ้าสำเร็จ) หรือ redirect ไปหน้าข้อมูลนิยายที่เพิ่งสร้าง

### 10.2 หน้าข้อมูลนิยายและสารบัญ (Novel Detail Page - `pages/novels/[novel_slug].tsx` หรือ `pages/novels/[novel_id].tsx`)

-   **วัตถุประสงค์:** แสดงข้อมูลรายละเอียดของนิยาย, สารบัญตอน, และเครื่องมือจัดการสำหรับผู้สร้าง/Admin
-   **การดึงข้อมูล:** ใช้ `getServerSideProps` หรือ `getStaticProps` (ถ้าเนื้อหาไม่เปลี่ยนบ่อยและต้องการ SEO) เพื่อดึงข้อมูลนิยาย (`novels` table) และรายการตอน (`chapters` table) ตาม `novel_slug` หรือ `novel_id` จาก URL.
-   **ส่วนประกอบ:**
    1.  **ส่วนแสดงประกาศ:** (เหมือนหน้าแรก, กรอง `target_pages` ให้ตรงกับหน้านิยาย)
    2.  **ส่วนข้อมูลนิยาย (Novel Information Section - ใช้ Chakra UI `Flex`, `Box`, `Image`, `Heading`, `Text`, `Badge`):**
        -   รูปปกนิยาย (`novels.cover_image_url`) แสดงด้วย `next/image`
        -   ชื่อเรื่องนิยาย (`novels.title`) เป็น **Chakra UI `Heading`**
        -   หมวดหมู่ (`categories.name` จาก join หรือ query แยก)
        -   คำโปรย (`novels.synopsis`) Render HTML จาก Rich Text Editor อย่างปลอดภัย (DOMPurify)
        -   จำนวนตอนทั้งหมด (`novels.total_chapters`)
        -   สถานะนิยาย (`novels.status` - แสดงด้วย **Chakra UI `Badge`**)
        -   ภาษาต้นฉบับ (`novels.original_language` - แสดงเป็นชื่อเต็ม เช่น "จีน" จาก lang code 'zh')
        -   Slug นิยาย (`novels.slug` - อาจแสดงให้ Admin เห็น)
        -   ผู้สร้าง (`profiles.display_name` ของ `novels.created_by`)
        -   วันที่สร้าง/อัปเดตล่าสุด
    3.  **(สำหรับนักแปล/Admin ที่เป็นเจ้าของนิยาย) ปุ่มสลับ "โหมดนักแปล" (Translator Mode Switch):**
        -   **Chakra UI `Switch`** พร้อม Label "เปิดโหมดนักแปลสำหรับนิยายนี้"
        -   สถานะของ Switch นี้จะถูกบันทึก (อาจใน `localStorage` หรือ `profiles.user_settings` โดยเชื่อมกับ `novel_id`) เพื่อให้จำค่าที่ตั้งไว้สำหรับนิยายแต่ละเรื่อง
        -   การเปิดโหมดนี้จะมีผลต่อการแสดงผลในหน้าอ่านตอน (ข้อ 10.5)
    4.  **ปุ่มจัดการนิยาย (Novel Management Buttons - สำหรับผู้สร้าง/Admin, ใช้ Chakra UI `ButtonGroup` หรือ `Flex`):**
        -   ปุ่ม "แก้ไขข้อมูลนิยาย" (**Chakra UI `Button`** `leftIcon={<FontAwesomeIcon icon={faEdit} />}`): เปิด Modal แก้ไขข้อมูลนิยาย (ดูข้อ 10.2.1)
        -   ปุ่ม "ลบนิยาย" (**Chakra UI `Button`** colorScheme="red" `leftIcon={<FontAwesomeIcon icon={faTrashAlt} />}`): แสดง **Chakra UI `AlertDialog`** เพื่อยืนยันการลบ (ควรเป็นการ soft delete หรือมีกระบวนการที่ชัดเจน)
    5.  **ส่วนเพิ่มตอน (Add Chapter Section - สำหรับผู้สร้าง/Admin, ใช้ Chakra UI `ButtonGroup` หรือ `Flex`):**
        -   ปุ่ม "เพิ่มตอนด้วย .txt" (**Chakra UI `Button`** `leftIcon={<FontAwesomeIcon icon={faFileUpload} />}`): เปิด Modal Batch Upload ตอน (ดูข้อ 10.2.2)
        -   ปุ่ม "เพิ่มตอนใหม่" (**Chakra UI `Button`** `leftIcon={<FontAwesomeIcon icon={faPlusCircle} />}`): เปิด Modal สร้าง/แก้ไขตอน (ดูข้อ 10.2.3, แต่เป็นโหมดสร้างใหม่)
    6.  **ส่วนสารบัญ (Table of Contents Section - ใช้ Chakra UI `Accordion` หรือ custom List component):**
        -   ดึงข้อมูลตอนทั้งหมด (`chapters` table) ที่ `novel_id` ตรงกัน, เรียงตาม `chapter_number`
        -   **แต่ละรายการตอน (Chapter Item - ใน `AccordionItem` หรือ `ListItem`):**
            -   ไอคอนบุ๊คมาร์ค: **Font Awesome Icon** (`faBookmark` ถ้าบุ๊คมาร์คไว้, `faBookmarkOutline` ถ้าไม่) - ดึงข้อมูลจาก `user_bookmarks`
            -   หมายเลขตอน (`chapters.chapter_number`)
            -   ชื่อตอน (`chapters.title`) - เป็น `<Link>` ไปยังหน้าอ่านตอน (`/novels/[novel_slug]/[chapter_number]`)
            -   สถานะการอ่าน: ไอคอน **Font Awesome** `faEye` (หากผู้ใช้อ่านตอนนี้แล้ว - ดึงข้อมูลจาก `user_chapter_reads`) หรือ `faEyeSlash`
            -   **(สำหรับผู้สร้าง/Admin ของตอนนี้):**
                -   ไอคอนสถานะตอน: **Font Awesome Icon** (เช่น `faCheckCircle` สำหรับ 'published', `faHourglassHalf` สำหรับ 'draft')
                -   ปุ่ม "แก้ไขตอน": **Chakra UI `IconButton`** `aria-label="แก้ไขตอน"` `icon={<FontAwesomeIcon icon={faPencilAlt} />}`. เปิด Modal สร้าง/แก้ไขตอน (ข้อ 10.2.3, โหมดแก้ไข)
                -   ปุ่ม "ลบตอน": **Chakra UI `IconButton`** `aria-label="ลบตอน"` `icon={<FontAwesomeIcon icon={faTrashAlt} />}`. แสดง **Chakra UI `AlertDialog`** เพื่อยืนยันการลบ
                -   **(สำหรับนักแปลเจ้าของ) ปุ่ม "คัดลอกมาร์คทั้งหมดของตอนนี้":** **Chakra UI `IconButton`** `aria-label="คัดลอกมาร์ค"` `icon={<FontAwesomeIcon icon={faCopy} />}`. ดึง `marked_line_numbers` จาก `mark_lines` ของตอนนี้ และคัดลอกไปยัง clipboard.
        -   อาจมีการแบ่งกลุ่มตอน (เช่น ทุกๆ 50 ตอน) หากมีจำนวนตอนมาก โดยใช้ `AccordionPanel` หรือส่วนหัวย่อย

### 10.2.1 Modal แก้ไขข้อมูลนิยาย (Edit Novel Modal)

-   **การเรียกใช้งาน:** คลิกปุ่ม "แก้ไขข้อมูลนิยาย" บนหน้าข้อมูลนิยาย
-   **UI ภายใน Modal (ใช้ **Chakra UI `Modal`**):**
    -   ฟอร์มเหมือนกับ Modal สร้างนิยาย (ข้อ 10.1.1) ทุกประการ แต่จะมีการเติมข้อมูลปัจจุบันของนิยายลงในฟิลด์ต่างๆ
    -   **หัวเรื่อง Modal:** "แก้ไขข้อมูลนิยาย: [ชื่อนิยายปัจจุบัน]"
    -   **ภาษาต้นฉบับ:** แสดงค่าปัจจุบัน (เช่น 'zh') และสามารถแก้ไขได้ผ่าน **Chakra UI `Select`**
    -   ผู้ใช้สามารถแก้ไขข้อมูลต่างๆ เช่น ชื่อเรื่อง, คำโปรย, หมวดหมู่, รูปปก, สถานะ, สิทธิ์การมองเห็น, การตั้งค่าเริ่มต้นของตอน
    -   **ปุ่มดำเนินการ (ใน `ModalFooter`):**
        -   "บันทึกการเปลี่ยนแปลง" (**Chakra UI `Button`** colorScheme="blue")
        -   "ยกเลิก" (**Chakra UI `Button`** variant="ghost")
        -   (อาจมี) "ลบนิยาย" (**Chakra UI `Button`** colorScheme="red"): เปิด **Chakra UI `AlertDialog`** เพื่อยืนยัน
-   **การทำงานเบื้องหลัง (เมื่อ Submit Form):**
    -   เรียก Next.js API Route
    -   API Route ทำการ Validate ข้อมูล
    -   หากมีการเปลี่ยนรูปปก, อัปโหลดรูปใหม่และลบรูปเก่า (ถ้าจำเป็น)
    -   อัปเดตข้อมูลในตาราง `novels`
    -   สร้าง record ใหม่ในตาราง `novel_versions` เพื่อเก็บ snapshot ของข้อมูลที่เปลี่ยนแปลง พร้อม `notes` (ถ้ามี)
    -   แสดง **Chakra UI Toast** แจ้งผล และปิด Modal, รีเฟรชข้อมูลในหน้าข้อมูลนิยาย

### 10.2.2 Modal เพิ่มตอนด้วย .txt (Batch Upload Chapters Modal)

-   **การเรียกใช้งาน:** คลิกปุ่ม "เพิ่มตอนด้วย .txt" บนหน้าข้อมูลนิยาย
-   **UI ภายใน Modal (ใช้ **Chakra UI `Modal`**):**
    -   **หัวเรื่อง Modal:** "เพิ่มหลายตอนด้วยไฟล์ .txt"
    -   **ส่วนอัปโหลดไฟล์:**
        -   พื้นที่ Drag & Drop หรือ ปุ่ม "เลือกไฟล์ .txt" (**Chakra UI `Input type="file"`** `multiple accept=".txt"` ที่ซ่อนอยู่และเรียกใช้ผ่าน `Button`)
        -   แสดงรายการไฟล์ที่เลือกไว้ (ชื่อไฟล์, ขนาด)
    -   **ตัวเลือกการจัดการชื่อตอน/หมายเลขตอน:**
        -   ระบบจะพยายามดึงชื่อตอนและหมายเลขตอนจากชื่อไฟล์ (เช่น "ตอนที่ 001 - ชื่อตอน.txt" -> `chapter_number: '001'`, `title: 'ชื่อตอน'`) หรือผู้ใช้อาจต้องกำหนดรูปแบบ
        -   **ตัวเลือกการจัดการชื่อตอน/หมายเลขตอนที่ซ้ำกับที่มีอยู่แล้ว:** **Chakra UI `RadioGroup`**. ตัวเลือก:
            -   "เขียนทับเนื้อหาตอนที่มีอยู่" (Update existing chapter)
            -   "ข้ามไฟล์นี้" (Skip this file)
            -   "สร้างเป็นตอนใหม่" (อาจต้องมีการปรับ `chapter_number` เล็กน้อย)
    -   **Progress Bar (**Chakra UI `Progress`**):** แสดงความคืบหน้าการอัปโหลดและประมวลผลไฟล์ทั้งหมด
    -   **ปุ่มดำเนินการ:** "เริ่มอัปโหลด", "ยกเลิก"
-   **การทำงานเบื้องหลัง (เมื่อเริ่มอัปโหลด):**
    -   Next.js API Route รับรายการไฟล์
    -   วนลูปประมวลผลแต่ละไฟล์ .txt:
        -   อ่านเนื้อหาไฟล์
        -   พยายามแยก `chapter_number` และ `title` จากชื่อไฟล์ หรือตามรูปแบบที่กำหนด
        -   ตรวจสอบการซ้ำซ้อนตามตัวเลือกของผู้ใช้
        -   สร้าง/อัปเดต record ในตาราง `chapters` (ใช้ `default_chapter_status` และ `default_chapter_access` จาก `novels` table, `created_by = auth.uid()`, คำนวณ `word_count`)
        -   สร้าง record ว่างในตาราง `mark_lines` สำหรับแต่ละตอนที่สร้างใหม่ (ถ้ายังไม่มี)
        -   สร้าง record ในตาราง `chapter_versions` สำหรับแต่ละตอนที่สร้าง/อัปเดต
    -   อัปเดต `novels.total_chapters` และ `novels.last_chapter_updated_at`
    -   แสดง **Chakra UI Toast** แจ้งผลรวม (เช่น "อัปโหลดสำเร็จ 5 ตอน, ข้าม 1 ตอน") และปิด Modal, รีเฟรชสารบัญ

### 10.2.3 Modal เพิ่ม/แก้ไขตอน (Create/Edit Chapter Modal)

-   **การเรียกใช้งาน:**
    -   **เพิ่มตอน:** คลิกปุ่ม "เพิ่มตอนใหม่" บนหน้าข้อมูลนิยาย
    -   **แก้ไขตอน:** คลิกปุ่ม "แก้ไขตอน" ในรายการสารบัญของแต่ละตอน
-   **UI ภายใน Modal (ใช้ **Chakra UI `Modal`**):**
    -   **หัวเรื่อง Modal:** "เพิ่มตอนใหม่" หรือ "แก้ไขตอน: [ชื่อตอนปัจจุบัน]"
    -   **ฟอร์ม (จัดการด้วย React Hook Form, Validation ด้วย Zod Schema สำหรับ `chapters`):**
        -   **ชื่อตอน (Chapter Title):** **Chakra UI `Input`**. (`chapters.title`, Required)
        -   **หมายเลขตอน (`chapter_number` - TEXT):** **Chakra UI `Input`**. (`chapters.chapter_number`, Required). ระบบต้อง Validate ไม่ให้ซ้ำกับ `chapter_number` อื่นในนิยายเดียวกัน และอาจแนะนำรูปแบบตัวเลขพร้อม padding (เช่น '001', '002').
        -   **เนื้อหา (`content_original`):** **Rich Text Editor (TipTap)**. (`chapters.content_original`)
        -   **สถานะตอน (Chapter Status):** **Chakra UI `RadioGroup`**. ตัวเลือก: "เผยแพร่" (published), "ฉบับร่าง" (draft). (`chapters.status`)
        -   **สิทธิ์การเข้าถึงตอน (Chapter Access Level):** **Chakra UI `Select`** (multiple) หรือ **`CheckboxGroup`**. ตัวเลือก: "ทุกคน" (everyone), "เฉพาะสมาชิก" (members), "เฉพาะนักแปล" (translators). (`chapters.access_level`)
    -   **ปุ่มดำเนินการ:** "บันทึกตอน", "ยกเลิก"
-   **การทำงานเบื้องหลัง (เมื่อ Submit Form):**
    -   เรียก Next.js API Route
    -   API Route ทำการ Validate ข้อมูล
    -   คำนวณ `word_count` จาก `content_original`
    -   **ถ้าเป็นการสร้างตอนใหม่:**
        -   บันทึกข้อมูลลงในตาราง `chapters` (กำหนด `novel_id`, `created_by = auth.uid()`)
        -   สร้าง record ว่างในตาราง `mark_lines`
        -   สร้าง record แรกใน `chapter_versions`
        -   อัปเดต `novels.total_chapters` และ `novels.last_chapter_updated_at`
    -   **ถ้าเป็นการแก้ไขตอน:**
        -   อัปเดตข้อมูลในตาราง `chapters`
        -   สร้าง record ใหม่ใน `chapter_versions`
        -   อัปเดต `novels.last_chapter_updated_at`
    -   แสดง **Chakra UI Toast** แจ้งผล และปิด Modal, รีเฟรชสารบัญ

### 10.3 หน้าบุ๊คมาร์ค (Bookmarks Page - `pages/bookmarks.tsx`)

-   **วัตถุประสงค์:** แสดงรายการนิยายที่ผู้ใช้บุ๊คมาร์คตอนไว้ และให้ผู้ใช้จัดการบุ๊คมาร์คเหล่านั้น
-   **การดึงข้อมูล:** ดึงข้อมูลจาก `user_bookmarks` โดย `user_id = auth.uid()`, join กับ `novels` และ `chapters` เพื่อแสดงข้อมูลที่เกี่ยวข้อง (ชื่อนิยาย, รูปปก, ชื่อตอนล่าสุดที่บุ๊คมาร์ค)
-   **ส่วนประกอบ:**
    1.  **ส่วนหัว:** "บุ๊คมาร์คของฉัน"
    2.  **รายการนิยายที่มีบุ๊คมาร์ค (แสดงเป็น **Chakra UI `Card`** หรือ `ListItem` ต่อหนึ่งนิยาย):**
        -   รูปปกนิยาย, ชื่อนิยาย
        -   ข้อมูลตอนล่าสุดที่บุ๊คมาร์ค: "บุ๊คมาร์คล่าสุด: ตอนที่ [chapter_number] - [chapter_title]"
        -   ปุ่ม "ไปยังบุ๊คมาร์คล่าสุด" (**Chakra UI `Button`** หรือ `<Link>`): พาไปยังหน้าอ่านของตอนล่าสุดที่บุ๊คมาร์คในนิยายนั้น
        -   ปุ่ม "จัดการบุ๊คมาร์คสำหรับเรื่องนี้" (**Chakra UI `Button`** variant="outline"): เปิด Modal จัดการบุ๊คมาร์คเฉพาะเรื่อง (ดูด้านล่าง)
    3.  **Modal "จัดการบุ๊คมาร์คสำหรับเรื่อง: [ชื่อนิยาย]" (เรียกโดยปุ่มด้านบน, ใช้ **Chakra UI `Modal`**):**
        -   **หัวเรื่อง Modal:** "จัดการบุ๊คมาร์คสำหรับ: [ชื่อนิยาย]"
        -   **ตัวเลือกเรียงลำดับ:** **Chakra UI `Select`** (เรียงตามวันที่บุ๊คมาร์ค ล่าสุด/เก่าสุด, เรียงตามหมายเลขตอน น้อยไปมาก/มากไปน้อย)
        -   **รายการบุ๊คมาร์คสำหรับนิยายนั้น (แสดงเป็น List พร้อม **Chakra UI `Checkbox`** หน้าแต่ละรายการ):**
            -   "ตอนที่ [chapter_number] - [chapter_title]" (เป็น Link ไปยังหน้าอ่าน)
            -   วันที่บุ๊คมาร์ค
        -   **ปุ่มดำเนินการใน Modal:**
            -   "ลบรายการที่เลือก" (**Chakra UI `Button`** colorScheme="red"): ลบ `user_bookmarks` record ที่ถูกเลือก
            -   "ล้างบุ๊คมาร์คทั้งหมดของเรื่องนี้" (**Chakra UI `Button`** colorScheme="red" variant="outline"): เปิด **Chakra UI `AlertDialog`** เพื่อยืนยันการลบบุ๊คมาร์คทั้งหมดของนิยายเรื่องนี้สำหรับผู้ใช้ปัจจุบัน
    4.  **ปุ่ม "ลบบุ๊คมาร์คทั้งหมด (ทุกเรื่อง)" (แสดงที่ท้ายหน้าหรือส่วนที่เห็นชัดเจน):**
        -   **Chakra UI `Button`** colorScheme="red" variant="solid"
        -   เมื่อคลิก, แสดง **Chakra UI `AlertDialog`** เพื่อยืนยันการลบบุ๊คมาร์ค *ทั้งหมด* ของผู้ใช้ปัจจุบัน
-   **การทำงาน:** การลบหรือเพิ่มบุ๊คมาร์ค (จากหน้าอ่าน) จะอัปเดตข้อมูลในตาราง `user_bookmarks` และหน้านี้ควรแสดงข้อมูลล่าสุด

### 10.4 แดชบอร์ดนักแปล (Translator Dashboard - `pages/translator/dashboard.tsx`)

-   **วัตถุประสงค์:** ให้นักแปลดูภาพรวมของนิยายที่ตนเองสร้าง/จัดการ และเข้าถึงเครื่องมือที่เกี่ยวข้อง
-   **การดึงข้อมูล:** ดึงข้อมูลนิยายจากตาราง `novels` ที่ `created_by = auth.uid()`
-   **ส่วนประกอบ:**
    1.  **ส่วนหัว:** "แดชบอร์ดนักแปล"
    2.  **รายการนิยายที่จัดการ (แสดงเป็น **Chakra UI `Card`** หรือ Grid Layout):**
        -   **แต่ละการ์ดนิยาย:**
            -   รูปปกนิยาย, ชื่อนิยาย
            -   สถานะนิยาย (ongoing, completed)
            -   จำนวนตอนทั้งหมด
            -   **ความคืบหน้าการมาร์ค (Marking Progress):**
                -   คำนวณ % ของตอนทั้งหมดในนิยายนี้ที่มีการมาร์คบรรทัดไว้แล้ว (จาก `mark_lines` ที่มี `marked_line_numbers` ไม่ใช่ '[]')
                -   แสดงด้วย **Chakra UI `Progress` Bar** และข้อความ "ความคืบหน้าการมาร์ค: X%"
            -   **ปุ่มดำเนินการ:**
                -   ปุ่ม "จัดการบรรทัดมาร์ค" (**Chakra UI `Button`**): Link ไปยังหน้า `/translator/manage-marks/[novel_slug]` (ข้อ 10.4.1)
                -   ปุ่ม "ไปยังหน้าข้อมูลนิยาย" (**Chakra UI `Button`** variant="outline"): Link ไปยังหน้า `/novels/[novel_slug]`
                -   ปุ่ม "แก้ไขนิยาย" (**Chakra UI `Button`** variant="ghost"): เปิด Modal แก้ไขข้อมูลนิยาย (ข้อ 10.2.1)
    3.  (Optional) สถิติรวม: จำนวนนิยายที่แปล, จำนวนตอนทั้งหมด, จำนวนคำทั้งหมด ฯลฯ

### 10.4.1 หน้าจัดการบรรทัดมาร์ค (Mark Line Management Page - `pages/translator/manage-marks/[novel_slug].tsx`)

-   **วัตถุประสงค์:** ให้นักแปลจัดการ (ดู, คัดลอก, ดาวน์โหลด) บรรทัดที่มาร์คไว้สำหรับนิยายที่เลือก
-   **การดึงข้อมูล:** ดึงข้อมูลนิยาย (`novels.title`, `novels.id`) และรายการตอนทั้งหมด (`chapters`) ของนิยายนี้ พร้อมข้อมูล `mark_lines` ที่เกี่ยวข้อง (จำนวนบรรทัดที่มาร์ค)
-   **ส่วนประกอบ:**
    1.  **ส่วนหัว:** "จัดการบรรทัดมาร์คสำหรับ: [ชื่อนิยาย]"
    2.  **ส่วนควบคุมหลัก:**
        -   ปุ่ม "เลือกตอนเพื่อดำเนินการหลายรายการ" (Batch Mode Toggle): **Chakra UI `Switch`** หรือ `Button`. เมื่อเปิด Batch Mode, Checkbox จะปรากฏหน้าแต่ละตอน
    3.  **การแสดงผลสารบัญตอน (ใช้ **Chakra UI `Accordion`** หรือ List ที่มีการแบ่งกลุ่ม):**
        -   **หัวกลุ่มตอน (ถ้ามีการแบ่งกลุ่ม เช่น ทุกๆ 50 ตอน):**
            -   (ถ้า Batch Mode เปิดอยู่) **Chakra UI `Checkbox`** "เลือกทั้งหมดในกลุ่มนี้"
        -   **แต่ละรายการตอน:**
            -   (ถ้า Batch Mode เปิดอยู่) **Chakra UI `Checkbox`** สำหรับเลือกตอนนี้
            -   หมายเลขตอน, ชื่อตอน
            -   จำนวนบรรทัดที่มาร์ค (จาก `mark_lines.marked_line_numbers.length`)
            -   สถานะการมาร์ค: ไอคอน **Font Awesome** (เช่น `faCheckCircle` ถ้ามีมาร์ค, `faCircle` หรือ `faTimesCircle` ถ้ายังไม่มีมาร์ค หรือมาร์คยังไม่สมบูรณ์ตามเกณฑ์ที่อาจกำหนด)
            -   **ปุ่มดำเนินการสำหรับตอนนี้:**
                -   ปุ่ม "คัดลอกมาร์คตอนนี้" (**Chakra UI `IconButton`** `icon={<FontAwesomeIcon icon={faCopy} />}`): คัดลอก `marked_line_numbers` (ที่แปลงเป็นข้อความที่อ่านง่าย) ของตอนนี้ไป clipboard
                -   ปุ่ม "ไปที่หน้าอ่าน/แปลตอนนี้" (**Chakra UI `IconButton`** `icon={<FontAwesomeIcon icon={faExternalLinkAlt} />}`): Link ไปยังหน้าอ่านของตอนนี้
    4.  **แถบเครื่องมือดำเนินการหลายรายการ (Batch Actions Toolbar - ใช้ Chakra UI `Flex` หรือ `Stack`, จะแสดงเมื่อ Batch Mode เปิดอยู่และมีตอนถูกเลือกอย่างน้อยหนึ่งตอน):**
        -   ปุ่ม "คัดลอกบรรทัดมาร์คของตอนที่เลือก" (**Chakra UI `Button`**): รวม `marked_line_numbers` จากทุกตอนที่เลือกแล้วคัดลอกไป clipboard
        -   ปุ่ม "ดาวน์โหลดไฟล์บรรทัดมาร์คของตอนที่เลือก" (**Chakra UI `Button`**): เปิด **Chakra UI `Modal`** เพื่อให้ผู้ใช้เลือกรูปแบบการดาวน์โหลด:
            -   **Modal ดาวน์โหลด:**
                -   **รูปแบบไฟล์:** **Chakra UI `RadioGroup`**. ตัวเลือก:
                    -   "รวมเป็นไฟล์ .txt เดียว" (Concatenated .txt file)
                    -   "แยกเป็นหลายไฟล์ .txt ใน .zip" (Separate .txt files in a .zip archive - ใช้ JSZip)
                -   ปุ่ม "ดาวน์โหลด", "ยกเลิก"
-   **การทำงาน:** การคัดลอก/ดาวน์โหลดจะดึงข้อมูล `marked_line_numbers` จากตาราง `mark_lines` และประมวลผลตามที่ผู้ใช้เลือก

### 10.5 หน้าอ่านนิยายและโหมดแปล (Reading & Translator Mode UI - `pages/novels/[novel_slug]/[chapter_number].tsx`)

-   **วัตถุประสงค์:** แสดงเนื้อหานิยายให้ผู้อ่าน และมีเครื่องมือพิเศษสำหรับนักแปลเมื่อเปิด "โหมดนักแปล"
-   **การดึงข้อมูล:** ดึงข้อมูลตอนปัจจุบัน (`chapters` table) และข้อมูลนิยาย (`novels` table). ตรวจสอบสิทธิ์การเข้าถึงของผู้ใช้ (`chapters.access_level`). ดึงข้อมูลบุ๊คมาร์ค (`user_bookmarks`) และสถานะการอ่าน (`user_chapter_reads`) ของผู้ใช้สำหรับตอนนี้. ดึงข้อมูล `mark_lines` ถ้าเป็นนักแปลและเปิดโหมดนักแปล.
-   **ส่วนประกอบ:**
    1.  **การแสดงผลเนื้อหา (`chapters.content_original`):**
        -   Render HTML จาก Rich Text Editor อย่างปลอดภัย (DOMPurify) ภายใน **Chakra UI `Box`** ที่มีการจัดสไตล์ให้อ่านง่าย (font, line-height, max-width)
        -   ต้องไม่มี Horizontal Scroll
        -   ปุ่ม "เลื่อนขึ้นบนสุด" (Scroll to Top): **Chakra UI `IconButton`** `icon={<FontAwesomeIcon icon={faArrowUp} />}` ที่ปรากฏเมื่อผู้ใช้เลื่อนลงมาพอสมควร และเป็น `position: fixed`.
    2.  **โหมดการอ่าน (Reading Mode - สำหรับผู้ใช้ทั่วไปและนักแปลที่ยังไม่เปิดโหมดนักแปลเฉพาะนิยาย):**
        -   **แถบเครื่องมือหลักด้านบน (Top Reading Toolbar - ใช้ Chakra UI `Flex`, `position: sticky`, `top: 0`, `zIndex: sticky`):**
            -   ปุ่ม "กลับไปหน้าข้อมูลนิยาย" (**Chakra UI `IconButton`** `icon={<FontAwesomeIcon icon={faBookOpen} />}` หรือชื่อนิยายที่เป็น Link)
            -   ชื่อตอนปัจจุบัน
            -   ปุ่ม "สารบัญ" (**Chakra UI `IconButton`** `icon={<FontAwesomeIcon icon={faList} />}`): เปิด **Chakra UI `Modal`** หรือ **`Drawer`** แสดงสารบัญของนิยายปัจจุบัน (เหมือนในหน้าข้อมูลนิยาย แต่เน้นการนำทาง)
            -   ปุ่ม "ปรับแต่ง (Aa)" (**Chakra UI `IconButton`** `icon={<FontAwesomeIcon icon={faCog} />}` หรือ `faFont`): เปิด **Chakra UI `Popover`** หรือ **`Menu`** ที่มีตัวเลือก:
                -   ปรับขนาดฟอนต์ (Font Size Slider หรือ Buttons)
                -   เลือก Font Family (ตัวเลือกพื้นฐาน เช่น Sarabun, Noto Sans)
                -   ปรับระยะห่างบรรทัด (Line Height Options)
                -   (สถานะการตั้งค่าเหล่านี้จะบันทึกลง `profiles.user_settings.reading_prefs`)
            -   ปุ่ม "บุ๊คมาร์ค" (**Chakra UI `IconButton`** `icon={<FontAwesomeIcon icon={isBookmarked ? faBookmarkSolid : faBookmark} />}`): คลิกเพื่อเพิ่ม/ลบตอนนี้ออกจาก `user_bookmarks`. แสดง **Chakra UI Toast** ยืนยัน.
            -   ปุ่ม "สลับธีม" (**Chakra UI `IconButton`** `icon={<FontAwesomeIcon icon={colorMode === 'light' ? faMoon : faSun} />}`): สลับ Light/Dark mode.
            -   ปุ่ม "อ่านต่อเนื่อง" (**Chakra UI `IconButton`** `icon={<FontAwesomeIcon icon={faInfinity} />}`): สลับเปิด/ปิดการโหลดตอนถัดไปอัตโนมัติ (Infinite Scroll ใช้ Intersection Observer API).
            -   ปุ่มนำทางตอน: "< ก่อนหน้า" (**Chakra UI `Button`**), "ถัดไป >" (**Chakra UI `Button`**). (ปุ่มจะ disabled ถ้าเป็นตอนแรก/ตอนสุดท้าย)
    3.  **โหมดนักแปล (Translator Mode - เมื่อเปิดใช้งานสำหรับนิยายนี้ และผู้ใช้เป็นนักแปลเจ้าของ):**
        -   **การเปลี่ยนแปลงในแถบเครื่องมือหลัก:**
            -   เพิ่มปุ่ม "คัดลอกมาร์คทั้งหมดของตอนนี้" (**Chakra UI `IconButton`** `icon={<FontAwesomeIcon icon={faCopy} />}`): คัดลอก `marked_line_numbers` ที่แปลงเป็นข้อความ
        -   **การแสดงผลเนื้อหา:**
            -   **แถบเลขบรรทัด (Line Numbers Bar):** แสดงทางซ้ายของเนื้อหาแต่ละบรรทัด (คำนวณจากการ split `content_original` ด้วย newline หรือ <p> tags)
            -   **Checkbox สำหรับมาร์ค:** **Chakra UI `Checkbox`** แสดงทางขวาของเนื้อหาแต่ละบรรทัด, เชื่อมโยงกับ `marked_line_numbers` ใน `mark_lines`.
        -   **ปฏิสัมพันธ์กับการมาร์คบรรทัด (MarkLine Interaction):**
            -   **คลิก Checkbox:**
                -   อัปเดต `mark_lines.marked_line_numbers` (เพิ่ม/ลบ line number นั้น) ใน Supabase แบบ Real-time หรือ Debounced (ใช้ Next.js API Route + React Query `useMutation`).
                -   แสดง **Chakra UI Toast** สั้นๆ "มาร์คบรรทัด X" หรือ "ยกเลิกมาร์คบรรทัด X".
            -   **ดับเบิลคลิกที่บรรทัดเนื้อหา:**
                -   คัดลอกเนื้อหาบรรทัดนั้นไปยัง Clipboard.
                -   ทำการมาร์คบรรทัดนั้น (เหมือนคลิก Checkbox).
                -   แสดง **Chakra UI Toast** "คัดลอกและมาร์คบรรทัด X".
        -   **ท้ายบทในโหมดนักแปล:**
            -   ปุ่ม "ล้างมาร์คทั้งหมดในตอนนี้" (**Chakra UI `Button`** colorScheme="red"): แสดง **Chakra UI `AlertDialog`** เพื่อยืนยันการล้าง `marked_line_numbers` ทั้งหมดของตอนนี้ใน `mark_lines`.
    4.  **การบันทึกสถานะการอ่าน:** เมื่อผู้ใช้เปิดหน้าอ่านตอนนี้, ระบบจะบันทึก/อัปเดต record ใน `user_chapter_reads`

### 10.6 แดชบอร์ดผู้ดูแลระบบ (Admin Dashboard - `pages/admin/dashboard/*`)

-   **วัตถุประสงค์:** ให้ Admin จัดการส่วนต่างๆ ของระบบ
-   **โครงสร้าง (Admin Layout Component - ใช้ `_app.tsx` หรือ HOC เพื่อ wrap pages ใน `/admin/dashboard`):**
    -   **Sidebar (ใช้ **Chakra UI `VStack`** หรือ `Box` ที่มี `position: fixed` และสไตล์):**
        -   โลโก้/ชื่อเว็บไซต์
        -   รายการลิงก์ (ใช้ Next.js `<Link>` กับ **Chakra UI `Link`** หรือ `Button` styling) ไปยังส่วนต่างๆ ของ Admin Dashboard:
            -   "ภาพรวม" (`/admin/dashboard`) - ไอคอน **Font Awesome** `faChartLine`
            -   "จัดการผู้ใช้" (`/admin/dashboard/users`) - ไอคอน **Font Awesome** `faUsersCog`
            -   "จัดการหมวดหมู่นิยาย" (`/admin/dashboard/categories`) - ไอคอน **Font Awesome** `faTags`
            -   "จัดการนิยายทั้งหมด" (`/admin/dashboard/novels`) - ไอคอน **Font Awesome** `faBook`
            -   "จัดการประกาศ" (`/admin/dashboard/announcements`) - ไอคอน **Font Awesome** `faBullhorn`
            -   "จัดการเว็บไซต์" (`/admin/dashboard/settings`) - ไอคอน **Font Awesome** `faCogs`
            -   "จัดการเวอร์ชัน/Changelog" (`/admin/dashboard/versions`) - ไอคอน **Font Awesome** `faHistory`
    -   **Content Area:** ส่วนที่แสดงเนื้อหาของแต่ละหน้าใน Admin Dashboard

#### 10.6.1 หน้าภาพรวม (Admin Dashboard Overview - `pages/admin/dashboard/index.tsx`)

-   แสดงสถิติสำคัญของระบบในรูปแบบ **Chakra UI `Card`** หรือ `Stat` components:
    -   จำนวนผู้ใช้ทั้งหมด, ผู้ใช้ใหม่ในสัปดาห์นี้
    -   จำนวนนิยายทั้งหมด, นิยายใหม่
    -   จำนวนตอนทั้งหมด, ตอนใหม่
    -   กิจกรรมล่าสุด (เช่น การลงทะเบียนใหม่, นิยายใหม่, ตอนใหม่)
    -   (อาจมีกราฟง่ายๆ ด้วย Chart.js หรือ Recharts ถ้าจำเป็น)

#### 10.6.2 หน้าจัดการผู้ใช้ (User Management)

-   **หน้ารายการผู้ใช้ (`pages/admin/dashboard/users/index.tsx`):**
    -   **เครื่องมือค้นหา/ฟิลเตอร์:**
        -   **Chakra UI `Input`** สำหรับค้นหาตาม Email, Display Name
        -   **Chakra UI `Select`** สำหรับฟิลเตอร์ตาม Role (`roles.name`)
    -   **ตารางผู้ใช้ (User Table - ใช้ **Chakra UI `Table`, `Thead`, `Tbody`, `Tr`, `Th`, `Td`**):**
        -   คอลัมน์: Avatar (`profiles.avatar_url`), Email (`auth.users.email`), ชื่อที่แสดง (`profiles.display_name`), สิทธิ์/บทบาท (`roles.name` - แสดงด้วย **Chakra UI `Badge`**), วันที่ลงทะเบียน (`auth.users.created_at`), สถานะ (`profiles.is_suspended`), ปุ่ม "จัดการ"
        -   ปุ่ม "จัดการ": **Chakra UI `IconButton`** `icon={<FontAwesomeIcon icon={faCog} />}` หรือ `faEdit` Link ไปยังหน้าแก้ไขผู้ใช้ (`/admin/dashboard/users/[user_id]`)
    -   **Pagination** สำหรับรายการผู้ใช้
-   **หน้าแก้ไขผู้ใช้ (`pages/admin/dashboard/users/[user_id].tsx`):**
    -   ดึงข้อมูลผู้ใช้จาก `auth.users` และ `profiles` ตาม `user_id`
    -   **ฟอร์มแก้ไขข้อมูลผู้ใช้ (ใช้ Chakra UI `FormControl`, `FormLabel`, `Input`, `Select`, `Switch`, `Button`):**
        -   แสดง Avatar ปัจจุบัน, อาจมีปุ่มให้ Admin อัปโหลดใหม่
        -   Email (แสดง, readonly)
        -   ชื่อที่แสดง (Display Name - แก้ไขได้)
        -   สิทธิ์/บทบาท (Role): **Chakra UI `Select`** ดึงข้อมูลจาก `roles` table. Admin สามารถเปลี่ยนบทบาทผู้ใช้ได้.
        -   วันหมดอายุบทบาท (Role Expiry Date): **Chakra UI `Input type="date"`** หรือ `react-datepicker` ที่ styled ให้เข้ากับ Chakra UI.
        -   สถานะการระงับ (Is Suspended): **Chakra UI `Switch`**.
        -   (Optional) ปุ่ม "รีเซ็ตรหัสผ่าน" (ส่งอีเมลให้ผู้ใช้), ปุ่ม "ลบบัญชี" (แสดง `AlertDialog`)
    -   ปุ่ม: "บันทึกการเปลี่ยนแปลง" (**Chakra UI `Button`** colorScheme="blue")

#### 10.6.3 หน้าจัดการหมวดหมู่นิยาย (`pages/admin/dashboard/categories.tsx`)

-   ปุ่ม "เพิ่มหมวดหมู่ใหม่" (**Chakra UI `Button`**): เปิด **Chakra UI `Modal`** สำหรับเพิ่ม/แก้ไขหมวดหมู่
-   **Modal เพิ่ม/แก้ไขหมวดหมู่:**
    -   ฟิลด์: ชื่อหมวดหมู่ (`categories.name`, Required, Unique), Slug (`categories.slug`, Required, Unique, แนะนำจากชื่อ), คำอธิบาย (`categories.description`)
    -   ปุ่ม: "บันทึก", "ยกเลิก"
-   **รายการหมวดหมู่ (แสดงเป็น **Chakra UI `Table`** หรือ List):**
    -   คอลัมน์: ชื่อ, Slug, จำนวนนิยายในหมวดนี้, ปุ่ม "แก้ไข", ปุ่ม "ลบ"
    -   ปุ่ม "แก้ไข": เปิด Modal เดิมแต่เป็นโหมดแก้ไข
    -   ปุ่ม "ลบ": แสดง **Chakra UI `AlertDialog`**. อาจมีตัวเลือกให้ย้ายนิยายในหมวดนี้ไปยังหมวดหมู่อื่น หรือตั้งเป็นไม่มีหมวดหมู่ (`category_id = NULL`) ก่อนลบ

#### 10.6.4 หน้าจัดการประกาศ (`pages/admin/dashboard/announcements.tsx`)

-   ปุ่ม "สร้างประกาศใหม่" (**Chakra UI `Button`**): เปิด **Chakra UI `Modal`** สำหรับสร้าง/แก้ไขประกาศ
-   **ตารางประกาศ (ใช้ **Chakra UI `Table`**):**
    -   คอลัมน์: หัวข้อ (`announcements.title`), สถานะ (`announcements.is_active` - แสดงด้วย **Chakra UI `Badge`** "Active"/"Inactive"), วันที่เริ่ม-สิ้นสุด, กลุ่มเป้าหมาย, หน้าเป้าหมาย, ปุ่ม "แก้ไข", ปุ่ม "ลบ", ปุ่ม "สลับสถานะ Active/Inactive"
-   **Modal สร้าง/แก้ไขประกาศ:**
    -   ฟิลด์: หัวข้อ (`announcements.title`), เนื้อหา (`announcements.content` - ใช้ Markdown Editor หรือ Rich Text Editor), วันที่เริ่มแสดง (`announcements.start_date` - Date Picker), วันที่สิ้นสุดการแสดง (`announcements.end_date` - Date Picker), หน้าที่แสดง (`announcements.target_pages` - Multi-select หรือ Checkbox Group จากรายการหน้าหลักๆ), กลุ่มผู้รับ (`announcements.target_roles` - Multi-select หรือ Checkbox Group จาก `roles` table), สถานะ Active/Inactive (`announcements.is_active` - **Chakra UI `Switch`**)
    -   ปุ่ม: "บันทึก", "ยกเลิก"

#### 10.6.5 หน้าจัดการเว็บไซต์ (`pages/admin/dashboard/settings.tsx`)

-   UI สำหรับแก้ไขค่าต่างๆ ในตาราง `site_settings` (แสดงเป็น **Chakra UI `Card`** หรือกลุ่มของ `FormControl`):
    -   `site_name`: ชื่อเว็บไซต์ (Input Text)
    -   `site_logo_url`: URL โลโก้ (Input Text หรือ File Upload ไปยัง `site_assets` bucket)
    -   `site_favicon_url`: URL Favicon (Input Text หรือ File Upload)
    -   `allow_registration`: อนุญาตให้ลงทะเบียนใหม่หรือไม่ (Boolean - **Chakra UI `Switch`**)
    -   `default_novel_visibility`: ค่าเริ่มต้น visibility สำหรับนิยายใหม่ (JSON Array - อาจใช้ Multi-select)
    -   `default_novel_chapter_status`: ค่าเริ่มต้นสถานะตอนสำหรับนิยายใหม่ (Text - Select)
    -   ... และการตั้งค่าอื่นๆ ที่จำเป็น
-   ปุ่ม "บันทึกการตั้งค่าทั้งหมด" (**Chakra UI `Button`**)

#### 10.6.6 หน้าจัดการเวอร์ชัน/Changelog (`pages/admin/dashboard/versions.tsx`)

-   **Layout ใช้ **Chakra UI `Tabs`** (`TabList`, `TabPanels`, `Tab`, `TabPanel`):**
    -   **Tab 1: Changelog เว็บไซต์ (`site_changelogs`):**
        -   **ตาราง Changelog (Chakra UI `Table`):** แสดง `version_number`, `release_date`, `summary` (ส่วนหนึ่ง), `is_published` (Badge).
        -   ปุ่ม "เพิ่ม Changelog ใหม่" (**Chakra UI `Button`**): เปิด Modal
        -   **Modal เพิ่ม/แก้ไข Changelog:** ฟิลด์ `version_number`, `release_date`, `summary` (Markdown), `is_published` (Switch). ปุ่ม "บันทึก".
        -   ในตาราง: ปุ่ม "แก้ไข", "ลบ", "สลับสถานะ Published".
    -   **Tab 2: ประวัติเวอร์ชันนิยายและตอน (`novel_versions`, `chapter_versions`):**
        -   **เครื่องมือค้นหา:** **Chakra UI `Input`** (ค้นหานิยายตามชื่อ) หรือ `Select` (เลือกนิยาย)
        -   เมื่อเลือกนิยาย:
            -   **ส่วนประวัติเวอร์ชันนิยาย:** แสดงรายการ `novel_versions` ของนิยายนั้นใน **Chakra UI `Table`** (Version Number, วันที่, ผู้แก้ไข, หมายเหตุ). ปุ่ม "ดูรายละเอียด Snapshot" (เปิด Modal แสดง JSON), (Optional) ปุ่ม "กู้คืนไปยังเวอร์ชันนี้" (แสดง `AlertDialog`).
            -   **ส่วนประวัติเวอร์ชันตอน:** `Select` เพื่อเลือกตอนของนิยายนั้น แล้วแสดงรายการ `chapter_versions` ใน **Chakra UI `Table`** (Version Number, วันที่, ผู้แก้ไข, หมายเหตุ). ปุ่ม "ดูรายละเอียด Snapshot เนื้อหา" (เปิด Modal แสดง Text), (Optional) ปุ่ม "กู้คืน".
-   **การกู้คืน (Restore):** เป็นฟังก์ชันที่ซับซ้อน ควรมีการยืนยันหลายขั้นตอน และอาจสร้างเวอร์ชันใหม่จาก snapshot เก่า แทนการเขียนทับโดยตรง

### 10.7 หน้าโปรไฟล์ (Profile Page - `pages/profile.tsx`)

-   **วัตถุประสงค์:** ให้ผู้ใช้ปัจจุบันจัดการข้อมูลส่วนตัวของตนเอง
-   **การดึงข้อมูล:** ดึงข้อมูลผู้ใช้ปัจจุบันจาก `auth.users` และ `profiles` (ใช้ `auth.uid()`)
-   **UI (ใช้ **Chakra UI `Card`, `Avatar`, `FormControl`, `FormLabel`, `Input`, `Button`, `Modal`, `AlertDialog`, `Badge`, `Stack`**):**
    1.  **ส่วนรูปโปรไฟล์:**
        -   แสดง **Chakra UI `Avatar`** ปัจจุบัน (`profiles.avatar_url`)
        -   ปุ่ม "เปลี่ยนรูปโปรไฟล์": เปิด **Chakra UI `Modal`** ที่มี `Input type="file"` และ `react-image-crop` (ควร crop เป็นสี่เหลี่ยมจัตุรัส 1:1). อัปโหลดไปยัง `avatars` bucket.
    2.  **ส่วนข้อมูลส่วนตัว:**
        -   **ชื่อที่แสดง (Display Name):**
            -   แสดง `profiles.display_name` ปัจจุบัน
            -   **Chakra UI `Input`** สำหรับแก้ไข, พร้อมปุ่ม "บันทึกชื่อที่แสดง"
        -   **สิทธิ์/บทบาท (Role):**
            -   แสดง `roles.name` ปัจจุบันด้วย **Chakra UI `Badge`** (เช่น "Member", "Translator")
            -   (ถ้ามี `role_expiry_date`, แสดงข้อมูลนั้นด้วย)
        -   **อีเมล (Email):**
            -   แสดง `auth.users.email` ปัจจุบัน (readonly หรือมีกระบวนการเปลี่ยนอีเมลที่ซับซ้อนกว่าผ่าน Supabase Auth)
    3.  **ส่วนการตั้งค่าบัญชี:**
        -   **เปลี่ยนรหัสผ่าน:**
            -   ปุ่ม "เปลี่ยนรหัสผ่าน": เปิด **Chakra UI `Modal`**
            -   **Modal เปลี่ยนรหัสผ่าน:** ฟอร์มมีฟิลด์: รหัสผ่านปัจจุบัน, รหัสผ่านใหม่, ยืนยันรหัสผ่านใหม่ (ใช้ React Hook Form + Zod). เรียก `supabase.auth.updateUser({ password: newPassword })`.
        -   **(Optional) การตั้งค่าการอ่าน (Reading Preferences):**
            -   ตัวเลือกสำหรับ ขนาดฟอนต์เริ่มต้น, Font Family เริ่มต้น, Line Height เริ่มต้น (บันทึกลง `profiles.user_settings.reading_prefs`)
        -   **(Optional) การตั้งค่าธีม (Theme Preference):**
            -   ตัวเลือกบังคับ Light/Dark/System (บันทึกลง `profiles.user_settings.theme_preference`)
    4.  **ส่วนการออกจากระบบ:**
        -   ปุ่ม "ออกจากระบบ" (**Chakra UI `Button`** colorScheme="red"): แสดง **Chakra UI `AlertDialog`** เพื่อยืนยันการออกจากระบบ (`supabase.auth.signOut()`).

## 11. Key User Experience Flows & Enhancements

-   **การล็อกอิน/ลงทะเบียน:**
    -   ราบรื่นผ่าน **Chakra UI Modal** โดยไม่เปลี่ยนหน้า (ถ้าคลิกจาก Navbar)
    -   Feedback ชัดเจนด้วย **Chakra UI Toast** สำหรับทุกสถานะ (กำลังดำเนินการ, สำเร็จ, ผิดพลาด)
-   **การนำทาง (Navigation):**
    -   Navbar ที่ใช้งานง่ายและ responsive
    -   Admin Sidebar ที่ชัดเจนและเข้าถึงง่าย (ใช้ **Chakra UI `Flex`, `VStack`, Next.js `<Link>` styled as `Button` or `LinkBox`**)
-   **เครื่องมือช่วยแปล (Translator Tools):**
    -   การมาร์คบรรทัด: ตอบสนองทันที (Real-time หรือ Debounced update) ด้วย React Query `useMutation` ส่งข้อมูลไปยัง Next.js API Route ที่จะอัปเดต `mark_lines` ใน Supabase. อาจพิจารณา Supabase Realtime subscriptions เพื่อซิงค์สถานะถ้ามีหลายคนทำงานพร้อมกัน (ซึ่งอาจไม่อยู่ใน scope ปัจจุบัน).
    -   **Chakra UI Toast** feedback สั้นๆ เมื่อมาร์ค/ยกเลิกการมาร์ค หรือคัดลอก
-   **การตอบสนองของระบบ (Responsiveness & Feedback):**
    -   **Loading States:**
        -   เมื่อมีการดึงข้อมูล: แสดง **Chakra UI `Spinner`** ในบริเวณที่ข้อมูลจะปรากฏ หรือ **Chakra UI `Skeleton`** เพื่อจำลองโครงสร้างของเนื้อหาที่กำลังโหลด
        -   เมื่อกดปุ่มที่ต้องใช้เวลาประมวลผล: ปุ่มควรมีสถานะ loading (เช่น **Chakra UI `Button isLoading` prop**)
    -   **Error Handling:**
        -   **Chakra UI Toast** สำหรับข้อผิดพลาดทั่วไปที่ผู้ใช้ควรทราบ (เช่น "ไม่สามารถบันทึกข้อมูลได้", "ไฟล์ใหญ่เกินไป")
        -   ข้อความใน UI ที่ชัดเจนหากฟอร์มไม่ผ่าน validation (ผ่าน React Hook Form + Zod + **Chakra UI `FormErrorMessage`**)
-   **การแจ้งเตือนการกระทำ (Action Confirmations):**
    -   ใช้ **Chakra UI Toast** เพื่อยืนยันการกระทำที่สำเร็จ (เช่น "บันทึกข้อมูลสำเร็จ", "สร้างนิยายใหม่แล้ว")
    -   ใช้ **Chakra UI `AlertDialog`** สำหรับการกระทำที่ irreversible (เช่น ลบข้อมูล) เพื่อให้ผู้ใช้ยืนยันอีกครั้ง
-   **การสร้าง `mark_lines` อัตโนมัติ:**
    -   เมื่อมีการสร้างตอนใหม่ (ทั้งแบบเดี่ยวและ batch upload), ระบบจะต้องสร้าง record ที่สอดคล้องกันในตาราง `mark_lines` โดยอัตโนมัติ (ด้วย `marked_line_numbers` เป็น `[]`) เพื่อให้พร้อมสำหรับนักแปล
-   **การคงสถานะหน้าเมื่อรีเฟรช:**
    -   สถานะ UI ส่วนใหญ่ (เช่น หน้าปัจจุบันของ pagination, ค่าในฟิลเตอร์) ควรถูกเก็บใน URL query parameters เพื่อให้เมื่อผู้ใช้รีเฟรชหน้าหรือแชร์ URL จะยังคงเห็นสถานะเดิม

## 12. การจัดการข้อผิดพลาดเบื้องต้น (Basic Error Handling)

-   **Client-Side Validation:**
    -   ใช้ **Zod** schemas ร่วมกับ **React Hook Form** เพื่อ validate ข้อมูลในฟอร์มก่อนส่งไป server
    -   แสดงข้อความผิดพลาดที่ชัดเจนใต้ฟิลด์ที่ไม่ถูกต้องโดยใช้ **Chakra UI `FormErrorMessage`**
-   **Server-Side Validation:**
    -   Validate ข้อมูลอีกครั้งใน Next.js API Routes หรือ Supabase Edge Functions (อาจใช้ Zod schema เดียวกันกับ client-side) เพื่อความปลอดภัย
    -   ส่ง response ที่มี status code และข้อความผิดพลาดที่เหมาะสมกลับไปยัง client
-   **User-Facing Error Messages:**
    -   ใช้ **Chakra UI Toast** สำหรับข้อผิดพลาดที่ไม่เกี่ยวข้องกับฟอร์มโดยตรง (เช่น Network error, Server error)
    -   ข้อความควรเป็นมิตรกับผู้ใช้และให้ข้อมูลที่พอจะเข้าใจได้ (หลีกเลี่ยงการแสดง technical jargon)
-   **Code-Level Error Handling:**
    -   ใช้ `try...catch` blocks ในส่วนที่ติดต่อกับ API หรือ Supabase
    -   ตรวจสอบ `{ data, error }` object ที่ได้จาก Supabase client library
-   **Error Logging (Optional but Recommended for Production):**
    -   `console.error` ในระหว่างการพัฒนา
    -   พิจารณาใช้บริการ Error Tracking เช่น Sentry หรือ Logtail สำหรับ production เพื่อรวบรวมและวิเคราะห์ข้อผิดพลาดที่เกิดขึ้นกับผู้ใช้จริง
    -   Supabase มี Logs Dashboard ให้ตรวจสอบ

## 13. มาตรการความปลอดภัยเบื้องต้น (Basic Security Measures)

-   **Supabase Row Level Security (RLS):** เปิดใช้งาน RLS ในทุกตารางที่มีข้อมูล sensitive เพื่อให้แน่ใจว่าผู้ใช้เข้าถึงได้เฉพาะข้อมูลที่ตนมีสิทธิ์
-   **Input Validation:** ทั้ง Client-side (Zod + React Hook Form) และ Server-side (Zod ใน API Routes/Edge Functions) เพื่อป้องกันข้อมูลที่ไม่ถูกต้องหรือ malicious
-   **HTTPS:** บังคับใช้ HTTPS เสมอ (Supabase และ Vercel/Next.js hosting platforms ส่วนใหญ่ทำให้อัตโนมัติ)
-   **Environment Variables:** เก็บ Sensitive Keys (Supabase API keys, database connection strings - ถ้าไม่ได้ใช้ Supabase client โดยตรง) ไว้ใน Environment Variables (`.env.local`, และตั้งค่าใน hosting platform) อย่า commit ขึ้น Git repository
-   **CSRF Protection:** Next.js API Routes ส่วนใหญ่มีการป้องกัน CSRF พื้นฐาน แต่ถ้ามีการใช้ฟอร์มที่ซับซ้อนหรือ sensitive มาก อาจต้องพิจารณามาตรการเพิ่มเติม (เช่น csurf library ถ้าไม่ได้ใช้ API routes แบบมาตรฐาน)
-   **HTTP Security Headers:** ตั้งค่า Security Headers ที่เหมาะสม (เช่น `Content-Security-Policy`, `X-Content-Type-Options`, `X-Frame-Options`, `Strict-Transport-Security`) ผ่าน `next.config.js` หรือ middleware
-   **Data Backups:** Supabase มีระบบสำรองข้อมูลอัตโนมัติ ตรวจสอบและทำความเข้าใจนโยบายการสำรองข้อมูล
-   **Principle of Least Privilege:** ให้สิทธิ์ผู้ใช้ (ทั้งใน RLS และ application logic) น้อยที่สุดเท่าที่จำเป็นในการทำงาน
-   **Regular Updates:** อัปเดต dependencies (Next.js, Chakra UI, Supabase client, etc.) อย่างสม่ำเสมอเพื่อแก้ไขช่องโหว่ที่ค้นพบ
-   **Sanitize HTML Output:**
    -   เมื่อมีการ render HTML ที่มาจากผู้ใช้ (เช่น `novels.synopsis`, `chapters.content_original` ที่เก็บจาก Rich Text Editor) ด้วย `dangerouslySetInnerHTML`, ต้องทำการ sanitize HTML นั้นก่อนด้วย DOMPurify เพื่อป้องกัน XSS attacks.
    -   Chakra UI components ส่วนใหญ่จะ handle การแสดงผลข้อความอย่างปลอดภัย แต่ถ้าใช้ `dangerouslySetInnerHTML` โดยตรง ต้องระวังเป็นพิเศษ

---

เอกสารนี้พยายามครอบคลุมทุกรายละเอียดจากข้อมูลที่คุณให้มา หากมีส่วนใดต้องการขยายความเพิ่มเติม หรือมีข้อสงสัย สามารถสอบถามได้เลยครับ
