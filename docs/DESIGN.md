### 1. Color Palette (Extracted from Stitch Prototype & Tuned for Outdoor Sunlight)

| Token Name | Hex Code | Usage across RythuVaani Screens |
| :--- | :--- | :--- |
| `Primary Agrarian Green` | `#1B5E20` | Primary brand headers, active buttons, verified badges, selected tab icons. |
| `Vibrant Leaf Accent` | `#2E7D32` | Secondary highlights, positive price trend (`+₹350 / 1.6%`), checkmarks. |
| `Soft Sage Surface` | `#E8F5E9` | Selected card backgrounds (Language selection, Step 2 Scheme cards, Quick Tool icons). |
| `Warm Alabaster Canvas` | `#F7F9F5` | Main app background (reduces harsh white glare in bright midday sun). |
| `Pure Card White` | `#FFFFFF` | Elevated cards with `1px solid #E2E8F0` border for crisp visual separation. |
| `Warning Amber / Gold` | `#FFF8E1` / `#D97706` | Farmer Advice banners, MAO Preliminary AI Disclaimer card, Weather sun icons. |
| `Urgent Alert Red` | `#FEF2F2` / `#DC2626` | Heavy Rain Alert (`70% on Day After Tomorrow`), voice recording timer indicator. |
| `High-Contrast Ink` | `#0F172A` | Primary headings, Modal Price (`₹22,400`), Temperature (`32°C`). |
| `Muted Slate` | `#475569` | Secondary bilingual subtitles and timestamps. |

### 2. Bilingual Typography & Script Rendering Rules
* **Telugu Font Family:** **`Noto Sans Telugu`** (Weights: `400 Regular`, `600 SemiBold`, `700 Bold`).
* **English/Numeric Font Family:** **`Plus Jakarta Sans`** or **`Inter`** (Tabular numerals enabled `font-variant-numeric: tabular-nums` for Mandi prices and weather metrics).
* **Critical Telugu Line-Height Rule:** Telugu vattulu (subscript consonants) and matras clip if standard Latin line heights (`1.2`) are used. Every text component rendering Telugu **must** enforce `lineHeight >= 1.55 * fontSize` and `includeFontPadding: true` on Android.
* **Dual-Label Pattern:** Primary actions and section headers pair Telugu and English side-by-side (e.g., `త్వరిత సేవలు / Quick Tools`, `మాట్లాడండి / Recording Voice...`) to build confidence for semi-literate users.

### 3. Ergonomics & Accessibility Standards
* **Touch Targets:** Minimum `52dp x 52dp` for all buttons (accommodates rough/calloused or wet fingers in the field).
* **Audio-First Affordance:** Every information-dense screen (Login, Home, Schemes, Mandi, Weather) features a dedicated **`వినండి / Listen`** audio pill at the top or bottom so a farmer can consume the entire screen without reading a single paragraph.
* **Persistent 5-Tab Bottom Navigation:**
  1. `Home` (`హోమ్` - Home icon)
  2. `Ask AI` (`AI ని అడగండి` - Microphone icon)
  3. `Schemes` (`పథకాలు` - Document icon)
  4. `Market` (`మార్కెట్` - Storefront icon)
  5. `Profile` (`ప్రొఫైల్` - Person icon)

---