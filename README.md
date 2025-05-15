-- ========== EXTENSIONS (ถ้ายังไม่มี) ==========
-- CREATE EXTENSION IF NOT EXISTS moddatetime SCHEMA extensions; -- สำหรับ auto updated_at
-- CREATE EXTENSION IF NOT EXISTS "uuid-ossp" SCHEMA extensions; -- สำหรับ uuid_generate_v4()

-- ========== ENUM TYPES (ถ้าต้องการใช้ แทน TEXT สำหรับบางฟิลด์) ==========
CREATE TYPE novel_status_enum AS ENUM ('ต่อเนื่อง', 'จบแล้ว', 'ยุติการลง', 'เผยแพร่', 'ฉบับร่าง');
CREATE TYPE chapter_status_enum AS ENUM ('เผยแพร่', 'ฉบับร่าง');
CREATE TYPE language_enum AS ENUM ('zh', 'en', 'th');
CREATE TYPE visibility_enum AS ENUM ('ทุกคน', 'เฉพาะสมาชิก', 'เฉพาะนักแปล'); -- อาจใช้ JSONB ถ้าต้องการหลายค่า

-- ========== ROLES TABLE ==========
CREATE TABLE public.roles (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL UNIQUE,
    description TEXT
);

-- Seed initial roles
INSERT INTO public.roles (name, description) VALUES
('Member', 'สมาชิกทั่วไป'),
('Translator', 'นักแปล'),
('Admin', 'ผู้ดูแลระบบ');

-- ========== PROFILES TABLE (เชื่อมกับ auth.users) ==========
CREATE TABLE public.profiles (
    id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
    display_name TEXT,
    avatar_url TEXT,
    role_id INTEGER NOT NULL REFERENCES public.roles(id) DEFAULT 1, -- Default to Member
    role_expiry_date TIMESTAMP WITH TIME ZONE NULL,
    is_suspended BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);

-- RLS for profiles
ALTER TABLE public.profiles ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow individual user to read their own profile" ON public.profiles FOR SELECT USING (auth.uid() = id);
CREATE POLICY "Allow individual user to update their own profile" ON public.profiles FOR UPDATE USING (auth.uid() = id) WITH CHECK (auth.uid() = id);
CREATE POLICY "Admins can manage all profiles" ON public.profiles FOR ALL USING (
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
);

-- Trigger to automatically update `updated_at`
CREATE TRIGGER handle_updated_at_profiles
BEFORE UPDATE ON public.profiles
FOR EACH ROW
EXECUTE FUNCTION extensions.moddatetime (updated_at);

-- Function to create a profile when a new user signs up
CREATE OR REPLACE FUNCTION public.handle_new_user()
RETURNS TRIGGER
LANGUAGE plpgsql
SECURITY DEFINER SET search_path = public
AS $$
BEGIN
  INSERT INTO public.profiles (id, display_name, role_id)
  VALUES (
    NEW.id,
    NEW.raw_user_meta_data->>'display_name', -- Or some default if not provided
    (SELECT id FROM public.roles WHERE name = 'Member') -- Default role
  );
  RETURN NEW;
END;
$$;

-- Trigger to call handle_new_user on new user creation
CREATE TRIGGER on_auth_user_created
  AFTER INSERT ON auth.users
  FOR EACH ROW EXECUTE PROCEDURE public.handle_new_user();


-- ========== CATEGORIES TABLE ==========
CREATE TABLE public.categories (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL UNIQUE,
    slug TEXT NOT NULL UNIQUE,
    description TEXT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);
-- RLS for categories
ALTER TABLE public.categories ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow public read access to categories" ON public.categories FOR SELECT USING (true);
CREATE POLICY "Allow admins to manage categories" ON public.categories FOR ALL USING (
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
);
-- Trigger for categories updated_at
CREATE TRIGGER handle_updated_at_categories
BEFORE UPDATE ON public.categories
FOR EACH ROW
EXECUTE FUNCTION extensions.moddatetime (updated_at);

-- ========== NOVELS TABLE ==========
CREATE TABLE public.novels (
    id UUID PRIMARY KEY DEFAULT extensions.uuid_generate_v4(),
    title TEXT NOT NULL,
    slug TEXT NOT NULL UNIQUE,
    synopsis TEXT,
    cover_image_url TEXT,
    category_id INTEGER REFERENCES public.categories(id) ON DELETE SET NULL,
    author_id UUID NOT NULL REFERENCES public.profiles(id) ON DELETE CASCADE, -- The translator/creator on this platform
    original_language language_enum NOT NULL DEFAULT 'th',
    status novel_status_enum NOT NULL DEFAULT 'ฉบับร่าง',
    visibility JSONB DEFAULT '["ทุกคน"]'::jsonb, -- e.g., ["ทุกคน"], ["เฉพาะสมาชิก"], ["เฉพาะนักแปล", "เฉพาะสมาชิก"]
    default_chapter_status chapter_status_enum NOT NULL DEFAULT 'เผยแพร่',
    default_chapter_access_level JSONB DEFAULT '["ทุกคน"]'::jsonb,
    total_chapters INTEGER DEFAULT 0,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);
-- Index for novels
CREATE INDEX idx_novels_author_id ON public.novels(author_id);
CREATE INDEX idx_novels_category_id ON public.novels(category_id);
CREATE INDEX idx_novels_status ON public.novels(status);
-- RLS for novels (Example - needs refinement based on exact logic)
ALTER TABLE public.novels ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow public read for published novels accessible to everyone"
  ON public.novels FOR SELECT USING (
    status = 'เผยแพร่' AND visibility @> '["ทุกคน"]'::jsonb
  );
CREATE POLICY "Allow member read for published novels accessible to members"
  ON public.novels FOR SELECT USING (
    status = 'เผยแพร่' AND visibility @> '["เฉพาะสมาชิก"]'::jsonb AND
    (SELECT role_id FROM profiles WHERE id = auth.uid()) IS NOT NULL -- checks if user is logged in
  );
CREATE POLICY "Allow translator/admin to read their novels regardless of status/visibility"
  ON public.novels FOR SELECT USING (
    author_id = auth.uid() OR
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
CREATE POLICY "Allow author (translator/admin) to create novels"
  ON public.novels FOR INSERT WITH CHECK (
    author_id = auth.uid() AND
    ( (SELECT role_id FROM profiles WHERE id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Translator') OR
      (SELECT role_id FROM profiles WHERE id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin') )
  );
CREATE POLICY "Allow author to update their own novels"
  ON public.novels FOR UPDATE USING (author_id = auth.uid()) WITH CHECK (author_id = auth.uid());
CREATE POLICY "Allow admin to update any novel"
  ON public.novels FOR UPDATE USING (
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
CREATE POLICY "Allow author to delete their own novels"
  ON public.novels FOR DELETE USING (author_id = auth.uid());
CREATE POLICY "Allow admin to delete any novel"
  ON public.novels FOR DELETE USING (
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
-- Trigger for novels updated_at
CREATE TRIGGER handle_updated_at_novels
BEFORE UPDATE ON public.novels
FOR EACH ROW
EXECUTE FUNCTION extensions.moddatetime (updated_at);


-- ========== CHAPTERS TABLE ==========
CREATE TABLE public.chapters (
    id UUID PRIMARY KEY DEFAULT extensions.uuid_generate_v4(),
    novel_id UUID NOT NULL REFERENCES public.novels(id) ON DELETE CASCADE,
    chapter_number INTEGER NOT NULL,
    title TEXT NOT NULL,
    -- slug TEXT NOT NULL, -- URL uses chapter_number, slug here might be for other purposes if needed
    content TEXT, -- HTML content from Rich Text Editor
    content_original TEXT, -- Original text for translator mode
    status chapter_status_enum NOT NULL DEFAULT 'ฉบับร่าง',
    access_level JSONB DEFAULT '["ทุกคน"]'::jsonb, -- e.g., ["ทุกคน"], ["เฉพาะสมาชิก"]
    created_by UUID NOT NULL REFERENCES public.profiles(id) ON DELETE RESTRICT, -- Creator of the chapter (usually same as novel author)
    word_count INTEGER DEFAULT 0,
    read_by_users JSONB DEFAULT '[]'::jsonb, -- Array of user_ids who have read this chapter
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    CONSTRAINT unique_novel_chapter_number UNIQUE (novel_id, chapter_number)
);
-- Index for chapters
CREATE INDEX idx_chapters_novel_id ON public.chapters(novel_id);
-- RLS for chapters (Example - needs refinement)
ALTER TABLE public.chapters ENABLE ROW LEVEL SECURITY;
-- Public read for published chapters accessible to everyone
CREATE POLICY "Public read access for chapters" ON public.chapters
  FOR SELECT USING (
    status = 'เผยแพร่' AND access_level @> '["ทุกคน"]'::jsonb AND
    (SELECT novels.status FROM public.novels WHERE novels.id = chapters.novel_id) = 'เผยแพร่' AND
    (SELECT novels.visibility FROM public.novels WHERE novels.id = chapters.novel_id) @> '["ทุกคน"]'::jsonb
  );
-- Member read for published chapters accessible to members
CREATE POLICY "Member read access for chapters" ON public.chapters
  FOR SELECT USING (
    status = 'เผยแพร่' AND access_level @> '["เฉพาะสมาชิก"]'::jsonb AND
    (SELECT novels.status FROM public.novels WHERE novels.id = chapters.novel_id) = 'เผยแพร่' AND
    (SELECT novels.visibility FROM public.novels WHERE novels.id = chapters.novel_id) @> '["เฉพาะสมาชิก"]'::jsonb AND
    (SELECT role_id FROM profiles WHERE id = auth.uid()) IS NOT NULL
  );
-- Translator read for their own chapters
CREATE POLICY "Translator/Admin read their own/managed chapters" ON public.chapters
  FOR SELECT USING (
    created_by = auth.uid() OR
    (SELECT novels.author_id FROM public.novels WHERE novels.id = chapters.novel_id) = auth.uid() OR
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
-- Allow novel author or admin to manage chapters
CREATE POLICY "Author/Admin can manage chapters of their novels" ON public.chapters
  FOR ALL USING (
    (SELECT novels.author_id FROM public.novels WHERE novels.id = chapters.novel_id) = auth.uid() OR
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  ) WITH CHECK (
    (SELECT novels.author_id FROM public.novels WHERE novels.id = chapters.novel_id) = auth.uid() OR
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
-- Trigger for chapters updated_at
CREATE TRIGGER handle_updated_at_chapters
BEFORE UPDATE ON public.chapters
FOR EACH ROW
EXECUTE FUNCTION extensions.moddatetime (updated_at);

-- Function to update novel's total_chapters count
CREATE OR REPLACE FUNCTION public.update_novel_total_chapters()
RETURNS TRIGGER
LANGUAGE plpgsql
AS $$
BEGIN
  IF (TG_OP = 'INSERT') THEN
    UPDATE public.novels
    SET total_chapters = total_chapters + 1
    WHERE id = NEW.novel_id;
  ELSIF (TG_OP = 'DELETE') THEN
    UPDATE public.novels
    SET total_chapters = total_chapters - 1
    WHERE id = OLD.novel_id;
  END IF;
  RETURN NULL; -- result is ignored since this is an AFTER trigger
END;
$$;
-- Trigger to update total_chapters
CREATE TRIGGER after_chapter_insert_delete
AFTER INSERT OR DELETE ON public.chapters
FOR EACH ROW EXECUTE FUNCTION public.update_novel_total_chapters();


-- ========== USER_BOOKMARKS TABLE ==========
CREATE TABLE public.user_bookmarks (
    id SERIAL PRIMARY KEY,
    user_id UUID NOT NULL REFERENCES public.profiles(id) ON DELETE CASCADE,
    chapter_id UUID NOT NULL REFERENCES public.chapters(id) ON DELETE CASCADE,
    novel_id UUID NOT NULL REFERENCES public.novels(id) ON DELETE CASCADE, -- Denormalized for easier querying
    bookmarked_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    CONSTRAINT unique_user_chapter_bookmark UNIQUE (user_id, chapter_id)
);
-- Index for user_bookmarks
CREATE INDEX idx_user_bookmarks_user_novel ON public.user_bookmarks(user_id, novel_id);
-- RLS for user_bookmarks
ALTER TABLE public.user_bookmarks ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users can manage their own bookmarks" ON public.user_bookmarks
  FOR ALL USING (user_id = auth.uid()) WITH CHECK (user_id = auth.uid());


-- ========== MARK_LINES TABLE (for Translator Mode) ==========
CREATE TABLE public.mark_lines (
    id SERIAL PRIMARY KEY,
    chapter_id UUID NOT NULL REFERENCES public.chapters(id) ON DELETE CASCADE,
    user_id UUID NOT NULL REFERENCES public.profiles(id) ON DELETE CASCADE, -- Translator who marked
    line_number INTEGER NOT NULL, -- The line number in content_original
    marked_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    CONSTRAINT unique_mark_line_user_chapter UNIQUE (chapter_id, user_id, line_number)
);
-- Index for mark_lines
CREATE INDEX idx_mark_lines_user_chapter ON public.mark_lines(user_id, chapter_id);
-- RLS for mark_lines
ALTER TABLE public.mark_lines ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Translators can manage their own marks" ON public.mark_lines
  FOR ALL USING (
    user_id = auth.uid() AND
    ( (SELECT role_id FROM profiles WHERE id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Translator') OR
      (SELECT role_id FROM profiles WHERE id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin') )
  ) WITH CHECK (
    user_id = auth.uid() AND
    ( (SELECT role_id FROM profiles WHERE id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Translator') OR
      (SELECT role_id FROM profiles WHERE id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin') )
  );
CREATE POLICY "Admins can view all marks" ON public.mark_lines
    FOR SELECT USING ((SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin'));


-- ========== ANNOUNCEMENTS TABLE ==========
CREATE TABLE public.announcements (
    id SERIAL PRIMARY KEY,
    title TEXT NOT NULL,
    content TEXT NOT NULL, -- Markdown content
    start_date TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT timezone('utc'::text, now()),
    end_date TIMESTAMP WITH TIME ZONE NOT NULL,
    target_roles JSONB DEFAULT '["ทุกคน"]'::jsonb, -- e.g., ["Member"], ["Translator"] or role_ids
    target_pages JSONB DEFAULT '["หน้าแรก"]'::jsonb, -- e.g., ["หน้าแรก"], ["หน้านิยาย (ทุกเรื่อง)"], ["หน้านิยาย (เฉพาะเรื่อง):novel_id_or_slug"]
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);
-- RLS for announcements
ALTER TABLE public.announcements ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow public read for active announcements based on role and page" ON public.announcements
  FOR SELECT USING (
    is_active = TRUE AND
    timezone('utc'::text, now()) BETWEEN start_date AND end_date AND
    ( target_roles @> jsonb_build_array((SELECT r.name FROM public.roles r JOIN public.profiles p ON p.role_id = r.id WHERE p.id = auth.uid())) OR
      target_roles @> '["ทุกคน"]'::jsonb OR
      (auth.uid() IS NULL AND target_roles @> '["ผู้เยี่ยมชม"]'::jsonb) -- For guests if you add "ผู้เยี่ยมชม"
      -- Logic for target_pages needs to be handled client-side or via RPC, as RLS cannot easily know the current page.
    )
  );
CREATE POLICY "Allow admins to manage announcements" ON public.announcements
  FOR ALL USING (
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
-- Trigger for announcements updated_at
CREATE TRIGGER handle_updated_at_announcements
BEFORE UPDATE ON public.announcements
FOR EACH ROW
EXECUTE FUNCTION extensions.moddatetime (updated_at);


-- ========== SITE_SETTINGS TABLE ==========
CREATE TABLE public.site_settings (
    key TEXT PRIMARY KEY,
    value JSONB,
    description TEXT,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);
-- Seed initial site settings
INSERT INTO public.site_settings (key, value, description) VALUES
('site_name', '"INKREALM.CO"'::jsonb, 'ชื่อเว็บไซต์หลัก'),
('site_description', '"แพลตฟอร์มนิยายแปลสำหรับทุกคน"'::jsonb, 'คำอธิบายเว็บไซต์สำหรับ SEO'),
('logo_url', 'null'::jsonb, 'URL ของโลโก้เว็บไซต์'),
('favicon_url', 'null'::jsonb, 'URL ของ Favicon'),
('allow_registration', 'true'::jsonb, 'เปิด/ปิดการลงทะเบียนใหม่ (true/false)'),
('allow_google_login', 'true'::jsonb, 'เปิด/ปิดการเข้าสู่ระบบด้วย Google (true/false)'),
('default_items_per_page', '20'::jsonb, 'จำนวนรายการต่อหน้าเริ่มต้นสำหรับการแบ่งหน้า'),
('novel_default_chapter_status', '"เผยแพร่"'::jsonb, 'สถานะตอนเริ่มต้นสำหรับนิยายใหม่ ("เผยแพร่" หรือ "ฉบับร่าง")'),
('novel_default_chapter_access', '["ทุกคน"]'::jsonb, 'สิทธิ์เข้าถึงตอนเริ่มต้นสำหรับนิยายใหม่ (JSON Array)');
-- RLS for site_settings
ALTER TABLE public.site_settings ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow public read for site settings" ON public.site_settings FOR SELECT USING (true);
CREATE POLICY "Allow admins to manage site settings" ON public.site_settings
  FOR ALL USING (
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
-- Trigger for site_settings updated_at
CREATE TRIGGER handle_updated_at_site_settings
BEFORE UPDATE ON public.site_settings
FOR EACH ROW
EXECUTE FUNCTION extensions.moddatetime (updated_at);


-- ========== SITE_CHANGELOGS TABLE ==========
CREATE TABLE public.site_changelogs (
    id SERIAL PRIMARY KEY,
    version_number TEXT NOT NULL UNIQUE,
    release_date DATE NOT NULL,
    summary TEXT, -- Markdown content
    is_published BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);
-- RLS for site_changelogs
ALTER TABLE public.site_changelogs ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow public read for published changelogs" ON public.site_changelogs FOR SELECT USING (is_published = TRUE);
CREATE POLICY "Allow admins to manage changelogs" ON public.site_changelogs
  FOR ALL USING (
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );


-- ========== NOVEL_VERSIONS TABLE (for tracking changes to novel metadata) ==========
CREATE TABLE public.novel_versions (
    id SERIAL PRIMARY KEY,
    novel_id UUID NOT NULL REFERENCES public.novels(id) ON DELETE CASCADE,
    version_number INTEGER NOT NULL,
    metadata_snapshot JSONB NOT NULL, -- Snapshot of novel fields (title, synopsis, category, status, etc.)
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    created_by UUID REFERENCES public.profiles(id) ON DELETE SET NULL, -- User who made the change
    notes TEXT, -- Optional notes about the version
    UNIQUE(novel_id, version_number)
);
-- RLS for novel_versions
ALTER TABLE public.novel_versions ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow admins and novel author to read novel versions" ON public.novel_versions
  FOR SELECT USING (
    (SELECT novels.author_id FROM public.novels WHERE novels.id = novel_versions.novel_id) = auth.uid() OR
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
CREATE POLICY "Allow admins to manage novel versions (primarily for restore, creation is programmatic)" ON public.novel_versions
  FOR ALL USING ( -- Be cautious with direct manipulation
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );


-- ========== CHAPTER_VERSIONS TABLE (for tracking changes to chapter content) ==========
CREATE TABLE public.chapter_versions (
    id SERIAL PRIMARY KEY,
    chapter_id UUID NOT NULL REFERENCES public.chapters(id) ON DELETE CASCADE,
    version_number INTEGER NOT NULL,
    content_snapshot TEXT, -- Snapshot of chapter's HTML content
    content_original_snapshot TEXT, -- Snapshot of chapter's original text content
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    created_by UUID REFERENCES public.profiles(id) ON DELETE SET NULL, -- User who made the change
    notes TEXT, -- Optional notes about the version
    UNIQUE(chapter_id, version_number)
);
-- RLS for chapter_versions
ALTER TABLE public.chapter_versions ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow admins and chapter author to read chapter versions" ON public.chapter_versions
  FOR SELECT USING (
    (SELECT chapters.created_by FROM public.chapters WHERE chapters.id = chapter_versions.chapter_id) = auth.uid() OR
    (SELECT novels.author_id FROM public.novels JOIN public.chapters ON novels.id = chapters.novel_id WHERE chapters.id = chapter_versions.chapter_id) = auth.uid() OR
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );
CREATE POLICY "Allow admins to manage chapter versions (primarily for restore, creation is programmatic)" ON public.chapter_versions
  FOR ALL USING ( -- Be cautious with direct manipulation
    (SELECT profiles.role_id FROM public.profiles WHERE profiles.id = auth.uid()) = (SELECT id FROM public.roles WHERE name = 'Admin')
  );

-- ========== STORAGE BUCKETS SETUP (Run these in Supabase SQL Editor or via Management API) ==========
-- Note: RLS for storage is typically managed through policies on the storage.objects table
-- For `novel-covers`: Public read, authenticated insert/update/delete by author/admin
-- For `avatars`: Public read, authenticated insert/update by owner, admin can manage
-- For `site_assets`: Public read, admin insert/update/delete
-- For `chapter-text-files`: Private, authenticated insert by author/admin, server-side delete after processing.

-- Example for novel-covers (you'll need to adjust based on your exact RLS needs for storage)
/*
INSERT INTO storage.buckets (id, name, public)
VALUES ('novel-covers', 'novel-covers', true); -- Public true means files are accessible via URL without token if policy allows

CREATE POLICY "Allow authenticated users to upload covers"
ON storage.objects FOR INSERT TO authenticated WITH CHECK (bucket_id = 'novel-covers');
-- Add more specific policies for select, update, delete based on user roles and ownership.
-- e.g., only novel author or admin can update/delete their novel's cover.
-- This often involves checking against a metadata column in storage.objects or joining with your public tables.
-- Supabase documentation provides good examples for storage RLS.
*/

-- Example for avatars
/*
INSERT INTO storage.buckets (id, name, public)
VALUES ('avatars', 'avatars', true);

CREATE POLICY "Allow users to upload their own avatar"
ON storage.objects FOR INSERT TO authenticated WITH CHECK (
    bucket_id = 'avatars' AND owner = auth.uid()
);
CREATE POLICY "Allow users to update their own avatar"
ON storage.objects FOR UPDATE TO authenticated USING (
    bucket_id = 'avatars' AND owner = auth.uid()
);
CREATE POLICY "Allow users to view their own avatar or public avatars" -- (if avatars are public)
ON storage.objects FOR SELECT USING (bucket_id = 'avatars');
*/

-- Remember to set up RLS policies for your storage buckets according to your access control needs.
-- The `public` flag on a bucket just makes it *possible* for files to be public, RLS still applies.
