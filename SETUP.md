# Setup Guide – Arnold & Isaac Gallery

## What you're adding to GitHub
- `gallery.html` → Your page with the QR code (you display this at the venue)
- `share.html`   → What guests see when they scan the QR code

---

## Step 1 – Create a FREE Supabase account (5 min)

1. Go to **supabase.com** and sign up for free
2. Click **New Project**
3. Name it `wedding-gallery`, set a password, click Create

---

## Step 2 – Create the database table

1. In your Supabase project, click **SQL Editor** on the left
2. Paste this and click **Run**:

```sql
CREATE TABLE uploads (
  id uuid DEFAULT gen_random_uuid() PRIMARY KEY,
  url text NOT NULL,
  type text NOT NULL,
  created_at timestamptz DEFAULT now()
);

ALTER TABLE uploads ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Public read"   ON uploads FOR SELECT USING (true);
CREATE POLICY "Public insert" ON uploads FOR INSERT WITH CHECK (true);
```

---

## Step 3 – Create the storage bucket

1. Click **Storage** on the left sidebar
2. Click **New Bucket**, name it exactly: `wedding-gallery`
3. Toggle **Public bucket** to ON
4. Click **Policies** → Add two policies:
   - Allow public **SELECT** (so everyone can view)
   - Allow public **INSERT** (so guests can upload)

---

## Step 4 – Get your keys

1. Click **Settings** → **API** in Supabase
2. Copy your:
   - **Project URL** (looks like: https://xxxx.supabase.co)
   - **anon public key** (long string starting with eyJ...)

---

## Step 5 – Paste keys into both HTML files

Open `gallery.html` and `share.html` in a text editor.

Find these two lines near the top of the `<script>` section in BOTH files:

```js
const SUPABASE_URL = 'YOUR_SUPABASE_URL';
const SUPABASE_KEY = 'YOUR_SUPABASE_ANON_KEY';
```

Replace with your actual keys:

```js
const SUPABASE_URL = 'https://xxxx.supabase.co';
const SUPABASE_KEY = 'eyJhbGc...your actual key...';
```

---

## Step 6 – Change the admin password

In `gallery.html`, find:
```js
const ADMIN_PASS = 'AandI2026';
```
Change `AandI2026` to whatever password you want.

---

## Step 7 – Upload files to GitHub

1. Go to your GitHub repo: **github.com/arnoldsegine/Our-Wedding-Invite**
2. Click **Add file** → **Upload files**
3. Drag both `gallery.html` and `share.html` into the upload box
4. Click **Commit changes**
5. Vercel will automatically deploy within 1-2 minutes

---

## Step 8 – Test it!

- Visit: `https://www.arnoldandisaacsoinlove.com/gallery.html`
- The QR code on that page links to: `https://www.arnoldandisaacsoinlove.com/share.html`
- Scan it with your phone to test the guest upload experience

---

## On the wedding day

1. Open `gallery.html` on your laptop/tablet
2. Screenshot or print the QR code — put it on tables, signs, screens
3. Guests scan → land on `share.html` → upload photos & videos instantly
4. Everything shows in the gallery in real time
5. After the wedding: go to `gallery.html`, click the small ⚙ gear in the bottom left corner, enter your password → **Download All**

---

## Music
Your music from `music.mp3` plays from `index.html`.
The gallery pages are separate HTML files so the music will pause when guests
navigate to them. If you want music on the gallery pages too, add this line
just before the `</body>` tag in both files:

```html
<audio autoplay loop style="display:none">
  <source src="music.mp3" type="audio/mpeg">
</audio>
```
