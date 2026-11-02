### 1. High-Level System Topology

```
+-----------------------------------------------------------------------------------+
|                        RYTHUVAANI MOBILE CLIENT (React Native / Expo)             |
|  +-----------------+  +------------------+  +-----------------+  +-------------+  |
|  | Bilingual UI    |  | Voice Recorder & |  | LiteRT Edge     |  | Offline DB  |  |
|  | (Zustand + i18n)|  | Audio Player     |  | Leaf Detector   |  | (SQLite/MMKV|  |
|  +--------+--------+  +--------+---------+  +--------+--------+  +------+------+  |
+-----------|--------------------|---------------------|------------------|---------+
            | HTTPS / REST       | WSS / Audio Stream  | Multipart WebP   | Sync
            v                    v                     v                  v
+-----------------------------------------------------------------------------------+
|                        API GATEWAY & LOAD BALANCER (NGINX / Cloud Run)            |
+-----------------------------------------------------------------------------------+
                                         |
                                         v
+-----------------------------------------------------------------------------------+
|                        CORE BACKEND ORCHESTRATOR (Python FastAPI)                 |
|  +--------------------+  +-----------------------+  +--------------------------+  |
|  | Agronomy RAG       |  | Scheme Eligibility    |  | Agromet & Mandi          |  |
|  | Pipeline (LangGraph|  | Rule Engine (Zero-PII)|  | Aggregation Service      |  |
|  +---------+----------+  +-----------+-----------+  +------------+-------------+  |
+------------|-------------------------|---------------------------|----------------+
             |                         |                           |
   +---------+---------+     +---------+---------+       +---------+---------+
   | AI & VOICE LAYER  |     | DATA PERSISTENCE  |       | EXTERNAL AGRI APIs|
   | - Sarvam Saaras   |     | - PostgreSQL 16   |       | - e-NAM / Agmark  |
   |   (Telugu ASR)    |     | - PostGIS (Mandal)|       | - IMD Agromet API |
   | - Gemini 2.5 Flash|     | - pgvector (RAG)  |       | - Open-Meteo Soil |
   | - Sarvam Bulbul   |     | - Redis 7 Cache   |       |   Moisture (10cm) |
   |   (Telugu TTS)    |     | - S3/GCS Audio CDN|       | - ANGRAU Bulletins|
   +-------------------+     +-------------------+       +-------------------+
```

### 2. Voice-to-Agronomy RAG Pipeline (Screen 4 Execution Flow)
1. **Audio Capture:** Mobile client records 16kHz mono Opus/AAC audio stream (`max 60s`).
2. **Code-Mixed ASR:** Stream is processed by **Sarvam AI `Saaras-v2` / Bhashini ASR**, returning both the native Telugu transcript (`నా గుంటూరు బిపిటి 5204 వరి పంటలో...`) and normalized English query (`How to control yellow stem borer in my BPT 5204 paddy crop in Guntur?`).
3. **Editable Confirmation:** Transcript is displayed live on Screen 4 so the farmer can verify or tap `Stop & Ask AI`.
4. **Contextual Enrichment:** Backend automatically injects the farmer's profile metadata (`Location: Guntur Mandal`, `Season: Kharif`, `Soil Moisture: 24%`, `48hr Rain Forecast: 70%`) into the prompt context.
5. **Vector Retrieval (`pgvector`):** Hybrid semantic + keyword search queries the **Verified ANGRAU Vyavasaya Panchangam & ICAR Knowledge Base** for `crop="Paddy"`, `variety="BPT 5204"`, `pest="Yellow Stem Borer (Scirpophaga incertulas)"`.
6. **Guardrailed LLM Synthesis:** **Gemini 2.5 Flash** generates a structured bilingual response strictly using chemical dosages approved by CIB&RC and ANGRAU. *Crucial Context Awareness:* If the 48-hour rain forecast is `>60%` (as in Screen 8), the AI explicitly warns the farmer not to spray foliar pesticides immediately before heavy rain.
7. **TTS Audio Synthesis:** Response is converted to a natural Telugu voice clip via **Sarvam `Bulbul-v1`** and streamed back to the mobile app alongside structured text cards.

### 3. Production Database Schema (PostgreSQL 16 + PostGIS + pgvector)

```sql
-- Enable Required Extensions
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS "postgis";
CREATE EXTENSION IF NOT EXISTS "vector";

-- 1. Farmers & Agronomic Profile Table (Screen 1 & Screen 7)
CREATE TABLE farmers (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    phone_e164 VARCHAR(15) UNIQUE, -- Nullable for Guest Mode sessions
    full_name_en VARCHAR(120) NOT NULL DEFAULT 'Farmer',
    full_name_te VARCHAR(120) NOT NULL DEFAULT 'రైతు గారు',
    preferred_lang VARCHAR(5) NOT NULL DEFAULT 'te', -- 'te', 'en', 'hi'
    ownership_type VARCHAR(30) DEFAULT 'PATTADAR', -- 'PATTADAR', 'TENANT', 'ASSIGNED'
    landholding_acres NUMERIC(5,2) DEFAULT 2.50,
    landholding_category VARCHAR(30) DEFAULT 'SMALL_MARGINAL', -- '< 2 Hectares / 5 Acres'
    state VARCHAR(50) DEFAULT 'Andhra Pradesh',
    district VARCHAR(50) DEFAULT 'Guntur',
    mandal VARCHAR(80) DEFAULT 'Guntur Mandal',
    primary_crops JSONB DEFAULT '["Chilli", "Paddy"]'::jsonb,
    farm_location GEOGRAPHY(POINT, 4326),
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- 2. ANGRAU / ICAR Verified Knowledge Base for RAG (Screen 4)
CREATE TABLE agronomy_knowledge_chunks (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    source_authority VARCHAR(50) NOT NULL, -- 'ANGRAU', 'PJTSAU', 'ICAR_KVK'
    crop_name VARCHAR(50) NOT NULL,        -- 'Paddy', 'Chilli', 'Cotton'
    crop_variety VARCHAR(50),              -- 'BPT 5204', 'Guntur Teja'
    pest_or_disease VARCHAR(100),          -- 'Yellow Stem Borer', 'Leaf Curl Virus'
    content_en TEXT NOT NULL,
    content_te TEXT NOT NULL,
    approved_chemicals JSONB,              -- Strictly CIB&RC approved dosages
    embedding VECTOR(768),                 -- Text embedding vector
    valid_season VARCHAR(20) DEFAULT 'KHARIF'
);

-- 3. Voice & Vision AI Consultations History (Screen 4 & Screen 7)
CREATE TABLE ai_consultations (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    farmer_id UUID REFERENCES farmers(id) ON DELETE CASCADE,
    query_type VARCHAR(20) NOT NULL, -- 'VOICE', 'CROP_SCAN', 'TEXT'
    transcript_en TEXT,
    transcript_te TEXT,
    audio_input_url TEXT,
    image_input_url TEXT,
    ai_response_en TEXT NOT NULL,
    ai_response_te TEXT NOT NULL,
    audio_output_url TEXT,
    is_saved BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- 4. e-NAM APMC Mandi Market Rates (Screen 6)
CREATE TABLE mandi_market_rates (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    state VARCHAR(50) NOT NULL,
    district VARCHAR(50) NOT NULL,
    apmc_yard_en VARCHAR(100) NOT NULL, -- 'Guntur Mirchi Yard'
    apmc_yard_te VARCHAR(100) NOT NULL, -- 'గుంటూరు యార్డ్'
    commodity_en VARCHAR(80) NOT NULL,  -- 'Guntur Teja Chilli'
    commodity_te VARCHAR(80) NOT NULL,  -- 'తేజ మిర్చి'
    grade VARCHAR(30) DEFAULT 'Grade A',
    lot_type VARCHAR(50) DEFAULT 'Drying Lot',
    modal_price_inr INTEGER NOT NULL,   -- 22400
    min_price_inr INTEGER NOT NULL,     -- 18500
    max_price_inr INTEGER NOT NULL,     -- 24200
    price_change_inr INTEGER DEFAULT 0, -- +350
    price_change_pct NUMERIC(4,2) DEFAULT 0.0, -- 1.60
    arrivals_bags INTEGER DEFAULT 0,    -- 42500
    traded_bags INTEGER DEFAULT 0,      -- 38200
    enam_status VARCHAR(20) DEFAULT 'OPEN',
    audio_bulletin_url TEXT,
    recorded_at TIMESTAMPTZ DEFAULT NOW()
);

-- 5. Government Schemes & Eligibility Rules (Screen 5 & Screen 7)
CREATE TABLE government_schemes (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    scheme_code VARCHAR(40) UNIQUE NOT NULL, -- 'YSR_RYTHU_BHAROSA_PM_KISAN'
    title_en VARCHAR(150) NOT NULL,
    title_te VARCHAR(150) NOT NULL,
    category VARCHAR(50) NOT NULL, -- 'INCOME_SUPPORT', 'CROP_INSURANCE', 'SUBSIDY'
    annual_benefit_inr INTEGER,    -- 13500
    next_installment_inr INTEGER,  -- 7500
    deadline_date DATE,
    eligibility_rules JSONB NOT NULL, -- Evaluated locally & on server without PII
    description_en TEXT NOT NULL,
    description_te TEXT NOT NULL,
    audio_guide_url TEXT
);
```

### 4. Core REST API Specification
* `POST /api/v1/auth/otp/request` & `POST /api/v1/auth/otp/verify` — Phone/Voice OTP authentication.
* `GET /api/v1/home/summary?mandal=Guntur&lang=te` — Returns consolidated Home payload (Today's Audio Advisory URL, Weather snapshot, Quick Tools metadata) in a single `<15KB` gzipped JSON call.
* `POST /api/v1/ai/voice-query` — Accepts audio/transcript + Mandal context; returns bilingual ANGRAU-grounded agronomic advice + Telugu TTS audio URL.
* `POST /api/v1/ai/crop-scan` — Accepts leaf image + crop tag; returns pest/disease classification, confidence score, and treatment protocol.
* `POST /api/v1/schemes/check-eligibility` — Stateless zero-PII endpoint evaluating `{ landholding, ownership, crops, state }` against active scheme rules.
* `GET /api/v1/market/rates?yard=Guntur+Mirchi+Yard&crop=Chilli` — Returns live e-NAM modal/min/max prices, bag volumes, and 1-min Telugu market audio bulletin.
* `GET /api/v1/weather/agromet?district=Guntur&mandal=Tenali` — Returns IMD + Open-Meteo 4-metric telemetry (including 10cm soil moisture) and severe weather alerts.

---