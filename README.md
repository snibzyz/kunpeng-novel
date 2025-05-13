INKREALM.CO – สเปคเว็บไซต์ฉบับเต็ม (UI ภาษาไทย, ฟอนต์ Sarabun, ธีมสีทอง-เทา, Responsive Design, Database & UX) V4.14
0. ปรัชญาการออกแบบโดยรวม (Overall Design Philosophy)
หรูหราและสง่างาม (Elegant & Luxurious): เน้นความหรูหรา สง่างาม ผ่านการใช้สีทองเป็นหลัก และสีเทาเพื่อเสริมความทันสมัย

ชัดเจนและอ่านง่าย (Clarity & Readability): ให้ความสำคัญสูงสุดกับการอ่านเนื้อหานิยายอย่างสบายตา ตัวอักษรต้องชัดเจน คอนทราสต์เหมาะสมบนพื้นหลังสีเทาและทอง โดยใช้ฟอนต์ Sarabun เพื่อความเป็นเอกภาพและอ่านง่ายในภาษาไทย

เครื่องมือสำหรับนักแปลโดยเฉพาะ (Translator-Focused Tools): มอบเครื่องมือที่ทรงพลังและใช้งานง่ายสำหรับนักแปลในการอ่าน ศึกษา และจัดเก็บบันทึกคำศัพท์หรือข้อความสำคัญจากเนื้อหาต้นฉบับ

ใช้งานง่ายและคำนึงถึงผู้ใช้เป็นศูนย์กลาง (Intuitive & User-Centric): ออกแบบโดยคำนึงถึงผู้ใช้งานทุกกลุ่ม (สมาชิก, นักแปล, ผู้ดูแลระบบ) มอบประสบการณ์ที่ใช้งานง่ายและตรงตามความต้องการ

ตอบสนองทุกอุปกรณ์และปรับเปลี่ยนแบบไดนามิก (Fully Responsive & Dynamic): ออกแบบให้เว็บไซต์ปรับเปลี่ยนการแสดงผลอัตโนมัติตามขนาดหน้าจอของทุกอุปกรณ์ (Desktop, Tablet, Mobile) ทั้งแนวตั้งและแนวนอน โดยเฉพาะหน้าอ่านนิยายที่จะต้องไม่มีการเลื่อนซ้าย-ขวา (No Horizontal Scroll)

ความเป็นเอกลักษณ์ของแบรนด์ที่สอดคล้องกัน (Consistent Branding): การใช้สีทอง-เทา, ฟอนต์ Sarabun, ไอคอน, และองค์ประกอบ UI ต่างๆ ต้องมีความสอดคล้องกันทั่วทั้งแพลตฟอร์ม

Tech Stack (ชุดเทคโนโลยีที่ใช้)
Frontend: Next.js + TailwindCSS + DaisyUI + TypeScript

DaisyUI: ใช้ร่วมกับ TailwindCSS เพื่อเป็น Component Library ช่วยให้สร้าง UI ที่สวยงาม ใช้งานง่าย และสอดคล้องกันได้รวดเร็วขึ้น โดยเฉพาะ Component พื้นฐาน เช่น ปุ่ม (Button), ฟอร์ม (Form), การ์ด (Card), Modal, Navbar, Dropdown, Avatar, Theme Controller (Toggle with icons), Collapse, Breadcrumbs, Progress Bar และอื่นๆ ที่ระบุในเอกสารนี้

Backend: Supabase (PostgreSQL + Auth + Storage)

Deployment: Vercel

CI/CD: GitHub Actions

Monitoring: Sentry + Grafana (หรือ Supabase Dashboard สำหรับการติดตามผลเบื้องต้น)

Form Handling & Validation: React Hook Form + Zod (สำหรับการตรวจสอบความถูกต้องของข้อมูลในฟอร์ม)

Rich Text Editor (สำหรับ Modal เพิ่ม/แก้ไขตอน): TipTap หรือ Editor.js (เลือกตัวที่เหมาะสมกับการใช้งานและปรับแต่งได้ง่าย)

1. โทนสีและสไตล์ (Color Palette & Style)
สีหลัก (Primary Color):

ทอง (Gold): Old Gold (#CFB53B), Satin Gold (#C4A647), Dark Goldenrod (#B8860B)

สีรอง (Secondary Color):

เทา (Grey): Light Grey (#F0F0F0, #E8E8E8), Medium Grey (#A9A9A9, #989898), Dark Grey (#4A4A4A, #333333)

สีเน้น (Accent Color): สีทองสว่าง, ขาว/ครีม, แดงอ่อน (#E57373) สำหรับแจ้งเตือน

สีกลาง (Neutral Colors): White/Off-White (#FFFFFF, #FAFAFA)

การพิมพ์ (Typography):

ฟอนต์หลัก (Font Family): Sarabun (สำหรับทั้งส่วนหัวเรื่องและเนื้อหาทั่วไป เพื่อความสอดคล้องและอ่านง่ายในภาษาไทย)

ส่วนหัวเรื่อง (Headings): Sarabun (ปรับน้ำหนักฟอนต์ เช่น Bold, Semi-Bold เพื่อความโดดเด่น)

เนื้อหาทั่วไป (Body Text): Sarabun (น้ำหนักปกติ Regular หรือ Medium)

ไอคอน (Icons): FontAwesome, Material Icons (Outline) - สีทองหรือเทาเข้ม

ไอคอนสถานะการอ่าน: ไอคอนรูปตาแบบมี Animation (เช่น ตาที่กะพริบเบาๆ หรือมีการเคลื่อนไหวเล็กน้อยเมื่อถูกมองเห็น)

ไอคอนสถานะตอน:

เผยแพร่: ไอคอนรูปลูกโลก (Globe icon)

ฉบับร่าง: ไอคอนรูปเอกสาร (Document icon)

ไอคอนปรับแต่งการแสดงผล (Display Customization Icon): ไอคอน "Aa" หรือไอคอนรูปเฟืองการตั้งค่าการแสดงผล

ไอคอนบุ๊คมาร์ค (Bookmark Icon): ไอคอนรูปที่คั่นหนังสือ

ปุ่มสลับธีม (Theme Toggle - สว่าง/มืด):

เว็บไซต์รองรับการสลับธีมระหว่างสว่าง (Light Mode) และมืด (Dark Mode) โดยใช้ DaisyUI Theme Controller (แบบ Toggle พร้อมไอคอนพระอาทิตย์/พระจันทร์)

การตั้งค่าธีมจะถูกจดจำสำหรับผู้ใช้แต่ละคน (เช่น ใช้ localStorage)

ผู้ใช้สามารถสลับธีมได้จากแถบนำทาง (Navbar) และ ภายในหน้าอ่านนิยาย

Breadcrumbs Styling: ใช้ DaisyUI Breadcrumbs component จัดสไตล์ให้เข้ากับธีมสีทอง-เทา มีขนาดตัวอักษรที่เหมาะสมและไม่รบกวนเนื้อหาหลัก

2. โครงสร้าง Layout หลัก (Main Layout Structure)
แถบนำทาง (Navbar - Sticky Top): ตอบสนองทุกอุปกรณ์, มีปุ่มสลับธีม (ใช้ DaisyUI Navbar component)

(ใหม่) ส่วน Breadcrumbs: แสดงอยู่ใต้ Navbar หรือบริเวณบนสุดของพื้นที่เนื้อหาหลักในแต่ละหน้า (ยกเว้นหน้าแรกสุด) เพื่อบอกตำแหน่งปัจจุบันของผู้ใช้

พื้นที่เนื้อหาหลัก (Main Content Area): พื้นหลังเทาอ่อน (#F0F0F0 ในโหมดสว่าง), ปรับความกว้างสูงสุด (Max-width)

ส่วนท้ายเว็บ (Footer): ดีไซน์เรียบหรู, ตอบสนองทุกอุปกรณ์

3. บทบาทผู้ใช้ (User Roles & Permissions)
สมาชิก (Member):

อ่านเนื้อหาสาธารณะและตอนที่เผยแพร่แบบปกติ

ไม่มีบรรทัดมาร์ค, ไม่มีสิทธิ์สร้างนิยาย หรือใช้โหมดแปล

สามารถคั่นหน้า (Bookmark) ตอนที่ชอบได้

แถบเครื่องมือสำหรับอ่าน (Reading Toolbar - จัดเรียงจากซ้ายไปขวา หรือตามความเหมาะสมของ UI):

ไอคอนสารบัญ (List/Menu Icon): คลิกเพื่อเปิด Modal หรือ Dropdown ใหญ่ แสดงชื่อตอนปัจจุบันและรายการตอนอื่นๆ

ไอคอนปรับแต่งการแสดงผล (Display Customization Icon - "Aa"): คลิกเพื่อเปิด Popover/Modal (ใช้ DaisyUI Modal) ที่มีตัวเลือก:

ขนาดตัวอักษร: แถบเลื่อน (Slider) หรือปุ่ม - / + (ใช้ DaisyUI Range หรือ Button)

ระยะห่างระหว่างบรรทัด: แถบเลื่อน (Slider) หรือปุ่ม - / +

สีพื้นหลัง: ตัวเลือกสี (เช่น พื้นขาว-อักษรดำ, พื้นเหลืองนวล/ซีเปีย-อักษรดำ, พื้นดำ-อักษรขาว) (ใช้ DaisyUI Button Group)

(ถ้ามี) รูปแบบตัวอักษร: ตัวเลือกฟอนต์ (เช่น แบบที่ 1, แบบที่ 2)

ไอคอนบุ๊คมาร์ค (Bookmark Icon): สำหรับคั่นหน้า/ยกเลิกการคั่นหน้าตอนปัจจุบัน

ปุ่มสลับธีมเว็บ (Website Theme Toggle): สลับธีมสว่าง/มืดของ UI เว็บไซต์โดยรวม (ใช้ DaisyUI Swap component หรือ Custom Toggle)

ปุ่มนำทาง < (ตอนก่อนหน้า)

ปุ่มนำทาง > (ตอนถัดไป)

นักแปล (Translator):

วัตถุประสงค์หลัก: อ่านนิยายภาษาต้นฉบับ (เช่น ภาษาจีน zh ที่เลือกไว้ตอนสร้างนิยาย) บนแพลตฟอร์ม เพื่อความสะดวกในการอ้างอิงและทำความเข้าใจเนื้อเรื่อง สามารถใช้ฟังก์ชัน "บรรทัดมาร์ค" เพื่อบันทึกคำศัพท์หรือประโยคที่น่าสนใจไว้สำหรับการแปลหรือการศึกษาเพิ่มเติม แพลตฟอร์มจะแสดงเนื้อหาต้นฉบับเสมอในโหมดแปล ทำให้นักแปลสามารถใช้เครื่องมือแปลภาษาของเบราว์เซอร์ (เช่น Google Translate) ควบคู่ไปได้หากต้องการทำความเข้าใจเบื้องต้น

เข้าถึง "โหมดแปล (Translator Mode)"

ใช้งานบรรทัดมาร์ค: คลิกเพื่อมาร์คบรรทัด, ดับเบิลคลิกเพื่อคัดลอกข้อความต้นฉบับของบรรทัดนั้นพร้อมไฮไลท์

สร้างนิยาย (ผ่าน Modal สร้างนิยาย), แก้ไขตอน (ผ่าน Modal แก้ไข/เพิ่มตอน)

แถบเครื่องมือพิเศษในโหมดแปล (Translator Mode Toolbar - จัดเรียงจากซ้ายไปขวา หรือตามความเหมาะสมของ UI):

ไอคอนสารบัญ (List/Menu Icon): คลิกเพื่อเปิด Modal หรือ Dropdown ใหญ่ แสดงชื่อตอนปัจจุบันและรายการตอนอื่นๆ (แสดงจำนวนมาร์คของแต่ละตอนด้วย)

ไอคอนปรับแต่งการแสดงผล (Display Customization Icon - "Aa"): (เหมือนกับของ Member)

ไอคอนบุ๊คมาร์ค (Bookmark Icon): สำหรับคั่นหน้า/ยกเลิกการคั่นหน้าตอนปัจจุบัน

เครื่องมือสำหรับนักแปล (Translator Tools - อาจรวมอยู่ในไอคอนเดียวแล้วเปิด Sub-menu หรือแสดงเป็นไอคอนแยก):

ไอคอน "คัดลอกมาร์คทั้งหมด" (Copy All Marks): สำหรับคัดลอกทุกบรรทัดที่มาร์คไว้ในตอนปัจจุบัน

ไอคอน "ล้างมาร์คทั้งหมด" (Clear Marks): สำหรับล้างมาร์คทั้งหมดในตอนปัจจุบัน (พร้อมยืนยันผ่าน DaisyUI Modal)

ปุ่มสลับธีมเว็บ (Website Theme Toggle): สลับธีมสว่าง/มืดของ UI เว็บไซต์โดยรวม

ปุ่มนำทาง < (ตอนก่อนหน้า)

ปุ่มนำทาง > (ตอนถัดไป)

สิทธิ์ทั้งหมดของสมาชิก

ผู้ดูแลระบบ (Admin):

มีสิทธิ์จัดการทุกอย่าง, รวมถึงการเลื่อนขั้น/ลดขั้น/ระงับ/ลบผู้ใช้

ดูบรรทัดมาร์คของนักแปลคนอื่น

ย้อนเวอร์ชันบท/นิยาย

สิทธิ์ทั้งหมดของนักแปล

การจัดการสิทธิ์: ใช้ Row-Level Security (RLS) ใน Supabase ตามบทบาท (Role)

4. หน้าเข้าสู่ระบบ / ลงทะเบียน / ลืมรหัสผ่าน
Stack: Supabase Auth, React Hook Form, Zod, DaisyUI (สำหรับ UI components เช่น Input, Button, Card)

(รายละเอียด UI เหมือนเดิม: ธีมทอง-เทา, ฟอนต์ Sarabun, ตอบสนองทุกอุปกรณ์, ข้อความ UI ทั้งหมดเป็นภาษาไทย)

5. โครงสร้างแถบนำทาง (Navbar - Dynamic ตาม Role & Responsive)
Stack: DaisyUI (Navbar, Dropdown, Avatar), TailwindCSS, Custom logic สำหรับ ThemeToggle

ดีไซน์: พื้นหลังเทาเข้ม/ทองเข้ม, ตัวอักษร/ไอคอนสีทองอ่อน/ขาว (ฟอนต์ Sarabun)

การจัดเรียงเมนู (จากซ้ายไปขวาสำหรับ Desktop):

[โลโก้ INKREALM]: คลิกเพื่อไปหน้าแรกเสมอ

[หน้าแรก]: ลิงก์หลักไปยังหน้าแสดงนิยายทั้งหมด

[บุ๊คมาร์ค]: สำหรับผู้ใช้ที่เข้าสู่ระบบแล้วทุกคน

[+ สร้างนิยาย]: สำหรับนักแปล, สำหรับผู้ดูแลระบบ (ปุ่มที่ชัดเจน คลิกเพื่อเปิด Modal สร้างนิยาย (ดูข้อ 6.7))

[แดชบอร์ดนักแปล]: สำหรับนักแปล, สำหรับผู้ดูแลระบบ (ลิงก์ไปยังแดชบอร์ดของนักแปล)

[แดชบอร์ดผู้ดูแลระบบ]: สำหรับผู้ดูแลระบบเท่านั้น (ลิงก์ไปยังแดชบอร์ดของผู้ดูแลระบบ)

[สลับธีม]: ไอคอนพระจันทร์/พระอาทิตย์ สำหรับสลับธีมสว่าง/มืดของ UI เว็บไซต์โดยรวม (ใช้ DaisyUI Theme Controller)

[โปรไฟล์ (Avatar Dropdown)]: แสดงรูปโปรไฟล์และชื่อผู้ใช้ (ใช้ DaisyUI Avatar และ Dropdown)

ตัวเลือกใน Dropdown:

[ตั้งค่าโปรไฟล์]

[เปลี่ยนรหัสผ่าน]

[ออกจากระบบ]

การแสดงผลตามบทบาท:

สมาชิก: [โลโก้], [หน้าแรก], [บุ๊คมาร์ค], [สลับธีม], [โปรไฟล์]

นักแปล: [โลโก้], [หน้าแรก], [บุ๊คมาร์ค], [+ สร้างนิยาย], [แดชบอร์ดนักแปล], [สลับธีม], [โปรไฟล์]

ผู้ดูแลระบบ: [โลโก้], [หน้าแรก], [บุ๊คมาร์ค], [+ สร้างนิยาย], [แดชบอร์ดนักแปล], [แดชบอร์ดผู้ดูแลระบบ], [สลับธีม], [โปรไฟล์]

การตอบสนองบนมือถือ (Mobile Responsive):

แถบนำทางจะแสดง [โลโก้ INKREALM] ทางด้านซ้าย และไอคอน Hamburger Menu (สีทอง) ทางด้านขวา

เมื่อคลิก Hamburger Menu: เมนูจะเลื่อนออกมาจากด้านข้าง (พื้นหลังสีเทาเข้ม รายการเมนูสีทอง/ขาว) โดยรายการเมนูจะเรียงตามลำดับความสำคัญและสิทธิ์ของผู้ใช้คล้ายกับบน Desktop แต่เป็นแนวตั้ง (ใช้ DaisyUI Drawer หรือ Dropdown สำหรับ Mobile Menu)

6. รายละเอียดหน้าเว็บไซต์หลัก (Responsive Design)
6.1 หน้าแรก (Home Page)

Stack: DaisyUI (Card, Grid, Pagination), Supabase (Query novels)

Breadcrumbs: ไม่จำเป็นต้องแสดงบนหน้าแรกสุด

หัวเรื่อง: "นิยายทั้งหมด"

Layout: ส่วน Hero (Hero Section - ถ้ามี), Grid Layout แสดงนิยาย (ตอบสนองทุกอุปกรณ์: Desktop 3-4 คอลัมน์, Tablet 2-3, Mobile 1-2)

การแบ่งหน้า (Pagination): มีระบบแบ่งหน้าสำหรับแสดงรายการนิยาย (ใช้ DaisyUI Pagination component)

การ์ดนิยาย (Novel Card - ใช้ DaisyUI Card):

รูปปกนิยาย (Cover Image): อัตราส่วน 3:4, คลิกปกไปยังหน้า "ข้อมูลนิยายและสารบัญ (Novel Detail Page)" ของนิยายเรื่องนั้น

ชื่อเรื่อง (Title): ใต้รูปปก, สีเทาเข้ม (ฟอนต์ Sarabun)

จำนวนตอน (Chapter Count): ใต้ชื่อเรื่อง, สีเทากลาง (ฟอนต์ Sarabun) (นับเฉพาะตอนที่มีสถานะ status = 'published')

ตัวเลือกการกรอง/จัดเรียง (Filter/Sort Options): กรองตามหมวดหมู่, เรียงตามอัปเดตล่าสุด (ใช้ DaisyUI Dropdown หรือ Select)

ปุ่ม "[+ สร้างนิยาย]" (สำหรับนักแปล/ผู้ดูแลระบบ): อาจไม่จำเป็นต้องมีบนหน้าแรกอีก หากมีในแถบนำทางที่ชัดเจนแล้ว หรืออาจคงไว้เป็นปุ่มลอย (FAB - DaisyUI Button) เพื่อความสะดวก

6.2 หน้าข้อมูลนิยายและสารบัญ (Novel Detail Page)

Stack: DaisyUI (Card, Button, Collapse, Modal, Tabs - ถ้ามี, Breadcrumbs), Supabase

Path: /[novel_slug] หรือ /[novel_id]

Breadcrumbs: แสดง เช่น หน้าแรก > [ชื่อหมวดหมู่ ถ้ามี] > [ชื่อนิยาย]

Layout (ตอบสนองทุกอุปกรณ์):

ส่วนข้อมูลนิยาย (Novel Information Section):

แสดงด้านบนหรือด้านซ้าย (Desktop) หรือด้านบน (Mobile)

รูปปกนิยาย (Cover Image): ขนาดใหญ่พอสมควร (เช่น อัตราส่วน 3:4)

ชื่อเรื่อง (Title): ตัวใหญ่, สีเทาเข้ม (ฟอนต์ Sarabun)

ผู้แต่ง/ผู้สร้าง (Author/Creator): (จาก novels.author_id JOIN profiles.display_name) (ฟอนต์ Sarabun)

หมวดหมู่ (Category): (จาก novels.category_id JOIN categories.name) แสดงเป็น Tag สีทองอ่อน (ใช้ DaisyUI Badge) (ฟอนต์ Sarabun)

ภาษาต้นฉบับ (Original Language): แสดงชื่อเต็มของภาษา (เช่น จีน, อังกฤษ) (ฟอนต์ Sarabun) (ถ้ามีนะครับ)

คำโปรย (Synopsis): แสดงเต็ม หรือย่อพร้อมปุ่ม "อ่านเพิ่มเติม" (ฟอนต์ Sarabun)

จำนวนตอนทั้งหมด (Total Chapters): (จาก novels.total_chapters หรือ novels.published_chapters ขึ้นอยู่กับบทบาท) (ฟอนต์ Sarabun)

สถานะนิยาย (Novel Status): "ต่อเนื่อง", "จบแล้ว" (ฟอนต์ Sarabun)

ปุ่ม "อ่านตอนแรก" / "อ่านต่อ" (ถ้ามีประวัติการอ่าน): ปุ่มสีทองหลัก (DaisyUI Button) นำไปยังตอนที่เหมาะสมใน "โหมดอ่าน" หรือ "โหมดแปล" ตามบทบาท

(สำหรับผู้สร้างนิยาย/ผู้ดูแลระบบ) ปุ่มจัดการนิยาย:

ปุ่ม "แก้ไขข้อมูลนิยาย": พาไปยังหน้าแก้ไขข้อมูลนิยาย (คล้ายหน้าสร้างนิยายแต่เป็นการแก้ไข)

ปุ่ม "ลบนิยาย": พร้อม Modal ยืนยันการลบ (สีแดง หรือเน้นการเตือน) (ใช้ DaisyUI Modal)

ส่วนปุ่มเพิ่มตอน (Add Chapter Buttons - สำหรับผู้สร้างนิยาย/ผู้ดูแลระบบ):

แสดงอยู่ใต้ข้อมูลนิยาย หรือในตำแหน่งที่เหมาะสม (อาจรวมเป็น DaisyUI Button Group)

ปุ่ม "เพิ่มตอนด้วย .txt": คลิกเพื่อเปิด Modal เพิ่มตอนด้วย .txt (ดูข้อ 6.8.1)

ปุ่ม "เพิ่มตอน": คลิกเพื่อเปิด Modal เพิ่ม/แก้ไขตอน (ดูข้อ 6.8.2)

ส่วนสารบัญ (Table of Contents Section):

รูปแบบ: ใช้ DaisyUI Collapse component แสดงอยู่บริเวณด้านล่างของส่วนข้อมูลนิยาย ผู้ใช้คลิกที่หัวข้อ "สารบัญ" (หรือชื่อนิยาย) เพื่อเปิด/ปิดรายการตอน

การแสดงผลภายใน Collapse Content:

รายการตอนจะเรียงลงมาเป็นแถวแนวตั้ง

แต่ละแถว (รายการตอน):

ชื่อตอน (Chapter Title): (ฟอนต์ Sarabun)

สถานะการอ่าน: หากผู้ใช้อ่านตอนนี้แล้ว แสดง ไอคอนรูปตาแบบมี Animation (เช่น ตาที่กะพริบเบาๆ หรือมีการเคลื่อนไหวเล็กน้อยเมื่อถูกมองเห็น) และพื้นหลังของแถวอาจเปลี่ยนเป็นสีเทาอ่อนเพื่อแยกความแตกต่าง (ยืนยันสถานะ "อ่านแล้ว" และการเปลี่ยนสีพื้นหลัง)

(สำหรับนักแปล/ผู้ดูแลระบบ) จำนวนเส้นมาร์ค (Mark Lines): แสดงจำนวนมาร์คที่นักแปลคนปัจจุบันทำไว้ในตอนนั้นๆ เช่น " (X มาร์ค)"

(สำหรับผู้สร้างนิยาย/ผู้ดูแลระบบ) ปุ่มจัดการตอน:

ปุ่ม "แก้ไขตอน" (ไอคอนรูปดินสอ): คลิกเพื่อเปิด Modal เพิ่ม/แก้ไขตอน (ดูข้อ 6.8.2) โดยโหลดข้อมูลของตอนนี้มาแสดง

ปุ่ม "ลบตอน" (ไอคอนรูปถังขยะ): พร้อม Modal ยืนยันการลบ (ใช้ DaisyUI Modal)

(สำหรับนักแปล/ผู้ดูแลระบบ) ปุ่ม "คัดลอกมาร์คทั้งหมดของตอนนี้" (ไอคอนรูป copy): คลิกเพื่อคัดลอกบรรทัดมาร์คทั้งหมดของตอนนี้โดยไม่ต้องเปิดอ่าน (ดึงข้อมูลจาก mark_lines และ chapters.content_original)

สถานะตอน (Chapter Status): แสดงด้วยไอคอน:

เผยแพร่: ไอคอนรูปลูกโลก (Globe icon)

ฉบับร่าง: ไอคอนรูปเอกสาร (Document icon)
(แสดงเฉพาะผู้สร้าง/ผู้ดูแลระบบ หรือตามความเหมาะสม)

การแบ่งหน้าสำหรับรายการตอน (Pagination): หากมีจำนวนตอนมากภายใน Collapse อาจมีการแบ่งหน้าย่อยๆ หรือใช้การ Scroll ภายใน Content ของ Collapse

การเข้าถึง:

สมาชิก: เห็นเฉพาะตอนที่มีสถานะ "เผยแพร่" และระดับการเข้าถึงอนุญาต

นักแปล/ผู้ดูแลระบบ: เห็นทุกตอน

6.3 หน้าบุ๊คมาร์ค (Bookmarks Page)

Stack: Supabase (จัดการ relation user_bookmarks), DaisyUI (Table หรือ Card List, Button, Modal สำหรับยืนยัน, Breadcrumbs)

Breadcrumbs: แสดง เช่น หน้าแรก > บุ๊คมาร์ค

(รายละเอียด UI เหมือนเดิม: ธีมทอง-เทา, ฟอนต์ Sarabun, ตอบสนองทุกอุปกรณ์, UI ภาษาไทย)

ปุ่ม "อ่าน" พาไปยังตอนที่ Bookmark ในโหมดที่เหมาะสมกับบทบาท

6.4 แดชบอร์ดนักแปล (Translator Dashboard)

Stack: Supabase, DaisyUI (Breadcrumbs), Custom JavaScript สำหรับ Line Numbers, บรรทัดมาร์ค และ Copy Mark functionality

Breadcrumbs: แสดง เช่น หน้าแรก > แดชบอร์ดนักแปล

Layout (ตอบสนองทุกอุปกรณ์):

แสดงรายการนิยายที่นักแปลมีสิทธิ์จัดการ (เช่น นิยายที่ตนเองสร้าง หรือได้รับมอบหมายให้แปล) (ฟอนต์ Sarabun)

อาจมีสถิติส่วนตัว (จำนวนตอนที่ Mark, นิยายที่กำลังทำ) (ฟอนต์ Sarabun)

การเข้าสู่โหมดแปล:

จากหน้านี้ เมื่อคลิกเลือกนิยายและตอน จะเข้าสู่ "โหมดแปล" โดยตรง

หรือจาก "หน้าข้อมูลนิยายและสารบัญ" หากบทบาทเป็นนักแปลและคลิกอ่านตอน

6.5 หน้าอ่านนิยายและโหมดแปล (Reading & Translator Mode UI)

Stack: DaisyUI (Toolbar, Modal, Button, Range/Slider, Progress, Breadcrumbs), Custom JavaScript

Path (สำหรับโหมดแปล): /translate/[novel_slug]/[chapter_slug]

Path (สำหรับโหมดอ่านของสมาชิก): /[novel_slug]/[chapter_slug]

Breadcrumbs: แสดง เช่น หน้าแรก > [ชื่อนิยาย] > [ชื่อตอน] (ลิงก์ไปยังหน้าข้อมูลนิยายและสารบัญ และหน้าแรก)

UI Layout (Single Pane View เน้นเนื้อหา):

การเลื่อนหน้า: เนื้อหาจะถูกจำกัดความกว้างเพื่อให้อ่านง่าย และจะมีการเลื่อนในแนวตั้งเท่านั้น (ไม่มี Scrollbar แนวนอน)

แถบความคืบหน้าการอ่าน (Reading Progress Bar):

แสดงที่ด้านบนหรือด้านล่างของหน้าจอ (อาจเป็นแบบบางๆ ไม่รบกวนสายตา)

แสดงเป็นแถบสี (เช่น สีทอง) ที่เพิ่มขึ้นตามตำแหน่งการเลื่อนของผู้ใช้ในเนื้อหาปัจจุบันของตอนนั้นๆ (คำนวณจาก Scroll Position / Total Scrollable Height)

(ใช้ DaisyUI Progress component หรือ Custom Styling)

พื้นที่เนื้อหา (Content Area):

แสดงเนื้อหานิยาย (chapters.content_original สำหรับโหมดแปล, หรือเนื้อหาที่ผ่านการประมวลผลแล้วสำหรับสมาชิก) (ฟอนต์ Sarabun หรือฟอนต์ของภาษาต้นฉบับหากมีการตั้งค่าแยก)

(สำหรับโหมดแปล) หมายเลขบรรทัด (Line Numbers): แสดงหมายเลขบรรทัดด้านข้างเนื้อหาต้นฉบับอย่างชัดเจน (สีเทากลาง)

(สำหรับโหมดแปล) การโต้ตอบกับเส้นมาร์ค (MarkLine Interaction):

คลิก (Click): ที่บรรทัดใดๆ เพื่อ "มาร์ค" บรรทัดนั้น (เช่น เปลี่ยนสีพื้นหลังบรรทัดเป็นสีทองอ่อนๆ หรือมีสัญลักษณ์ติ๊กถูกสีทองด้านข้างหมายเลขบรรทัด) การมาร์คจะถูกบันทึกสำหรับนักแปลคนนั้นในตอนนั้น (เพิ่ม/ลบหมายเลขบรรทัดออกจาก Array ใน mark_lines.marked_lines_data)

ดับเบิลคลิก (Double-click): ที่บรรทัดใดๆ จะเป็นการคัดลอกข้อความ ต้นฉบับ ของบรรทัดนั้นไปยัง Clipboard และทำการไฮไลท์บรรทัดนั้นด้วยสีทองโปร่งแสงชั่วคราว

แถบเครื่องมือลอย (Floating Toolbar - แสดงเมื่อมีการโต้ตอบหรืออยู่บริเวณขอบจอ เช่น ด้านบนหรือด้านล่าง - ใช้ DaisyUI Button Group หรือ Custom Styling):

การจัดเรียง (จากซ้ายไปขวา หรือจัดกลุ่มตามความเหมาะสม):

ไอคอนสารบัญ (List/Menu Icon): คลิกเพื่อเปิด Modal หรือ Dropdown ใหญ่ แสดงชื่อตอนปัจจุบันและรายการตอนอื่นๆ (คล้าย Collapse ในหน้าข้อมูลนิยาย)

ไอคอนปรับแต่งการแสดงผล "Aa" (Display Customization Icon): คลิกเพื่อเปิด Popover/Modal (ใช้ DaisyUI Modal หรือ Popover) ที่มีตัวเลือก:

ขนาดตัวอักษร: แถบเลื่อน (DaisyUI Range) หรือปุ่ม - / +

ระยะห่างระหว่างบรรทัด: แถบเลื่อน (DaisyUI Range) หรือปุ่ม - / +

สีพื้นหลัง: ตัวเลือกสี (เช่น พื้นขาว-อักษรดำ, พื้นเหลืองนวล/ซีเปีย-อักษรดำ, พื้นดำ-อักษรขาว) (ใช้ DaisyUI Button Group)

(ถ้ามี) รูปแบบตัวอักษร: ตัวเลือกฟอนต์ (เช่น แบบที่ 1, แบบที่ 2)

ไอคอนบุ๊คมาร์ค (Bookmark Icon): สำหรับคั่นหน้า/ยกเลิกการคั่นหน้าตอนปัจจุบัน

(สำหรับโหมดแปลเท่านั้น) เครื่องมือสำหรับนักแปล (Translator Tools - อาจรวมอยู่ในไอคอนเดียวแล้วเปิด Sub-menu หรือแสดงเป็นไอคอนแยก):

ไอคอน "คัดลอกมาร์คทั้งหมด" (Copy All Marks): สำหรับคัดลอกทุกบรรทัดที่มาร์คไว้ในตอนปัจจุบัน

ไอคอน "ล้างมาร์คทั้งหมด" (Clear Marks): สำหรับล้างมาร์คทั้งหมดในตอนปัจจุบัน (พร้อมยืนยันผ่าน DaisyUI Modal)

ปุ่มสลับธีมเว็บ (Website Theme Toggle): ไอคอนพระจันทร์/พระอาทิตย์ สำหรับสลับธีมสว่าง/มืดของ UI เว็บไซต์โดยรวม (ใช้ DaisyUI Swap)

ปุ่มนำทาง < (ตอนก่อนหน้า)

ปุ่มนำทาง > (ตอนถัดไป)

การตอบสนองบนมือถือ (Mobile Responsive): เนื้อหา, หมายเลขบรรทัด, และแถบเครื่องมือต้องใช้งานได้สะดวกบนทุกอุปกรณ์ แถบเครื่องมืออาจอยู่ด้านล่างจอเพื่อความสะดวกในการใช้งานมือเดียว และปุ่มต่างๆ อาจมีขนาดใหญ่ขึ้น

6.6 แดชบอร์ดผู้ดูแลระบบ (Admin Dashboard)

Stack: DaisyUI (Table, Modal, Button, Breadcrumbs), Supabase (Admin API, Policies, Functions สำหรับจัดการ user roles และ versioning)

Breadcrumbs: แสดง เช่น หน้าแรก > แดชบอร์ดผู้ดูแลระบบ > [ส่วนที่กำลังดู เช่น จัดการผู้ใช้]

(รายละเอียด UI เหมือนเดิม: ธีมทอง-เทา, ฟอนต์ Sarabun, ตอบสนองทุกอุปกรณ์, จัดการผู้ใช้, UI ภาษาไทย)

การดูและจัดการเส้นมาร์คของนักแปล:

ผู้ดูแลระบบสามารถดูบรรทัดมาร์คที่นักแปลคนอื่นทำไว้ได้ (ตามที่ระบุในข้อ 7.1)

ผู้ดูแลระบบสามารถแก้ไข mark_lines.marked_lines_data (ที่เป็น Array ของหมายเลขบรรทัด) โดยตรงผ่าน Supabase Studio หรือ Interface ที่สร้างขึ้นสำหรับผู้ดูแลระบบ (เช่น สำหรับการนำเข้าข้อมูลจากระบบเก่า หรือแก้ไขข้อผิดพลาด)

6.7 Modal สร้างนิยาย (Create Novel Modal) - สำหรับนักแปล/ผู้ดูแลระบบ

การเรียกใช้งาน: เปิดขึ้นเมื่อคลิกปุ่ม "[+ สร้างนิยาย]" ใน Navbar

Stack: React Hook Form, Zod, DaisyUI (Modal, Input, Textarea, Dropdown, FileInput, Checkbox, Radio), Supabase (Storage for cover, Database for novel data)

UI ภายใน Modal (ใช้ DaisyUI Modal):

หัวเรื่อง Modal: "สร้างนิยายใหม่"

ฟอร์มกรอกข้อมูลนิยาย (จัดเรียงเป็นส่วนๆ ภายใน Modal):

ชื่อเรื่อง (Title): (Input text, required)

คำโปรย (Synopsis): (Textarea)

หมวดหมู่ (Category): (Dropdown - ใช้ DaisyUI Select)

ปกนิยาย (Cover Image): (อัปโหลดไฟล์ - ใช้ DaisyUI FileInput, รองรับ jpg, png, webp, แสดง Preview)

ภาษาต้นฉบับ (Original Language): (Dropdown - ใช้ DaisyUI Select, required, ตัวเลือก: จีน (zh), อังกฤษ (en), ไทย (th))

สิทธิ์ในการมองเห็นนิยายนี้ (Novel Visibility): (Radio buttons - ใช้ DaisyUI Radio)

ทุกคน (All Members)

เฉพาะนักแปล (Translators Only)

ค่าเริ่มต้นสำหรับการสร้างตอน (Default Chapter Settings):

สถานะเริ่มต้นของตอน (Default Chapter Status): (Radio buttons - ใช้ DaisyUI Radio)

เผยแพร่ (Published)

ฉบับร่าง (Draft)

สิทธิ์การเข้าถึงตอนเริ่มต้น (Default Chapter Access): (Checkbox - ใช้ DaisyUI Checkbox) (แสดงตามสิทธิ์ของนิยาย)

สมาชิก (Member)

นักแปล (Translator)

ปุ่มดำเนินการใน Modal:

ปุ่ม "สร้างนิยาย" (Create Novel): (DaisyUI Button - สีทองหลัก)

ปุ่ม "ยกเลิก" (Cancel): (DaisyUI Button - สีเทา หรือ Outline) หรือปุ่มปิด Modal (X)

การตอบสนองบนมือถือ (Mobile Responsive): Modal ควรปรับขนาดและแสดงผลได้อย่างเหมาะสมบนหน้าจอขนาดเล็ก เนื้อหาภายใน Modal อาจมีการ Scroll หากยาวเกินไป

6.8 การจัดการตอน (Chapter Management - ผ่าน Modals)

6.8.1 Modal เพิ่มตอนด้วย .txt (Batch Upload Modal)

การเรียกใช้งาน: เปิดขึ้นเมื่อคลิกปุ่ม "เพิ่มตอนด้วย .txt" ในหน้าข้อมูลนิยายและสารบัญ (ข้อ 6.2)

Stack: DaisyUI (Modal, FileInput, Progress), Supabase (Storage for .txt, Database for chapter data)

UI ภายใน Modal (ใช้ DaisyUI Modal):

หัวเรื่อง Modal: "เพิ่มตอนด้วยไฟล์ .txt"

ส่วนอัปโหลดไฟล์:

พื้นที่สำหรับลากและวางไฟล์ (Drag & Drop Area) หรือปุ่ม "เลือกไฟล์" (ใช้ DaisyUI FileInput)

รองรับการเลือกหลายไฟล์พร้อมกัน (สูงสุด 500 ไฟล์ต่อครั้ง)

แสดงรายการไฟล์ที่เลือกพร้อมสถานะ (เช่น รออัปโหลด, กำลังอัปโหลด, อัปโหลดสำเร็จ, มีข้อผิดพลาด)

การจัดการไฟล์ซ้ำ:

ระบบจะตรวจสอบชื่อไฟล์ (ซึ่งจะใช้เป็นชื่อตอน) หากพบว่ามีชื่อตอนซ้ำกับที่มีอยู่แล้วในนิยายเรื่องนี้

สำหรับแต่ละไฟล์ที่ซ้ำ จะมีตัวเลือกให้ผู้ใช้:

"ข้าม (Skip)": ไม่นำเข้าตอนนี้

"เขียนทับ (Overwrite)": ลบตอนเก่าที่มีชื่อเดียวกันแล้วนำเข้าตอนใหม่นี้แทน

ความคืบหน้า (Progress):

แสดงแถบความคืบหน้าโดยรวม (Overall Progress Bar - ใช้ DaisyUI Progress) สำหรับการอัปโหลดและประมวลผลไฟล์ทั้งหมด

อาจแสดงความคืบหน้าของแต่ละไฟล์ด้วย

ข้อความสรุป: เมื่อดำเนินการเสร็จสิ้น แสดงจำนวนตอนที่เพิ่มสำเร็จ, จำนวนที่ข้าม, จำนวนที่เขียนทับ

ปุ่มดำเนินการ: "เริ่มอัปโหลด", "ยกเลิก", "ปิด"

6.8.2 Modal เพิ่ม/แก้ไขตอน (Create/Edit Chapter Modal)

การเรียกใช้งาน:

เพิ่มตอน: เปิดขึ้นเมื่อคลิกปุ่ม "เพิ่มตอน" ในหน้าข้อมูลนิยายและสารบัญ (ข้อ 6.2)

แก้ไขตอน: เปิดขึ้นเมื่อคลิกปุ่ม "แก้ไขตอน" (ไอคอนดินสอ) ในส่วนสารบัญ (ข้อ 6.2) โดยโหลดข้อมูลของตอนนั้นมาแสดง

Stack: TipTap หรือ Editor.js, DaisyUI (Modal, Input, Radio, Checkbox, Button), Supabase

UI ภายใน Modal (ใช้ DaisyUI Modal):

หัวเรื่อง Modal: "เพิ่มตอนใหม่" หรือ "แก้ไขตอน: [ชื่อตอนปัจจุบัน]"

ฟอร์มกรอกข้อมูลตอน:

ชื่อตอน (Chapter Title): (Input text, required)

เนื้อหา (Content): (Rich Text Editor - TipTap หรือ Editor.js)

สถานะ (Status): (Radio buttons - ใช้ DaisyUI Radio)

เผยแพร่ (Published) (พร้อมไอคอนรูปลูกโลก )

ฉบับร่าง (Draft) (พร้อมไอคอนรูปเอกสาร )

สิทธิ์การเข้าถึง (Access Level): (Checkbox group - ใช้ DaisyUI Checkbox)

สมาชิก (Member)

นักแปล (Translator)

(ผู้ดูแลระบบเห็นทุกอย่างโดยอัตโนมัติ ไม่จำเป็นต้องมีตัวเลือกนี้)

ปุ่มดำเนินการใน Modal:

(สำหรับเพิ่มตอนใหม่):

ปุ่ม "เผยแพร่ตอน" (Publish Chapter): (DaisyUI Button - สีทองหลัก)

ปุ่ม "บันทึกเป็นฉบับร่าง" (Save as Draft): (DaisyUI Button - สีทองรอง)

(สำหรับแก้ไขตอน):

ปุ่ม "บันทึกการเปลี่ยนแปลง" (Save Changes): (DaisyUI Button - สีทองหลัก)

ปุ่ม "ลบตอน" (Delete Chapter): (DaisyUI Button - สีแดง) พร้อม Modal ยืนยันการลบ

ปุ่ม "ยกเลิก" (Cancel): (DaisyUI Button - สีเทา หรือ Outline) หรือปุ่มปิด Modal (X)

6.9 หน้าโปรไฟล์ (Profile Page)

Stack: DaisyUI (Modal หรือ Tabs, Avatar, Form, Input, Button, Breadcrumbs), Supabase (Auth for password change, Storage for avatar, Database for display name)

Breadcrumbs: แสดง เช่น หน้าแรก > โปรไฟล์

(รายละเอียด UI เหมือนเดิม: ธีมทอง-เทา, ฟอนต์ Sarabun, ตอบสนองทุกอุปกรณ์, UI ภาษาไทย)

7. โครงสร้างฐานข้อมูล (Supabase - PostgreSQL) - ชื่อตารางและคอลัมน์เป็นภาษาอังกฤษ
profiles: id (UUID), display_name (TEXT), avatar_url (TEXT), role_id (INT), created_at, updated_at

roles: id (SERIAL), name (TEXT), permissions (JSONB)

categories: id (SERIAL), name (TEXT), slug (TEXT), created_at

novels: id (SERIAL), title (TEXT), slug (TEXT), synopsis (TEXT), cover_image_url (TEXT), author_id (UUID), category_id (INT), original_language (TEXT), visibility (TEXT), default_chapter_status (TEXT), default_chapter_access (TEXT), total_chapters (INT), published_chapters (INT), created_at, updated_at, last_published_at

chapters

id (SERIAL), novel_id (INT), chapter_number (INT), title (TEXT), slug (TEXT), content_original (TEXT), status (TEXT), access_level (TEXT), created_by (UUID), word_count (INT), created_at, updated_at, published_at

read_by_users (JSONB, Nullable, Default {}): เก็บข้อมูลว่า User ID ใดอ่านตอนนี้แล้วบ้าง เช่น {"user_uuid_1": true, "user_uuid_2": true} เพื่อใช้แสดงสถานะ "อ่านแล้ว"

bookmarks: id (SERIAL), user_id (UUID), chapter_id (INT), created_at

mark_lines

id (SERIAL, Primary Key)

chapter_id (INTEGER, Foreign Key อ้างอิง chapters.id, Not Null)

translator_id (UUID, Foreign Key อ้างอิง profiles.id, Not Null)

marked_lines_data (JSONB, Nullable, Default []):

โครงสร้าง: Array ของหมายเลขบรรทัด (Integers)

ตัวอย่างข้อมูล: [21, 40, 39], [8, 32, 48, 37], [] (หากไม่มีการมาร์ค)

การนำเข้าข้อมูลเก่า: คุณสามารถ Copy & Paste Array ของหมายเลขบรรทัดจาก DB เก่าของคุณมาใส่ใน Field นี้ได้โดยตรงผ่าน Supabase Studio หรือ Interface ที่สร้างขึ้นสำหรับผู้ดูแลระบบ

created_at (TIMESTAMP WITH TIME ZONE, Default now())

updated_at (TIMESTAMP WITH TIME ZONE, Default now())

UNIQUE (chapter_id, translator_id) - นักแปลคนเดียวสามารถมีชุดบรรทัดมาร์คเดียวต่อหนึ่งตอน

chapter_versions: (Optional) id, chapter_id, version_number, title, content_original, changed_by, created_at

7.1 มุมมองข้อมูล MarkLine สำหรับผู้ดูแลระบบ (Conceptual View/Query) - ชื่อคอลัมน์ใน View เป็นภาษาอังกฤษ, Comment เป็นภาษาไทย
เพื่อให้ผู้ดูแลระบบสามารถตรวจสอบและจัดการข้อมูลบรรทัดมาร์คได้ง่าย อาจสร้าง View ใน Supabase หรือใช้ Query ลักษณะนี้:

CREATE VIEW admin_markline_overview AS
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
    jsonb_array_length(ml.marked_lines_data) AS total_marks_in_chapter, -- จำนวนบรรทัดที่มาร์คในตอนนี้
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

การใช้งาน: ผู้ดูแลระบบสามารถ Query จาก View นี้เพื่อดูว่าใครมาร์คอะไรไว้บ้างในแต่ละตอนของแต่ละนิยาย และสามารถใช้ WHERE clause เพื่อกรองข้อมูลตามต้องการได้

การแก้ไข: การแก้ไขข้อมูล marked_lines_data (Array ของหมายเลขบรรทัด) โดยตรงผ่าน Supabase Studio หรือ Interface ที่สร้างขึ้นสำหรับผู้ดูแลระบบ จะกระทำบนตาราง mark_lines โดยอ้างอิงจาก markline_id หรือ chapter_id และ translator_id

หมายเหตุเกี่ยวกับ Database:

การตั้ง Default Value ของ mark_lines.marked_lines_data เป็น [] (JSON Array ว่าง) จะช่วยให้เมื่อมีการสร้าง Record ใหม่ จะมีข้อมูลพร้อมสำหรับการวางทับทันที

Database ได้รับการออกแบบให้รองรับการเก็บบรรทัดมาร์คสำหรับตอนจำนวนมาก (เช่น นักแปลอ่านไว้ 100 ตอน) โดยแต่ละตอนจะมี Record ของตัวเองในตาราง mark_lines (หากมีการมาร์ค)

การเพิ่ม chapters.read_by_users เพื่อติดตามสถานะการอ่านของแต่ละ User สำหรับแต่ละตอน

8. ประสบการณ์ผู้ใช้งาน (Leading UX & User Flows) - UI ภาษาไทย
8.1 การเข้าสู่ระบบ และหน้าแรก (Login & Home Page Experience)

(เหมือนเดิม - UI ภาษาไทย)

คลิกปกนิยายบนหน้าแรก -> ไปยัง "หน้าข้อมูลนิยายและสารบัญ (/[novel_slug])"

8.2 การเข้าดูนิยายและสารบัญ (Novel Detail Page Flow)

(เหมือนเดิม - UI ภาษาไทย, เพิ่มปุ่มจัดการนิยาย/ตอน, สารบัญแบบ Collapse)

เมื่อผู้ใช้อ่านตอนจบ: ระบบจะบันทึก user_id ของผู้ใช้ลงใน chapters.read_by_users ของตอนนั้น

8.3 การใช้งานบุ๊คมาร์ค (Bookmark Functionality)

(เหมือนเดิม - UI ภาษาไทย)

8.4 ขั้นตอนการแปลเบื้องต้น และการจัดการเส้นมาร์ค (Translator Flow)

การสร้างตอนใหม่ (โดยนักแปล):

นักแปลสร้างตอนใหม่ (ผ่าน Modal เพิ่มตอนด้วย .txt หรือ Modal เพิ่ม/แก้ไขตอน)

Backend:

ข้อมูลตอนถูกบันทึกลงตาราง chapters

ระบบ (เช่น Trigger หรือ Supabase Function) สร้าง Record ใหม่ในตาราง mark_lines โดยอัตโนมัติ:

chapter_id: ID ของตอนที่เพิ่งสร้าง

translator_id: ID ของนักแปลผู้สร้างตอน

marked_lines_data: [] (JSON Array ว่าง)

created_at, updated_at: เวลาปัจจุบัน

นักแปลเข้าสู่ "โหมดแปล" ของตอนที่ต้องการ:

หากเป็นครั้งแรกที่นักแปลคนนี้เข้าถึงตอนนี้ และยังไม่มี Record ใน mark_lines สำหรับ chapter_id และ translator_id นี้ ระบบจะสร้าง Record ใหม่ให้โดยอัตโนมัติด้วย marked_lines_data = []

การมาร์คบรรทัด: นักแปลคลิกที่บรรทัด -> หมายเลขบรรทัดของบรรทัดนั้นจะถูกเพิ่มเข้าไปใน Array ของ mark_lines.marked_lines_data (หากยังไม่มี) หรือลบออก (หากมีอยู่แล้ว) สำหรับนักแปลคนนั้นและตอนนี้

การล้างมาร์ค: คลิกปุ่ม "ล้างมาร์คทั้งหมด" -> Modal ยืนยัน -> หากยืนยัน mark_lines.marked_lines_data สำหรับนักแปลคนนั้นและตอนนี้จะถูกตั้งค่าเป็น [] และจำนวนมาร์คที่แสดงผลจะอัปเดต

การคัดลอกมาร์คทั้งหมด (Copy All Marks):

นักแปลคลิกปุ่ม "คัดลอกมาร์คทั้งหมด"

Backend/Frontend Logic:

ดึงข้อมูล marked_lines_data (Array ของหมายเลขบรรทัด) จากตาราง mark_lines สำหรับ chapter_id และ translator_id ปัจจุบัน

ดึงข้อมูล content_original จากตาราง chapters สำหรับ chapter_id ปัจจุบัน

แยก content_original ออกเป็น Array ของบรรทัด (เช่น โดยการ split ด้วย \n)

สร้าง String ใหม่โดยวน Loop ตามหมายเลขบรรทัดที่อยู่ใน marked_lines_data:

สำหรับแต่ละหมายเลขบรรทัด ให้ดึงข้อความจาก Array ของบรรทัดที่ได้จาก content_original (หมายเลขบรรทัดมักจะเริ่มจาก 1 ดังนั้นอาจต้องปรับ index เป็น lineNumber - 1)

นำข้อความของบรรทัดที่มาร์คไว้มาต่อกัน (อาจคั่นด้วย \n หรือเว้นวรรค ตามความต้องการ)

คัดลอก String ที่ได้ไปยัง Clipboard ของผู้ใช้

แสดง Feedback (เช่น Toast "คัดลอกข้อความที่มาร์คแล้ว")

การดับเบิลคลิก: คัดลอกข้อความต้นฉบับของบรรทัดนั้นๆ

การสลับธีม: สามารถทำได้ตลอดเวลาผ่านแถบเครื่องมือ

(สำหรับผู้ดูแลระบบ/ผู้ใช้ที่มีสิทธิ์) การวางข้อมูลบรรทัดมาร์คเก่า:

ผู้ใช้สามารถเข้าถึงตาราง mark_lines (ผ่าน Supabase Studio หรือ Admin Interface)

ค้นหา Record ที่ต้องการ (ตาม chapter_id และ translator_id)

แก้ไข Field marked_lines_data โดยวาง Array ของหมายเลขบรรทัดที่ต้องการทับลงไป (เช่น [21, 40, 39])

ระบบรองรับการวางข้อมูลบรรทัดมาร์คสำหรับหลายๆ ตอน (10-20 ตอน หรือมากกว่า) โดยการแก้ไข Record ที่เกี่ยวข้องทีละ Record หรือผ่านการ Import ข้อมูลหาก Supabase หรือ Admin Interface รองรับ

9. ระบบความปลอดภัย (Security)
Authentication: JWT Auth พร้อม Refresh Token (จัดการโดย Supabase Auth)

Transport Security: HTTPS ตลอดการเชื่อมต่อ

API Security:

Rate Limiting เพื่อป้องกันการโจมตีแบบ brute-force หรือ DoS

ป้องกัน XSS (Cross-Site Scripting) โดยการ sanitize user input และใช้ Content Security Policy (CSP) ที่เหมาะสม

ป้องกัน CSRF (Cross-Site Request Forgery) โดยการใช้ anti-CSRF tokens (Supabase อาจมีกลไกจัดการส่วนนี้)

ตรวจสอบสิทธิ์ (Authorization) ก่อนเรียกใช้งาน API ทุกตัว โดยอิงตาม Role และ Permissions (RLS)

Data Security:

Row-Level Security (RLS) ใน Supabase PostgreSQL เพื่อให้แน่ใจว่าผู้ใช้สามารถเข้าถึงได้เฉพาะข้อมูลที่ตนเองมีสิทธิ์

Account Security:

ฟีเจอร์ยืนยันอีเมล (Email Verification) สำหรับบัญชีใหม่

การป้องกันการเข้าสู่ระบบที่ไม่สำเร็จหลายครั้ง (Login attempt throttling)

10. การนำไปใช้งานและการติดตามผล (Deployment & Monitoring)
DevOps (CI/CD):

ใช้ GitHub Actions สำหรับการทำ Continuous Integration และ Continuous Deployment

เมื่อมีการ push code ไปยัง branch ที่กำหนด (เช่น main, staging) จะ trigger workflow ให้ build และ deploy ไปยัง Vercel โดยอัตโนมัติ

Environments:

.env.local: สำหรับการพัฒนาบนเครื่อง local (เก็บ Supabase URL, anon key)

.env.staging: สำหรับ Vercel staging environment (ถ้ามี)

.env.production: สำหรับ Vercel production environment

จัดการ Environment Variables ผ่าน Vercel dashboard

Monitoring:

Sentry: สำหรับติดตาม Error และ Exceptions ที่เกิดขึ้นใน Frontend และ Backend (ถ้ามีการใช้ Supabase Functions)

Grafana: (อาจใช้ร่วมกับ Prometheus หรือ InfluxDB ถ้าตั้งค่าเอง) สำหรับติดตาม Load, Performance metrics ของ Server/Database (Supabase มี Dashboard ของตัวเองสำหรับดู performance เบื้องต้น)

Supabase Logs: ตรวจสอบ Logs การใช้งาน API, Database queries, และ Auth events ผ่าน Supabase Dashboard

11. การทดสอบและประกันคุณภาพ (Testing & QA)
Unit Test: ใช้ Jest (หรือ Vitest) ร่วมกับ React Testing Library (RTL) สำหรับทดสอบ Components และ Functions ใน Frontend

End-to-End (E2E) Test: ใช้ Cypress สำหรับการทดสอบ User Flows ทั้งหมดของแอปพลิเคชัน (เช่น การสมัครสมาชิก, การเข้าสู่ระบบ, การสร้างนิยาย, การแปล)

Load Test: ใช้ k6 (หรือ JMeter) สำหรับทดสอบประสิทธิภาพของระบบภายใต้ภาระงานสูง เพื่อหาจุดคอขวดและประเมินความสามารถในการรองรับผู้ใช้

Accessibility (A11y) Test: ใช้ axe-core (หรือ Lighthouse ใน Chrome DevTools) เพื่อตรวจสอบและปรับปรุงการเข้าถึงเว็บไซต์สำหรับผู้พิการ

12. การปรับปรุงประสบการณ์ผู้ใช้ (UX Enhancements - Responsive Focus)
Empty States: ทุกส่วนที่มีการแสดงรายการข้อมูล (เช่น หน้าแรกไม่มีนิยาย, ไม่มีบุ๊คมาร์ค, ไม่มีผลการค้นหา) ควรมี Empty State ที่สวยงาม พร้อม Call-to-action ที่เหมาะสม

Loading States:

Skeleton Loaders: ขณะรอข้อมูลโหลด แสดงโครงร่างของ UI (เช่น โครง Card นิยาย) เพื่อให้ผู้ใช้รู้สึกว่ากำลังโหลด

Spinners/Progress Bars: สำหรับ action ที่ใช้เวลานาน (เช่น อัปโหลดไฟล์, บันทึกข้อมูล)

Notifications/Toasts:

ใช้สำหรับแจ้งผลการกระทำที่ไม่เปลี่ยนหน้า (เช่น "บันทึกการเปลี่ยนแปลงสำเร็จ", "คัดลอกข้อความแล้ว", "เพิ่มในบุ๊คมาร์คแล้ว")

ควรปรากฏในตำแหน่งที่ไม่บดบังเนื้อหาสำคัญ และหายไปเองในเวลาที่เหมาะสม (ใช้ DaisyUI Alert หรือ Toast)

Error Handling & Feedback:

แสดงข้อความ Error ที่เป็นมิตร เข้าใจง่าย และบอกแนวทางการแก้ไข (ถ้าทำได้)

หน้า 404 (Not Found) และ 500 (Server Error) ที่ออกแบบมาเฉพาะสำหรับ INKREALM

Accessibility (A11y):

Contrast ratio เพียงพอ

Keyboard navigation สมบูรณ์

ARIA attributes ที่เหมาะสมสำหรับ dynamic content

Alt text สำหรับรูปภาพ

Transitions & Micro-interactions:

การเปลี่ยนหน้า (page transitions) ที่นุ่มนวล

Hover effects, button press effects ที่ตอบสนองต่อผู้ใช้

ทั้งหมดนี้ควร subtle ไม่ใช่รกหรือทำให้เว็บช้า

© 2025 INKREALM.CO – เอกสารฉบับสมบูรณ์ (UI ภาษาไทย, ฟอนต์ Sarabun, ธีมสีทอง-เทา, Responsive Design, Database & UX) V4.14
