<div align="center">

# Dive Physics Study

**A bilingual (Hebrew / English) study and exam-prep web app for the AAUS Scientific Diver course: diving physics, physiology and accidents.**

Built for divers training at the Interuniversity Institute for Marine Sciences in Eilat (IUI).

`Single HTML file` · `No build step` · `Works offline after first load` · `Mobile + desktop` · `Light / dark`

[עברית ⬇](#עברית)

</div>

---

## Contents

- [Features](#features)
- [Getting started](#getting-started)
- [Deploy to GitHub Pages](#deploy-to-github-pages)
- [Project structure](#project-structure)
- [How it works](#how-it-works)
- [Editing content](#editing-content)
- [Accuracy notes](#accuracy-notes)
- [Disclaimer](#disclaimer)
- [Credits](#credits)

---

## Features

The app has four modes, switched from the top bar on desktop or the bottom tab bar on phones.

### 📖 Study
- 12 chapters that follow the lecture: respiration and circulation, properties of water, Archimedes, pressure, Boyle, temperature laws, Dalton, Henry and DCS, Haldane, additional concerns, emergencies, CPR and rescue.
- Each chapter has key points, formulas, numbers to know, accident cards (serious ones flagged in red) and step-by-step first aid.
- An interactive **depth gauge** (Boyle and Dalton chapters) shows ambient pressure, relative gas volume, PO₂ and PN₂ from 0 to 40 m.
- "Mark as learned" with a progress bar. Progress is saved in the browser.

### 📋 Cheat sheet
- Every chapter condensed onto one page, plus reference tables:
  - law → accident
  - all gas-law formulas
  - telling diving collapses apart
  - DCS types
  - oxygen exposure limits
  - the 12-step field neuro exam
- **Live search** filters and highlights matching lines, for example `AGE` or `1.4`.

### 🧮 Calculators (14)
Every calculator shows the working with your numbers, so you can check it by hand.

| # | Calculator | Based on |
|---|---|---|
| 1 | Free air in a cylinder | P × V |
| 2 | Gas volume at depth | Boyle |
| 3 | Lift bag: air needed | Archimedes + Boyle |
| 4 | Dive time: air vs nitrox (21–40%), with NDL, gas time, MOD, EAD | Dalton + Bühlmann ZH-L16C |
| 5 | Surface interval between dives + no-fly guidance | Bühlmann ZH-L16C, DAN |
| 6 | Cylinder pressure over a multi-level dive | SAC × ATA |
| 7 | Cylinder pressure vs temperature | Gay-Lussac (absolute) |
| 8 | Underwater distortion (size and distance) | Refraction, 4/3 |
| 9 | SAC rate from a logged dive | Boyle |
| 10 | Absolute vs gauge pressure, all units | Pressure principles |
| 11 | Weighting: sea ↔ fresh water | Archimedes |
| 12 | Partial pressures and best nitrox mix | Dalton |
| 13 | Oxygen exposure (CNS clock) | NOAA teaching table |
| 14 | Haldane tissue compartments | Half-time equation |

### ✅ Exam
- About 80 multiple-choice questions written from the lecture, in both languages.
- Pick topics and question count (10 / 20 / 30 / all). Answer order is shuffled each time.
- Instant feedback with an explanation after every question.
- Results: score ring, per-topic breakdown, review of mistakes, "retry mistakes" and best score.
- Keyboard: `1`–`4` to answer, `Enter` for the next question.

### General
- **Hebrew ⇄ English** with a full RTL layout, translated content, questions and UI.
- **Light / dark** theme. It follows the system setting until you choose one yourself.
- **Responsive:** sidebar layout on desktop, chips and a bottom tab bar on phones. Safe-area aware, no zoom-on-focus, reduced-motion support.
- **IUI branding:** the Hebrew or English logo follows the language, with a recolored version for dark mode.

---

## Getting started

There's nothing to install. Choose one of these:

```bash
# Option 1: open the file directly
open index.html          # macOS
start index.html         # Windows
xdg-open index.html      # Linux
```

```bash
# Option 2: serve locally (recommended)
python3 -m http.server 8000
# then visit http://localhost:8000
```

An internet connection is only needed the first time, to load Google Fonts. Everything else, including the logos, is embedded in the file.

---

## Deploy to GitHub Pages

1. Create a repository and add `index.html` and `README.md`.
2. Push to `main`:
   ```bash
   git init
   git add index.html README.md
   git commit -m "Dive Physics Study app"
   git branch -M main
   git remote add origin https://github.com/<user>/<repo>.git
   git push -u origin main
   ```
3. In the repo, go to **Settings → Pages → Build and deployment**, set the source to `main` and the folder to `/ (root)`.
4. The site goes live at `https://<user>.github.io/<repo>/`.

---

## Project structure

```
.
├── index.html   # the whole app: markup, styles, data, logic
└── README.md
```

Inside `index.html`, the `<script>` is organized in blocks:

| Block | What it holds |
|---|---|
| `LOGO` | Base64 PNG logos (`he`, `en`, plus `_d` dark variants) |
| `UI` | Interface strings, `en` / `he` |
| `CH` | The 12 study chapters (points, formulas, numbers, accidents, first aid) |
| `Q` | Exam questions. The first option is always the correct one, and options are shuffled at runtime |
| `LAW`, `O2T`, `DCST`, `NEURO`, `COLL`, `CORE` | Cheat-sheet tables |
| `SHEET` | Condensed cheat-sheet lines per chapter (a leading `!` marks a red-flag line) |
| `CU`, `CDEF` | Calculator strings and default inputs |
| `ZH` | Bühlmann ZH-L16C N₂ half-times and a/b coefficients |
| `render*()` | One renderer per mode: `renderStudy`, `renderSheetView`, `renderCalc`, `renderExam` |

---

## How it works

- **Vanilla JS**, with no framework and no dependencies. The only external request is Google Fonts (IBM Plex Sans, IBM Plex Sans Hebrew, IBM Plex Mono, Secular One).
- **Theming** uses CSS custom properties, with `prefers-color-scheme` plus a `data-theme` override.
- **i18n**: every content string is `{en, he}`. Switching language sets `dir="rtl"` / `ltr` and re-renders.
- **Persistence**: `localStorage` (with a `dive.` prefix) keeps language, theme, current chapter, learned chapters, best score and calculator inputs. Every access is wrapped in `try/catch`, so the app works even when storage is blocked.
- **Decompression model**: Bühlmann ZH-L16C, with 16 Haldane compartments, N₂ only, instant descent and ascent, seawater, and a selectable gradient factor (100 / 90 / 80).

---

## Editing content

**Add a question.** Append a line to the `Q` block. Put the correct answer first; it gets shuffled at runtime.

```js
q("boyle",
  ["Question?", "Correct", "Wrong 1", "Wrong 2", "Wrong 3", "Explanation."],
  ["שאלה?",     "נכון",    "שגוי 1",  "שגוי 2",  "שגוי 3",  "הסבר."]);
```

**Edit a chapter.** Find its object in `CH` by `id` (`phys`, `water`, `arch`, `press`, `boyle`, `temp`, `dalton`, `henry`, `haldane`, `other`, `emerg`, `cpr`). Every text field uses `L("English", "עברית")`.

---

## Accuracy notes

- **Slide 18 (buoyancy example).** A 20 kg crate displacing 25 L of seawater gives 20 − 25.75 = −5.75 kg. Physically it **floats**, because the displaced water outweighs it. The slide says it sinks. The app flags this, and no exam question depends on it.
- **NDL values** come from the Bühlmann model, so they differ slightly from PADI RDP or US Navy tables.
- **CNS limits** are interpolated linearly between the teaching-table values (1.6/45 … 0.6/720 min).

---

## Disclaimer

> ⚠️ **For study only.** This app is a learning aid for an exam. **Never use it to plan a real dive.** Follow your program's diving safety manual, approved tables or dive computer, and your Diving Safety Officer. Figures are teaching values and may not reflect current agency standards.

---

## Credits

- Course content: *AAUS Scientific Diver Training: Diving Physics, Physiology & Accidents* lecture deck.
- Sources cited in the deck: 2024 AHA/AAP Focused Update on Resuscitation Following Drowning, OSHA 29 CFR 1910 Subpart T, DAN, NOAA Diving Manual, U.S. Navy Diving Manual, AAUS Standards for Scientific Diving, NOAA Fisheries.
- Logos © The Interuniversity Institute for Marine Sciences in Eilat (IUI), used for institutional study purposes. They are not covered by this repository's license.

---

<div dir="rtl" align="right">

## עברית

**אפליקציית לימוד והכנה למבחן לקורס צוללן מדעי AAUS: פיזיקה, פיזיולוגיה ותאונות צלילה.** האפליקציה נבנתה עבור צוללנים בהכשרה במכון הבינאוניברסיטאי למדעי הים באילת.

**ארבעה מצבים:**
- **לימוד:** 12 פרקים לפי ההרצאה, מד עומק אינטראקטיבי ומעקב התקדמות.
- **צ׳יט שיט:** כל החומר בעמוד אחד, עם טבלאות עזר וחיפוש.
- **חישובים:** 14 מחשבונים, כולל אוויר במיכל, בויל, מצנח הרמה, ניטרוקס ו-NDL, מרווח שטח, לחץ וטמפרטורה, עיוות ראייה, SAC, משקולות, דלטון, שעון CNS ותאי הולדיין.
- **מבחן:** כ-80 שאלות אמריקאיות, הסבר אחרי כל שאלה, וסיכום לפי נושא.

**הרצה:** פותחים את `index.html` בדפדפן. אין צורך בהתקנה.

**פרסום ב-GitHub Pages:** ב-Settings ← Pages בוחרים את הענף `main` ואת התיקייה `/ (root)`.

> ⚠️ **כלי לימוד בלבד.** אין לתכנן איתו צלילה אמיתית. יש לפעול לפי מדריך הבטיחות של התוכנית, הטבלאות או מחשב הצלילה המאושרים, וקצין בטיחות הצלילה.

</div>
