## 1. Project Identity
- **App Name:** RythuVaani (రైతువాణి AI) - AI Smart Agronomy Companion
- **Target Geography:** Andhra Pradesh & Telangana (Benchmark Hub: Guntur District, Amaravati Zone, Tenali & Lam Mandal Hub).
- **Primary User:** Venkata Reddy Garu (Pattadar farmer, 4.5 Acres in Guntur Mandal, cultivating BPT 5204 Paddy & Guntur Teja Chilli).
- **Languages:** Bilingual-first (Telugu `te` + English `en`, plus Hindi `hi` on onboarding). Every screen header includes an instant 1-tap `తె / EN` toggle.

## 2. Core Domain Glossary (Must Be Preserved in Code & Copy)
- **ANGRAU:** Acharya N.G. Ranga Agricultural University (Lam, Guntur) — the authoritative source for AP crop calendars and pest management.
- **MAO:** Mandal Agriculture Officer — the local government official whom farmers must consult for severe crop infections.
- **Pattadar (పట్టాదారు):** Self-owned landholder holding a legal land title (Patta / 1B ROR), a key determinant for scheme eligibility.
- **BPT 5204 (Samba Mahsuri):** Popular fine-grain Kharif paddy variety grown extensively in Guntur/Krishna deltas; susceptible to Yellow Stem Borer (*Scirpophaga incertulas* / పసుపు కాండం తొలుచు పురుగు).
- **Guntur Teja Chilli (తేజ మిర్చి):** High-pungency export-grade red chilli traded at the APMC Guntur Mirchi Yard (benchmark modal price ~₹22,400/Quintal).
- **e-NAM:** National Agriculture Market electronic trading portal linked to APMC yards.
- **Modal Price (సగటు ధర):** The most frequent price at which a commodity trades in a Mandi yard on a given day (measured in ₹ per Quintal = 100 kg).

## 3. Screen Inventory (8 Canonical Screens from Google Stitch Prototype)
1. `WelcomeAuthScreen`: KVK header, Listen Guide, 3-Language Instant Switch (Telugu/English/Hindi), +91 Mobile OTP login, Voice Assistant Login, Guest Mode.
2. `HomeDashboardScreen`: Farmer greeting, Kharif badge, Today's Audio Advisory (1:15 min), 4-metric Weather Outlook + Spraying Advice, 2x2 Quick Tools grid.
3. `QuickToolsExpandedView`: Focused view of the 4 AI tools (Crop Health Scanner, Ask Agronomist AI, Today's Mandi Rate, Govt Schemes 2025).
4. `AskAgronomistVoiceScreen`: MAO official guidance disclaimer, Quick Inquiry chips, 60s Voice Recorder, live editable bilingual Speech-to-Text transcript, Stop & Ask AI CTA.
5. `GovtSchemesCheckerScreen`: Search + filters, 1-min Audio Guide, Zero-PII 3-Step AI Scheme Eligibility Checker (Step 2: Landholding, Ownership, Crops), Instant match CTA.
6. `MandiMarketRatesScreen`: State/District/Yard selector (Guntur Mirchi Yard), e-NAM Live status, Guntur Teja Chilli Grade A price card (Modal/Min/Max, Arrivals/Traded bags), 1-Min Telugu Voice Report.
7. `FarmerProfileScreen`: Pattadar identity card (4.5 Acres, Guntur Mandal, Chilli & Paddy), 12 Questions / 5 Saved stats, Saved Scheme tracker (YSR Rythu Bharosa / PM-KISAN Convergence - ₹13,500/yr).
8. `AgrometWeatherScreen`: Tenali & Lam Mandal Hub live feed, 32°C status, 4-card Agromet grid (Rain 15% today / 70% Thu, Humidity 68%, Wind 14 km/h SW Safe Spray, Soil Moisture 10cm 24%), 1:10 min Audio Bulletin, Severe Rain Alert (45-60mm) with Urgent Action footer.
```

---