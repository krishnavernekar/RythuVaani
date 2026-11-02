## RULE 1: Agronomic Safety & Zero-Hallucination Guardrails (CRITICAL)
1. **Strict RAG Grounding:** The "Ask Agronomist AI" assistant MUST NEVER invent chemical pesticide names, dosages, or mixing ratios from ungrounded LLM weights. All chemical recommendations must be retrieved from the `agronomy_knowledge_chunks` table (verified ANGRAU / PJTSAU / ICAR-KVK package of practices).
2. **Banned Pesticide Filter:** A deterministic post-processing filter must block any mention of CIB&RC banned or restricted chemicals (e.g., Monocrotophos on vegetables, Endosulfan, Phorate, Methyl Parathion, Triazophos).
3. **Mandatory MAO Disclaimer:** Screen 4 and all AI advisory outputs MUST display the official disclaimer banner: *"Preliminary AI agronomist advisory. Always consult your local Mandal Agriculture Officer (MAO) for severe infections or seed blight."*
4. **Weather-Aware Spraying Interlock:** Before recommending any foliar pesticide or fertilizer spray, the backend MUST check the Mandal's 24-hour rain probability and wind speed. If `rain_prob > 50%` or `wind_speed_kmh > 18`, the AI must prepend a warning to postpone spraying.

## RULE 2: Privacy & Zero-PII Scheme Eligibility Rule
1. **No Sensitive Data in Scheme Checker:** As explicitly promised on Screen 5, the AI Scheme Eligibility Checker MUST NEVER prompt for Aadhaar numbers, bank account numbers, IFSC codes, or OTPs. Eligibility evaluation must rely strictly on agronomic attributes (Landholding size, Ownership category, Crops cultivated, District/Mandal).

## RULE 3: Bilingual UI & Localization Standards
1. **Zero Hardcoded Strings:** Every UI label must come from the typed i18n dictionary (`en.json`, `te.json`, `hi.json`).
2. **Instant State Switching:** Tapping the `తె / EN` header pill must flip the active language in Zustand synchronously without unmounting the current screen or losing form state (such as an active voice recording or Step 2 of the Scheme Checker).
3. **Telugu Typography Protection:** Never apply `overflow: hidden` with tight line-heights (`<1.5`) on Telugu text containers.

## RULE 4: Offline-First & Low-Bandwidth Resilience
1. **Stale-While-Revalidate Everywhere:** Every GET request (Weather, Mandi Rates, Schemes, Profile) must render cached SQLite/MMKV data immediately (`<50ms`) with a `"Last updated 10m ago"` timestamp while fetching fresh data in the background.
2. **Audio Compression:** All daily Telugu voice bulletins must be encoded in **Opus / HE-AAC at 32 kbps mono** (`~240 KB` per 1-minute bulletin) so they play instantly on rural 2G/4G connections.
```

---