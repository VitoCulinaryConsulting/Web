# Web
Helping restaurants grow through intelligent menu design

- Each recipe has its own folder named ID-slug/.
- Keep images/videos inside that folder for portability. (Alternatively, use the shared media/ root and keep only relative paths in Markdown.)

# Static Recipe Catalog — Folders, Naming & Templates

A simple, beginner‑friendly way to **catalog** recipes as plain files (text/Markdown/PDF), plus photos and videos. This structure can later be imported into a database or the React UI.

---

## 1) Folder structure

```
recipes/
  index.csv                      # master index for quick overview/spreadsheet work
  index.json                     # same data as JSON (optional)
  media/                         # all media in one place (or keep per recipe)
    images/
    videos/
  RCP-0001-crepes-butter/
    RCP-0001.md                  # canonical Markdown source
    RCP-0001.pdf                 # exported PDF (optional)
    images/
      RCP-0001-hero.jpg
      RCP-0001-step-01.jpg
      RCP-0001-step-02.jpg
    videos/
      RCP-0001-hero.mp4
  RCP-0002-rabbit-a-la-moutarde/
    RCP-0002.md
    images/
      RCP-0002-hero.jpg
```

**Why?**

* Each recipe has its own folder named `ID-slug/`.
* Keep images/videos inside that folder for portability. (Alternatively, use the shared `media/` root and keep only relative paths in Markdown.)

---

## 2) Naming conventions

* **ID**: `RCP-XXXX` (zero‑padded). Example: `RCP-0001`.
* **Folder**: `RCP-0001-crepes-butter` → *ID + short kebab‑case title*.
* **Files**:

  * Markdown: `RCP-0001.md`
  * PDF export: `RCP-0001.pdf`
  * Images: `RCP-0001-hero.jpg`, `RCP-0001-step-01.jpg`
  * Videos: `RCP-0001-hero.mp4`, `RCP-0001-howto.mp4`
* **Image sizes**: keep original + an optional web version (e.g., `RCP-0001-hero-1600.jpg`).

---

## 3) Minimal data model (schema)

Simple so it’s easy to maintain by hand. can grow later.

**Core fields**

* `id` (string) — unique recipe code e.g., `RCP-0001`
* `title` (string) — English title
* `description` (string) — short paragraph
* `cuisine` (string) — e.g., French, Polish, Spanish
* `tags` (array of strings) — e.g., `quick`, `breakfast`, `gluten-free`
* `difficulty` (enum) — `easy | medium | hard`
* `prep_minutes` (number)
* `cook_minutes` (number)
* `servings` (number)
* `calories_per_serving` (number, optional)
* `ingredients` (array) of `{ item, amount, unit, notes? }`
* `steps` (ordered list of strings)
* `equipment` (array of strings)
* `hero_image` (path string)
* `gallery_images` (array of paths)
* `videos` (array of paths)
* `source_url` (string, optional)
* `author` (string, optional)
* `created_at` (ISO date), `updated_at` (ISO date)

These fields map 1:1 to the React component later, and are friendly to CSV/JSON.

---

## 4) Markdown recipe template (canonical source)

Save as `recipes/RCP-0001-slug/RCP-0001.md`.

```markdown
---
id: RCP-0001
title: Butter Crêpes
slug: crepes-butter
description: Thin, elastic French crêpes – a base for sweet or savory fillings.
cuisine: French
tags: [quick, breakfast, base]
difficulty: easy
prep_minutes: 10
cook_minutes: 15
servings: 2
calories_per_serving: 320
hero_image: ./images/RCP-0001-hero.jpg
# You can use relative paths inside the recipe folder
gallery_images:
  - ./images/RCP-0001-step-01.jpg
  - ./images/RCP-0001-step-02.jpg
videos:
  - ./videos/RCP-0001-hero.mp4
source_url: https://example.com/original
author: Vito
created_at: 2025-10-16
updated_at: 2025-10-16
---

## Ingredients

- 120 g wheat flour (type 450), sifted
- 220 ml milk
- 2 pcs egg
- 20 g clarified butter, melted
- 1/4 tsp salt

## Steps

1. Whisk flour and salt. Add eggs, then gradually pour in milk, mixing until smooth.
2. Stir in melted butter. Rest 10 minutes for gluten to relax.
3. Heat a pan. Spread a thin layer of batter; cook ~45–60 s each side.

## Equipment

- Bowl
- Whisk
- 24 cm pan

## Notes

- For web, also prepare a 1600px wide hero image for fast loading.
```

> The **YAML front matter** (between `---`) is your structured metadata. Everything after is human‑readable content.

---

## 5) PDF export

* Export the Markdown to PDF using any editor or a CLI (e.g., Pandoc). Keep the same ID as filename.
* PDFs are great for sharing/printing. The Markdown remains your **master source**.

---

## 6) Spreadsheet (index.csv) — quick overview

You can maintain a spreadsheet and later export as CSV. Suggested columns:

```
id,title,slug,cuisine,tags,difficulty,prep_minutes,cook_minutes,servings,hero_image,gallery_images,videos,author,created_at,updated_at
RCP-0001,Butter Crêpes,crepes-butter,French,"quick|breakfast|base",easy,10,15,2,./RCP-0001-crepes-butter/images/RCP-0001-hero.jpg,"./RCP-0001-crepes-butter/images/RCP-0001-step-01.jpg|./RCP-0001-crepes-butter/images/RCP-0001-step-02.jpg","./RCP-0001-crepes-butter/videos/RCP-0001-hero.mp4",Vito,2025-10-16,2025-10-16
```

* Use `|` inside a cell to separate multiple paths for images/videos.
* This CSV can be imported into any database later.

---

## 7) Optional: JSON index (index.json)

Create a machine‑friendly overview for scripts or future import:

```json
[
  {
    "id": "RCP-0001",
    "title": "Butter Crêpes",
    "slug": "crepes-butter",
    "cuisine": "French",
    "tags": ["quick", "breakfast", "base"],
    "difficulty": "easy",
    "prep_minutes": 10,
    "cook_minutes": 15,
    "servings": 2,
    "hero_image": "./RCP-0001-crepes-butter/images/RCP-0001-hero.jpg",
    "gallery_images": [
      "./RCP-0001-crepes-butter/images/RCP-0001-step-01.jpg",
      "./RCP-0001-crepes-butter/images/RCP-0001-step-02.jpg"
    ],
    "videos": ["./RCP-0001-crepes-butter/videos/RCP-0001-hero.mp4"],
    "author": "Vito",
    "created_at": "2025-10-16",
    "updated_at": "2025-10-16"
  }
]
```

---

## 8) Media tips (photos & video)

* **Photos**: keep original JPEG/PNG + a web version (e.g., 1600px). Name with the recipe ID.
* **Alt text**: when you build the site, use concise English alt text, e.g., `"Butter crêpes stacked on a plate"`.
* **Video**: MP4 (H.264) for broad compatibility. Keep clips short; later you can generate thumbnails named `RCP-0001-hero-thumb.jpg`.
* **Copyright**: keep your own images, or store license info in a sidecar text file `RCP-0001-license.txt` if needed.

---

## 9) Growing later

* Add nutrition fields (protein, carbs, fat), allergens, or `source_url`.
* Add **schema.org/Recipe** when you migrate to the website — your YAML maps cleanly to that standard.
* Keep everything in **English** from the start, so later UI is consistent.

---

## 10) Quick start checklist

* [ ] Create `recipes/` folder
* [ ] Copy this Markdown template for your first recipe
* [ ] Add photos/videos using the naming rules
* [ ] Update `index.csv` (or `index.json`)
* [ ] Export the Markdown to PDF if you want a shareable version

---

**Need help?** Paste me one finished Markdown file and your folder tree, and I’ll sanity‑check and align it to the schema.
