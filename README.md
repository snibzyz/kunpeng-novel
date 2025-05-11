# 📖 Kunpeng Novel เว็บไซต์นิยายภาษาไทย - ฟีเจอร์ทั้งหมด

## 🧭 ภาพรวม

เว็บไซต์นิยายออนไลน์ สำหรับผู้อ่าน นักแปล และแอดมิน มีระบบมาร์กบรรทัด นิยายหลายภาษา รองรับมือถือ พร้อมระบบล็อกอินและการกำหนดสิทธิ์แบบละเอียด

---

## 🔐 ระบบล็อกอินและจัดการผู้ใช้ (Authentication)

### ระบบล็อกอิน (ใช้ Supabase Auth)

* ต้องล็อกอินก่อนใช้งานทุกหน้า (ยกเว้น `/login`, `/register`)
* รองรับ:

  * อีเมล + รหัสผ่าน
  * Google Login (OAuth)
* ลืมรหัสผ่าน: ส่งลิงก์รีเซ็ตผ่านอีเมล

### การสมัครสมาชิก

* ผู้ใช้สมัครเองได้
* บัญชีใหม่จะได้รับสิทธิ์เริ่มต้นเป็น `reader`

### สิทธิ์ผู้ใช้งาน (Roles)

| Role     | หน้าแรกหลังล็อกอิน  | สิทธิ์ที่ได้รับ                                        |
| -------- | ------------------- | ------------------------------------------------------ |
| `reader` | `/novels`           | อ่านนิยาย, บุ๊คมาร์ก, มาร์กบรรทัด                      |
| `vip`    | `/novels`           | สิทธิ์ reader + อ่านตอน VIP, อ่านล่วงหน้า              |
| `editor` | `/editor-dashboard` | เข้าสู่โหมดแปล, แก้ไขต้นฉบับ/แปล, ดูสถานะงานแปล        |
| `admin`  | `/admin-dashboard`  | จัดการผู้ใช้, เปลี่ยน role, ตั้ง VIP, CRUD นิยายและตอน |

### ระบบ VIP

* Admin ตั้งค่า:

  * `is_vip = true`
  * `vip_expires_at = 'YYYY-MM-DD'`
* ถ้าบทถูกตั้งว่า "เฉพาะ VIP" จะตรวจว่า:

  * `user.is_vip === true` และ `vip_expires_at > วันนี้`

---

## 🧭 หน้าใช้งานและเมนู (Navigation)

### หน้าทั่วไป (ทุกคนเข้าถึงได้หลังล็อกอิน)

| หน้า         | URL                        | คำอธิบาย                |
| ------------ | -------------------------- | ----------------------- |
| ล็อกอิน      | `/login`                   | หน้าเข้าสู่ระบบ         |
| สมัครสมาชิก  | `/register`                | หน้าเปิดบัญชีใหม่       |
| ลืมรหัสผ่าน  | `/forgot-password`         | รีเซ็ตรหัสผ่าน          |
| รายชื่อนิยาย | `/novels`                  | แสดงนิยายทั้งหมด        |
| อ่านตอนนิยาย | `/novel/<id>/chapter/<id>` | อ่านนิยาย + มาร์กบรรทัด |
| โปรไฟล์      | `/profile`                 | ข้อมูลบัญชี, ออกจากระบบ |

### Navbar ตามสิทธิ์

| Role   | รายการเมนู                                                    |
| ------ | ------------------------------------------------------------- |
| reader | หน้าแรก, บุ๊คมาร์กของฉัน, สมัคร VIP, โปรไฟล์                  |
| vip    | เช่นเดียวกับ reader                                           |
| editor | หน้าแรก, แดชบอร์ดนักแปล, โปรไฟล์                              |
| admin  | หน้าแรก, แดชบอร์ดแอดมิน, จัดการผู้ใช้, เครื่องมือแปล, โปรไฟล์ |

---

## 📚 ฟีเจอร์นิยาย (Novel Features)

### หน้านิยายหลัก (`/novel/<id>`)

* แสดง:

  * ชื่อจีนต้นฉบับ / ชื่อแปลไทย
  * ผู้เขียน / ผู้แปล
  * คำโปรย, หมวดหมู่, แท็ก
  * สถานะ (กำลังอัปเดต / จบแล้ว / หยุดพัก)
  * ตอนล่าสุด 10 ตอน พร้อมวันที่อัปเดต
  * ปุ่มแสดงช่วงตอน (1–50, 51–100, ...)

### การกำหนดสิทธิ์การเข้าถึง

* สามารถเลือกได้หลาย role: ☑️ reader, ☑️ vip, ☑️ editor
* เซฟลง `novels.access_roles: text[]`
* ถ้า user.role ∉ `access_roles` → ไม่สามารถเห็นนิยาย
* มีตัวเลือก เปิด/ปิด "โหมด editor"

  * ถ้าเปิด → Editor Mode
  * ถ้าไม่เปิด → Reader Mode

---

## 📖 หน้าอ่านตอน (`/novel/<id>/chapter/<id>`)

> เฉพาะบทที่ user.role ∈ `chapter.access_roles` เท่านั้น

### Reader Mode (นิยายทั่วไป)

* ปรับขนาดฟอนต์ (A+ / A-)
* โหมดแสง / กลางคืน
* บุ๊คมาร์กบทนี้
* ปุ่ม “ตอนก่อนหน้า” / “ตอนถัดไป”
* **ไม่มี** ระบบมาร์กบรรทัด
* **ไม่มี** ปุ่มคัดลอกข้อความที่มาร์ก

### Editor Mode (เฉพาะ role=`editor`|`admin` และเปิดโหมด editor)

> **UI Layout**: ทุกรายการแสดงในกล่องข้อความสีเข้ม พื้นหลังสีดำอ่อน ขอบมุมมน 2xl

1. **โครงสร้างหน้าอ่านตอน**

   ```jsx
   -------------------------------------------------------------
   | << ตอนก่อนหน้า | ชื่อเรื่อง — บทที่ 0001 | ตอนถัดไป >> |
   -------------------------------------------------------------
   | [1] ข้อความบรรทัดที่ 1                                   [ ] |
   | [2] ข้อความบรรทัดที่ 2                                   [ ] |
   | [3] ข้อความบรรทัดที่ 3                                   [ ] |
   | ...                                                 ...   |
   -------------------------------------------------------------
   | A+  A-  📋 คัดลอกข้อความที่มาร์ก  << ตอนก่อนหน้า | ตอนถัดไป >> |
   ```

---

## | << บทก่อนหน้า | ชื่อเรื่อง - บทที่ 0001 | ตอนถัดไป >> |

\| \[1] ข้อความบรรทัดที่ 1                               \[ ] |
\| \[2] ข้อความบรรทัดที่ 2                               \[ ] |
\| \[3] ข้อความบรรทัดที่ 3                               \[ ] |
\| ...                                               ... |
----------------------------------------------------------

\| 📋 คัดลอกข้อความที่มาร์ก    A+    A-    🌙/☀️    << บทก่อนหน้า | ตอนถัดไป >> |

````
3. **รายละเอียดฟีเจอร์** **ปุ่มคัดลอกข้อความที่มาร์ก (📋)**
- วางไว้ใน Footer ของส่วนอ่านตอน (ขนาบกับปุ่ม prev/next และปรับฟอนต์)
- เมื่อกด:
  1. ดึง `marked_lines_json` ของผู้ใช้และ chapter
  2. ดึงข้อความแต่ละบรรทัดจากบท
  3. รวมเป็นสตริงเดียว แทรก newline ตามต้นฉบับ
  4. คัดลอกไป clipboard พร้อม popup ยืนยัน

---

## 💾 โครงสร้างฐานข้อมูล (Supabase)

### ตาราง `line_marks`
```sql
create table line_marks (
id uuid primary key default uuid_generate_v4(),
user_id uuid references auth.users(id),
novel_id uuid,
chapter_id uuid,
marked_lines_json jsonb,
updated_at timestamp default now()
);
````

### RLS Policy

```sql
create policy "Users can access only their line marks"
on line_marks for all
using (auth.uid() = user_id);
```

### ตาราง `novels` และ `chapters`

```sql
-- novels
create table novels (
  id uuid primary key default uuid_generate_v4(),
  title text,
  description text,
  cover_url text,
  access_roles text[] default array['reader'],
  editor_mode boolean default false
);

-- chapters
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

### ตาราง `auth.users` (เพิ่มฟิลด์)

```sql
alter table auth.users
add column role text default 'reader',
add column is_vip boolean default false,
add column vip_expires_at timestamp;
```

---

## ⚙️ Stack ที่ใช้

* **Frontend**: Next.js, React, Tailwind CSS
* **Auth & Database**: Supabase
* **Hosting**: Vercel
* **Realtime**: Supabase Realtime / client polling

---

## 🔐 ความปลอดภัย

* ต้องล็อกอินก่อนทุกหน้า
* ตรวจ role และ access\_roles ทั้งฝั่ง frontend & backend
* ใช้ RLS ป้องกันข้อมูลข้าม user

---

## 🛠 พัฒนาเพิ่มเติมในอนาคต

* ระบบ export/migrate line marks
* ระบบอนุมัติการแปลหลายขั้นตอน
* ระบบค้นหาบรรทัดที่มาร์กไว้
* Markdown preview สำหรับ editor

---

> © 2025 Kunpeng Novel. เขียนเพื่ออิสระของนักแปล คุณภาพเพื่อผู้อ่าน
