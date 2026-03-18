<div align="center">

<br/>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:0f0f0f,100:c8441a&height=200&section=header&text=PixTrove&fontSize=80&fontColor=ffffff&fontAlignY=38&desc=AI-Powered%20Event%20Photo%20Organizer&descAlignY=58&descColor=ffffff&descSize=18&animation=fadeIn" width="100%"/>

</div>

<br/>

<div align="center">

```
  upload 500 photos  →  walk away  →  come back to sorted folders
```

</div>

<br/>

<div align="center">

![Python](https://img.shields.io/badge/-Python%203.8+-black?style=flat-square&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/-Flask-black?style=flat-square&logo=flask&logoColor=white)
![Next.js](https://img.shields.io/badge/-Next.js-black?style=flat-square&logo=next.js&logoColor=white)
![OpenCV](https://img.shields.io/badge/-OpenCV-black?style=flat-square&logo=opencv&logoColor=white)
![Tailwind](https://img.shields.io/badge/-Tailwind-black?style=flat-square&logo=tailwindcss&logoColor=white)
![MIT](https://img.shields.io/badge/-MIT%20License-black?style=flat-square)

</div>

---

<br/>

## `$ what_is_this`

> You shot 600 photos at a college fest. Now someone asks *"can you send me just the ones with Priya?"*  
> Without PixTrove: 2 hours of squinting at thumbnails.  
> With PixTrove: 8 seconds.

PixTrove uses a **Flask backend + face_recognition (Dlib)** to scan every photo, match faces against reference headshots, and drop each photo into a named folder automatically. Unmatched photos stay in the upload area for a quick manual pass.

<br/>

---

## `$ how_it_works`

```
┌─────────────────────────────────────────────────────────────────────┐
│                                                                     │
│   📂 reference_photos/          📸 Event Gallery (bulk upload)     │
│      John_Doe.jpg   ──────┐         500 photos from the fest       │
│      Jane_Smith.png ──────┤                    │                    │
│      Rahul_K.jpg    ──────┤                    │                    │
│                           ▼                    ▼                    │
│                   ┌───────────────────────────────┐                 │
│                   │        Flask Backend           │                 │
│                   │   face_recognition + OpenCV    │                 │
│                   │   detect → encode → compare    │                 │
│                   └──────────────┬────────────────┘                 │
│                                  │                                  │
│              ┌───────────────────┼──────────────────┐               │
│              ▼                   ▼                  ▼               │
│       ✅ match found       ✅ match found      ❌ no match          │
│   organised_photos/    organised_photos/      stays in              │
│      John_Doe/            Jane_Smith/          uploads/             │
│                                               (manual review)       │
└─────────────────────────────────────────────────────────────────────┘
```

<br/>

---

## `$ tech_stack`

<br/>

<table>
<tr>
<td valign="top" width="50%">

**`// backend`**
```python
framework  = "Flask"
ai_engine  = "face_recognition"
under_hood = "Dlib"
vision     = "OpenCV (cv2)"
language   = "Python 3.8+"
```

</td>
<td valign="top" width="50%">

**`// frontend`**
```javascript
framework = "Next.js (React)"
styling   = "Tailwind CSS"
upload_ui = "Drag & Drop"
port      = 3000
```

</td>
</tr>
</table>

<br/>

---

## `$ install`

**Prerequisites** → `Python 3.8+` &nbsp; `Node.js LTS` &nbsp; `Git`

<br/>

**Step 1 — Clone**
```bash
git clone <repository_url>
cd PixTrove
```

**Step 2 — Backend**
```bash
python -m venv env
source env/bin/activate        # Windows: .\env\Scripts\activate

pip install -r requirements.txt
```

> ⚠️ If `requirements.txt` shows encoding errors — open it in VS Code → Save with Encoding → UTF-8

**Step 3 — Frontend**
```bash
npm install
```

**Step 4 — Directories** *(auto-created by Flask on startup, but you can do it manually)*
```bash
mkdir reference_photos uploads organised_photos
```

**Step 5 — Run** *(two terminals)*

| Terminal | Command | URL |
|---|---|---|
| A — Backend  | `python app.py` | `http://127.0.0.1:5000` |
| B — Frontend | `npm run dev`   | `http://localhost:3000`  |

<br/>

---

## `$ api_reference`

<br/>

**`POST /upload`** — Upload a photo for face recognition

```
Request  →  multipart/form-data  { file: image.jpg }
```

```jsonc
// ✅ Face matched
{
  "message":      "File moved to organised_photos/JohnDoe",
  "matched_with": "JohnDoe.jpg",
  "preview_url":  "/preview/JohnDoe.jpg"
}

// ℹ️ No match
{ "message": "No matching faces found." }

// ℹ️ No face in image
{ "message": "No faces found in the uploaded image." }

// ❌ Bad request
{ "error": "Invalid file type" }   // HTTP 400
```

<br/>

**`GET /preview/<filename>`** — Fetch a reference image

```
/preview/John_Doe.jpg   →  returns the image file
/preview/nobody.jpg     →  { "error": "Image not found" }  // 404
```

<br/>

---

## `$ file_tree`

```
PixTrove/
│
├── app.py                 ← Flask backend (face recognition + API)
├── requirements.txt       ← Python dependencies
│
├── reference_photos/      ← 👤 PUT HEADSHOTS HERE (name = folder name)
│   ├── John_Doe.jpg
│   └── Jane_Smith.png
│
├── uploads/               ← 📥 temp store (auto-cleared after matching)
│
├── organised_photos/      ← 📂 final sorted output
│   ├── John_Doe/
│   │   ├── event_photo_003.jpg
│   │   └── event_photo_071.jpg
│   └── Jane_Smith/
│       └── event_photo_019.jpg
│
└── app/                   ← Next.js frontend
    ├── components/
    │   └── Previews.js    ← drag-and-drop upload UI
    └── pages/
```

<br/>

---

## `$ contributing`

```bash
git checkout -b feature/your-idea
# make changes
git commit -m "feat: your idea"
git push origin feature/your-idea
# open a pull request
```

PRs are welcome. If you find a bug or want to add a feature, open an issue first so we can discuss it.

<br/>

---

<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:c8441a,100:0f0f0f&height=120&section=footer" width="100%"/>

**Built with Python, obsession, and a lot of test photos.**

`face_recognition` · `OpenCV` · `Flask` · `Next.js` · `Tailwind CSS`

</div>
