### 1. Executive Summary
* **Product Name:** RythuVaani (రైతువాణి AI) – AI Smart Agronomy Companion
* **Target Region:** Andhra Pradesh & Telangana (Initial Hub: Guntur, Krishna, Amaravati Zone)
* **Supported Languages:** Telugu (`తెలుగు` - Primary), English (`English`), Hindi (`हिन्दी`) with 1-Tap Instant Switching.
* **Core Mission:** Empower smallholder and Pattadar farmers with voice-first, scientifically verified crop diagnosis, hyper-local Mandal agromet weather alerts, live e-NAM APMC Mandi rates, and zero-PII government scheme eligibility checks.
* **Institutional Data Alignment:** Grounded in Digital Agriculture Mission standards, ANGRAU (Acharya N.G. Ranga Agricultural University) agronomy datasets, IMD Agromet advisories, and e-NAM market feeds.

### 2. Target User Personas
1. **Venkata Reddy Garu (Primary Persona – Small/Marginal Pattadar Farmer):**
   * **Location:** Guntur Mandal, Andhra Pradesh.
   * **Landholding:** 4.5 Acres (Self-owned Patta land).
   * **Crops Cultivated:** Paddy (*BPT 5204 / Samba Mahsuri*) and Chilli (*Guntur Teja Grade A*).
   * **Behavior:** Prefers speaking in Telugu over typing; checks weather before spraying costly pesticides; sells produce at Guntur Mirchi Yard; wants to track *Rythu Bharosa / PM-KISAN* installments (`₹13,500/yr`).
2. **Ramesh Naik (Tenant / Marginal Farmer):**
   * **Need:** Uses **Guest Mode** or **Voice Login** without complex registration; relies heavily on the **Audio Guides (`వినండి`)** due to limited text literacy.

### 3. Detailed Functional Requirements (Mapped to Stitch Prototype Screens)

#### FR-01: Onboarding, Language Selection & Authentication (Screen 1)
* **FR-01.1 (KVK Context Header):** Display regional context (`AP/Telangana • Krishi Vigyan Kendra`) and a prominent **`Listen Guide` (`వినండి`)** button that plays a 20-second Telugu/English voice walkthrough of the login screen.
* **FR-01.2 (1-Tap Language Switcher):** Present three large selectable cards for **Telugu (`తెలుగు`)**, **English**, and **Hindi (`हिन्दी`)**. Tapping any card must update 100% of UI strings and audio voices within `<50ms` without restarting the app.
* **FR-01.3 (Multi-Modal Authentication):**
  1. **Standard OTP Login:** 10-digit Indian mobile number (`+91`) with 6-digit SMS OTP verification (auto-read via Android SMS Retriever API).
  2. **Voice Assistant Login:** Allows the farmer to speak their 10-digit phone number and spoken OTP in Telugu or English.
  3. **Guest Mode (`Explore Without Login`):** Grants immediate read-only access to Weather, Mandi Prices, and Scheme Checker without requiring a phone number.

#### FR-02: Smart Home Dashboard & Daily Audio Advisory (Screens 2 & 3)
* **FR-02.1 (Personalized Header & Greeting):** Show active Mandal/Market selector (`Guntur, AP • గుంటూరు మార్కెట్`), 1-tap `తె/En` toggle, notification bell, current season badge (`Kharif Season`), and personalized greeting (`నమస్కారం / Welcome Venkata Reddy Garu`).
* **FR-02.2 (Today's Audio Advisory Player):** Featured audio card (`ఈరోజు వ్యవసాయ సలహా`) displaying the day's top localized threat (e.g., *"Paddy stem borer alert & irrigation timing - 1:15 min"*) with 1-tap play/pause and offline caching.
* **FR-02.3 (Home Weather Snapshot):** Compact 4-column metric card showing **Temp (`32°C Sunny`)**, **Rain Probability (`15% వర్షం తక్కువ`)**, **Humidity (`68% తేమ`)**, and **Wind Speed (`14 km/h గాలి`)**, paired with an actionable **Farmer Advice Banner** (e.g., *"Suitable weather for fertilizer spraying on Paddy crops before 4:00 PM today"*).
* **FR-02.4 (AI-Powered Quick Tools 2x2 Grid):**
  1. **AI Camera – Crop Health Scanner (`పంట తెగుళ్ల గుర్తింపు`)**
  2. **Voice Assistant – Ask Agronomist AI (`సందేహాలు అడగండి`)**
  3. **Live Prices – Today's Mandi Rate (`మార్కెట్ ధరల సమాచారం`)**
  4. **Subsidies – Govt Schemes 2025 (`రైతు భరోసా & సబ్సిడీలు`)**

#### FR-03: Ask Agronomist AI Voice Assistant (Screen 4)
* **FR-03.1 (Mandatory Regulatory Disclaimer):** Persistently render the verified guidance banner at the top of the screen: *"Preliminary AI agronomist advisory. Always consult your local Mandal Agriculture Officer (MAO) for severe infections or seed blight."*
* **FR-03.2 (Quick Inquiry Chips):** Horizontal scrollable chips for common seasonal issues (`Paddy pest control`, `Chilli leaf curl virus`, `Fertilizer dosage`).
* **FR-03.3 (Live Bilingual Voice Recording & Editable Transcript):**
  * Support up to 60-second voice queries (`00:14 / 01:00`) with visual waveform feedback.
  * Display **real-time dual-script transcription** showing both the English translation and Telugu script simultaneously (e.g., *"How to control yellow stem borer in my BPT 5204 paddy crop in Guntur?"* / *"నా గుంటూరు బిపిటి 5204 వరి పంటలో పసుపు కాండం తొలుచు పురుగును ఎలా నియంత్రించాలి?"*).
  * Provide an **`Edit Transcript`** button, **`Cancel / Retry`**, and **`Stop & Ask AI`** CTA.

#### FR-04: Government Schemes & Zero-PII AI Eligibility Checker (Screen 5)
* **FR-04.1 (Scheme Search & Category Filters):** Voice-enabled search bar (`PM-KISAN, Rythu Bharosa...`) and filter pills (`All Schemes`, `Income Support`, `Crop Insurance`).
* **FR-04.2 (1-Minute Audio Guide):** Top audio banner (`పథకాల సహాయ ఆడియో గైడ్`) explaining how scheme eligibility works in simple Telugu.
* **FR-04.3 (3-Step Privacy-First Eligibility Wizard):**
  * Explicitly assure the farmer: *"Check qualifying government schemes in 3 simple questions without entering your Aadhaar, OTP, or bank details."*
  * **Step 1:** State, District & Farmer Category.
  * **Step 2 (Land & Farming Profile):** Select **Landholding** (`Small & Marginal < 2 Hectares / 5 Acres`), **Land Ownership** (`Patta Landholder / Self-Owned Pattadar` vs. Tenant), and **Crops Cultivated** (`Paddy & Chilli`).
  * **Step 3:** Instant match output showing qualifying schemes (`2 Schemes Likely Eligible`) with installment breakdowns and required document checklists.

#### FR-05: Live e-NAM Mandi Market Prices (Screen 6)
* **FR-05.1 (Hierarchical Yard Filter):** Filter by **State** (`Andhra Pradesh`), **District** (`Guntur`), and **APMC Yard** (`Guntur Mirchi Yard`), plus crop filter pills (`All Crops`, `Chilli`, `Paddy`, `Cotton`).
* **FR-05.2 (Live e-NAM Status & Commodity Card):**
  * Show market status (`Live Market Status: Open (E-NAM Linked) • 11:30 AM`).
  * Display variety & grade (`Guntur Teja Chilli • Grade A • Drying Lot`), **Modal Price (`₹22,400 / Quintal`)**, day-over-day delta (`+₹350 / +1.6% vs Yesterday`), **Min Price (`₹18,500`)**, **Max Price (`₹24,200`)**, **Arrivals (`42,500 Bags`)**, and **Traded Volume (`38,200 Bags`)**.
* **FR-05.3 (Daily Mandi Audio Bulletin):** Bottom sticky audio player (`1 Min Telugu Voice Report`) summarizing the day's price movements for farmers who cannot read numeric tables.

#### FR-06: Farmer Profile & Saved Advisories (Screen 7)
* **FR-06.1 (Agronomic Identity Card):** Display farmer name, masked phone, **Pattadar badge**, acreage (`4.5 Acres`), Mandal (`Guntur Mandal`), primary crop pills (`Chilli`, `Paddy`), and activity counters (`12 Questions Asked`, `5 Saved Items`, `Kharif '25`).
* **FR-06.2 (Saved Items & Question History Vault):** Tabbed view (`Saved`, `Questions`, `Settings`) with category filters (`All`, `Govt Schemes`, `Agronomy`) and offline access to bookmarked scheme cards (e.g., *YSR Rythu Bharosa / PM-KISAN Convergence — Next Installment ₹7,500 — 100% Eligible*).

#### FR-07: Hyper-Local Agromet Weather & Severe Alerts (Screen 8)
* **FR-07.1 (Mandal-Level Live Telemetry):** Display live feed timestamp (`10m ago • IMD & ANGRAU Live`) for specific Mandal hubs (`Tenali & Lam Mandal Hub`).
* **FR-07.2 (4-Parameter Agromet Grid):**
  1. **Rain Forecast (`వర్ష సూచన`):** Current (`15% Low Today`) + 48-hour lookahead (`70% on Day After Tomorrow`).
  2. **Relative Humidity (`తేమ శాతం`):** `68% RH (High moisture in air)`.
  3. **Wind Speed & Direction (`గాలి వేగం`):** `14 km/h SW` with explicit spray safety indicator (`Safe spray` when `<18 km/h`).
  4. **Soil Moisture at 10cm Depth (`నేల తేమ 10cm`):** `24% Volume (Adequate for Paddy)`.
* **FR-07.3 (Severe Weather Alert & Urgent Action Card):** High-visibility alert card for upcoming storms (e.g., `45–60mm rainfall in Guntur & Krishna districts on Thursday afternoon`) with specific Telugu agronomic mitigation steps (`తక్షణ చర్య / Urgent Action`).

### 4. Non-Functional Requirements (NFRs)
* **NFR-01 (Sunlight Readability):** Minimum contrast ratio of **7:1 (WCAG AAA)** on primary text and price metrics for outdoor field usage at 12:00 PM noon.
* **NFR-02 (Voice Pipeline Latency):** End-to-end voice query to Telugu response must complete in **`<2.5 seconds`** on a standard 4G VoLTE connection.
* **NFR-03 (Offline Resilience):** Home dashboard, last-fetched weather/mandi rates, saved schemes, and the 3-step scheme eligibility checker must function **100% offline**.
* **NFR-04 (App Size):** Android production AAB download size must be **`<25 MB`**.

---