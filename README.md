# 📖 Kunpeng Novel เว็บไซต์นิยายภาษาไทย - ฟีเจอร์ทั้งหมด

## 🧭 ภาพรวม

เว็บไซต์นิยายออนไลน์ สำหรับผู้อ่าน นักแปล และแอดมิน มีระบบมาร์กบรรทัด Responsive รองรับมือถือ พร้อมระบบล็อกอินและกำหนดสิทธิ์ผู้ใช้ละเอียด

---

## 🔐 ระบบล็อกอินและจัดการผู้ใช้ (Authentication)

### ฟังก์ชันหลัก

* **Supabase Auth**
* **Login** (Email/Password, Google OAuth)
* **Register** (ผู้ใช้สมัครได้เอง, default role = `reader`)
* **Forgot Password** (email reset link)
* **Roles**: `reader`, `vip`, `editor`, `admin`
* **VIP**: field `is_vip: boolean`, `vip_expires_at: timestamp` (admin ตั้งค่า)

---

## 🧭 เมนูนำทาง (Navigation)

### หน้าทั่วไป (หลังล็อกอิน)

| หน้า         | URL                | คำอธิบาย            |
| ------------ | ------------------ | ------------------- |
| ล็อกอิน      | `/login`           | เข้าสู่ระบบ         |
| สมัครสมาชิก  | `/register`        | เปิดบัญชีใหม่       |
| ลืมรหัสผ่าน  | `/forgot-password` | รีเซ็ตรหัสผ่าน      |
| รายชื่อนิยาย | `/novels`          | แสดงนิยายทั้งหมด    |
| โปรไฟล์      | `/profile`         | ข้อมูลบัญชี, Logout |

### Navbar (แยกตาม Role)

| Role   | เมนู                            |
| ------ | ------------------------------- |
| reader | Novels, Bookmarks, VIP, Profile |
| vip    | Novels, Bookmarks, VIP, Profile |
| editor | Editor Dashboard, Profile       |
| admin  | Admin Dashboard, Users, Profile |

---

## 📚 ฟีเจอร์นิยาย (Novel Features)

### สร้าง/แก้ไขนิยาย

* **Fields**: title, description, cover, genre, tags
* **Access Control**: Checkbox for `reader`, `vip`, `editor` → `access_roles: text[]`
* **Editor Mode**: `editor_mode: boolean` (เปิดฟีเจอร์มาร์ก)

### สร้าง/แก้ไขบท

* **Fields**: title, content, status(`draft`/`published`)
* **Access Control**: Checkbox for `reader`, `vip`, `editor`
* **Lock Chapter**: `is_locked: boolean` + `password: text` (option)

---

## 📖 หน้าอ่านตอน (`/novel/:id/chapter/:cid`)

> **ตรวจสิทธิ์ก่อน**: user.role ∈ `chapter.access_roles` และถ้า `is_locked` ต้องกรอกรหัส

### 1. Reader Mode

* A+ / A- (ปรับฟอนต์)
* Light / Dark Theme
* Bookmark Chapter (🔖)
* Prev / Next Chapter

### 2. Editor Mode

> **เฉพาะ role=`editor`|`admin` และ `editor_mode = true`**

#### 2.1 Sticky Toolbar

* ตำแหน่ง: ด้านบน, ติดเมื่อเลื่อน (position: sticky; top: 0; z-index:50)
* ปุ่ม:

  ```text
  A+ | A- | 🔖 | 📋 Copy Marks | << Prev | Next >>
  ```
* ช่วยให้ปรับฟอนต์, บุ๊คมาร์ก, คัดลอกบรรทัดที่มาร์ก และเลื่อนไปบทก่อน/ถัดไป ได้ง่ายตลอด

#### 2.2 เนื้อหาบรรทัด

```jsx
<div className="line-row">
  <span className="line-number">3</span>
  <span className="line-text">ข้อความบรรทัดที่ 3</span>
  <input type="checkbox" className="line-checkbox" />
</div>
```

* **หมายเลขบรรทัด**: แสดงเลขตามลำดับ
* **Checkbox**: มาร์ก/ยกเลิก → ไฮไลต์พื้นหลัง
* **Double-click**: คัดลอกข้อความบรรทัด (ไม่รวมเลข)
* **Popup**: แสดง “บันทึกแล้ว ✓” หรือ “คัดลอกแล้ว ✓” 2 วินาที

#### 2.3 Copy Marks Function

* ปุ่ม 📋 จะดึง `marked_lines_json` ของ user+chapter
* อ่าน `chapters.content` ตาม `line_number`
* รวมข้อความพร้อม newline
* คัดลอกไป clipboard + popup ยืนยัน

---

## 💾 ฐานข้อมูล (Supabase)

### ตาราง `novels`

```sql
create table novels (
  id uuid primary key default uuid_generate_v4(),
  title text,
  description text,
  cover_url text,
  genre text,
  tags text[],
  access_roles text[] default array['reader'],
  editor_mode boolean default false
);
```

### ตาราง `chapters`

```sql
create table chapters (
  id uuid primary key default uuid_generate_v4(),
  novel_id uuid references novels(id),
  title text,
  content text,
  status text default 'draft',
  access_roles text[] default array['reader'],
  is_locked boolean default false,
  password text
);
```

### ตาราง `line_marks`

```sql
create table line_marks (
  id uuid primary key default uuid_generate_v4(),
  user_id uuid references auth.users(id),
  chapter_id uuid references chapters(id),
  marked_lines_json jsonb,
  updated_at timestamp default now()
);
```

* **RLS**: policy ให้ user เห็น/แก้ข้อมูลของตัวเองเท่านั้น

### ตาราง `auth.users`

```sql
alter table auth.users
add column role text default 'reader',
add column is_vip boolean default false,
add column vip_expires_at timestamp;
```

---

## ⚙️ Tech Stack

* **Frontend**: Next.js, React, Tailwind CSS
* **Backend**: Supabase (Auth, Database, Realtime)
* **Hosting**: Vercel

---

## 🔐 ความปลอดภัย

* Login guard บังคับล็อกอิน
* ตรวจ `role` และ `access_roles` ทั้งฝั่ง client & server
* RLS ป้องกันข้าม user

---

## 🛠 พัฒนาต่อในอนาคต

* Export/Import `line_marks`
* Workflow อนุมัติการแปลหลายขั้นตอน
* ค้นหา/กรองบรรทัดที่มาร์ก
* Markdown preview สำหรับ editor

---

> © 2025 Kunpeng Novel. Freedom for translators, quality for readers.
