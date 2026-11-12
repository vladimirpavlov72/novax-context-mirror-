# Files NOVAX — instruction video script (EN, v1)

**Author:** Рома-маркетинг (EN adaptation of RU v1 by Р-4, marketing/video_files_scenario_ru.md)
**Frame:** 60–90 sec, 16:9, EN (en-US UI). Audience: clear to a kid and a grandparent.
**Canon:** no prices/trial/promises, informal "you", real UI only, only functions
that are in the build. CTA: "Get the app — link in the description."
**Name in titles:** "Files NOVAX" (name first). Label under the icon — "Files".
**Translation note:** live adaptation, not word-for-word. EN UI strings follow
app_en.arb (canon, 99 keys) — titles and voiceover use the exact on-screen wording.

---

## Script table (seconds | on screen | title slide | voice)

| Sec | On screen | Title (slide) | Voice |
|---|---|---|---|
| 0–4 | Dark intro, Files NOVAX icon large | "Tidy up your files" | Your phone is a mess and you can't find a thing? Here's how to sort it out in a couple of minutes. |
| 4–8 | Intro → second statement | "Everything visible. All under control" | Nothing hidden: the app shows you every file before anything gets deleted — you decide. |
| 8–12 | Real screen: opening Files NOVAX, "Home" tab | "Step 1. Open Files NOVAX" | Open Files NOVAX. This is the home screen. |
| 12–22 | Access card → "Why we need access" screen (3 items) → system permission dialog | "Step 2. Grant access — or pick a folder" | To see your files, the app asks for access. You can allow access to all files — or tap "Select folder" and give access to just one, say, Camera. Your call. Everything stays on the phone — nothing is uploaded anywhere. |
| 22–32 | Tap "Scan storage" → live progress → summary "12,061 files · 115.4 GB" → categories with figures | "Step 3. Tap "Scan storage"" | Tap "Scan storage". The app finds every file itself — photos, videos, music, documents — and sorts them onto shelves. You can see exactly what takes up the space. |
| 32–42 | Tap "Images" → file list, sorted by size (size · date) | "Step 4. Open a category" | Open "Images" — a list of every photo, biggest first. No mystery "junk" folders — you see each file, its size and its date. |
| 42–50 | "Folders" → tree (Download → folder → files), aggregates "N folders · M files" | "Step 5. Browse folders" | The "Folders" tab is a plain file explorer: open a folder, see what's inside and how much space it takes. |
| 50–62 | Long-press a file → "5 selected" → action bar → tap "Move to trash" → Trash: files "from {path}" → "Restore all" → "Move to trash" again → "Empty trash" → dialog "Delete forever?" → "Trash is empty" | "Step 6. Trash: delete — then change your mind" | Deleted files don't vanish right away — they go to the Trash. Changed your mind? Tap "Restore" and they go right back. "Empty trash" asks you first, and only then deletes for good. |
| 62–70 | "More" → "Operations log": list of operations with dates | "Log: everything on record" | The "Operations log" shows what was deleted or restored, and when. |
| 70–78 | "Select folder" screen → system folder picker → "Selected folders" section | "Step 7. Work via a selected folder" | Don't want to grant full access? Tap "Select folder", point to one folder — and work only with it. You can revoke the access in your phone settings at any time. |
| 78–86 | Quick montage: categories → explorer → trash → log (3–4 shots, 1–2 sec each) | "Photos · Video · Music · Documents. Explorer. Trash. Log" | Files NOVAX: every file in plain sight, deletion only through the trash and only with your say-so. Works offline. |
| 86–90 | Final frame: icon + name "Files NOVAX" | "Files NOVAX" | Get the app — link in the description. |

---

## Shooting notes for Р-3 / editor

1. **Test data:** shoot with safe files only (a folder of test images/documents),
   no personal photos, names, notifications or accounts. Swipe the notification
   shade away before recording.
2. **English UI:** shoot on the EN build, system locale **en-US**. Buttons on
   screen (exact strings from app_en.arb): "Scan storage", "Images", "Folders",
   "Move to trash", "Restore all", "Empty trash", "Delete forever?",
   "Operations log", "Select folder", "Trash is empty".
3. **Access screen:** the system permission dialog shows once (first launch).
   If already granted on the device — shoot the "Why we need access" rationale
   screen + the "Full access is enabled" card.
4. **Summary figures** ("12,061 files · 115.4 GB") depend on the test device —
   record the real ones, do not overlay graphics.
5. **Action bar** with 5 files selected shows: Copy / Move / Move to trash
   (Rename / Share — when a single file is selected). EN build after Р-4's
   panel fix: one shared label scale, no per-label shrink.
6. **Voiceover:** calm, no hype. Titles short — 2–5 words each.
7. **Do not show:** paywall/purchases, brand shelf, system notifications, real
   user data.
8. **EN gaps from R-3 inventory (08.11):** system permission dialog (en-US),
   SAF picker (en-US), "Selected folders" section — shoot these in the same
   session; Р-3 flagged them as missing from the raw set.

## Fact-check snapshot (which build the wording was verified against)

- EN strings: C:\dev\apps\novax-files\lib\l10n\app_en.arb (canon, 99 keys) —
  Scan storage / Images / Folders / Move to trash / Trash / Restore all /
  Empty trash / Delete forever? / Operations log / Select folder /
  Selected folders / Why we need access / Full access is enabled.
- Verified 08.11 against keys above; UI panel action labels match the Р-4
  b2-fix build (HEAD ebfcb62, single-scale panel labels, merged 08.11).
- Name canon: "Files NOVAX" untranslated; label under icon — "Files".
- No prices, no trial, no in-app purchase mentions (paid download — price is
  set in the console, never in texts).
