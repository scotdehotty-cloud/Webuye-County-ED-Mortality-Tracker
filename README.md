[README.md](https://github.com/user-attachments/files/26505035/README.md)
# Webuye County Hospital — ED Command Centre

**Integrated ICD-11 · MOH 301 · KHIS Mortality & Morbidity Tracker**  
*Emergency Department, Webuye County Hospital, Bungoma County, Kenya*

---

## Overview

The **WCH ED Command Centre** is a fully integrated, browser-based clinical information system purpose-built for the Emergency Department at Webuye County Hospital. It digitises the Kenya MOH 301 Inpatient Register, applies WHO ICD-11 (2024 release) diagnosis coding, auto-calculates KHIS/DHIS2 performance indicators, and embeds a live AI clinical intelligence assistant powered by Claude.

The system runs entirely as a single HTML file — no installation, no server, no database required. It works in any modern browser on a desktop, laptop, or tablet.

---

## Files

```
webuye_ed_tracker.html   ← Main application (open this in any browser)
webuye_ed_tracker.css    ← Standalone stylesheet (optional — see §Deployment)
README.md                ← This file
```

---

## Features

### 1. Dashboard
- Live KPI cards: total attendances, admissions, discharges, deaths (ED + DOA), LAMA, referrals
- Triage strip (T1–T4) with real-time patient counts and colour coding
- Full patient table with search, edit, and delete on every row
- Sticky topbar with live clock

### 2. MOH 301 Register
The complete Kenya MOH 301 Inpatient/Emergency Register — digitised.

| MOH 301 Field | System Field |
|---|---|
| IP/OP Number | Auto-generated or manual entry |
| Date & Time of Admission | datetime-local input |
| Full Name | Surname, Other Names |
| Date of Birth / Age | Calculated age displayed |
| Sex | Male / Female / Intersex |
| Sub-County of Origin | Webuye East, West, Kanduyi, Sirisia, Kabuchai, Bumula, Kimilili, Mt Elgon |
| Presenting Complaint | Free text |
| Triage Level | T1 Resuscitation / T2 Emergent / T3 Urgent / T4 Less Urgent |
| Vital Signs | BP, Pulse, SpO2, Temperature, GCS |
| Diagnosis (ICD-11) | Searchable, multi-code |
| External Cause | ICD-11 external cause codes |
| Outcome | Discharged / Admitted / LAMA / Referred / Death / DOA |
| Ward / Referral Destination | Free text |
| Attending Clinician | Free text |

Filters: All · Admitted · Discharged · Deaths · DOA · LAMA · Referred  
Search: Name, IP number, or diagnosis — live filtering.

### 3. Mortality & Morbidity Review
- Death register with ICD-11-coded underlying and immediate causes
- WHO avoidable death classification (Yes / No / Uncertain)
- M&M review status tracking (Pending / Reviewed / Not started)
- DOA flag
- KPIs: total deaths, DOA, case fatality rate, potentially avoidable deaths, pending M&M reviews
- Charts: cause-of-death distribution, death by triage level

### 4. KHIS Indicators
Auto-calculated from live register data, ready for DHIS2 upload.

| KHIS Indicator | Target | Calculated From |
|---|---|---|
| ED Daily Attendance | ≥ 50/day | Total patients |
| Case Fatality Rate | < 5% | Deaths ÷ Total × 100 |
| Admission Rate | 30–40% | Admissions ÷ Total × 100 |
| LAMA Rate | < 5% | LAMA ÷ Total × 100 |
| Triage Completion Rate | 100% | All patients triaged |
| ICD-11 Coding Rate | 100% | All patients have ICD-11 code |
| M&M Review Rate | 100% of deaths | Reviewed ÷ Deaths × 100 |
| Referral Rate | Monitor trends | Referrals ÷ Total × 100 |

**IDSR Notifiable Disease Surveillance** — alert thresholds for Malaria (severe), Cholera, Typhoid, Tuberculosis, Road Traffic Injury, Self-harm.  
**ED Quality Radar** — benchmarks WCH ED performance against Kenya national standards across 6 dimensions.

### 5. ICD-11 Browser
- 32 WHO ICD-11 (2024) codes preloaded, covering ED priority conditions
- Searchable by code or description
- Chapters: Cardiovascular, Respiratory, Neurology, Infections, Injuries, Toxicology, Endocrine, Renal, Surgical, Obstetric, Blood, External Causes
- External cause cross-reference column

### 6. Analytics
- **Top 10 diagnoses** — horizontal bar chart (ICD-11 coded)
- **Outcome distribution** — doughnut chart
- **Age & sex distribution** — grouped bar chart by age band
- **Sub-county catchment** — animated progress bars showing patient origin
- **AI Clinical Intelligence** — live, context-aware assistant (see below)

### 7. AI Clinical Intelligence (Live)
Powered by Claude (Anthropic). The assistant has full access to the live ED register in every query.

**Quick-action buttons:**

| Button | What it generates |
|---|---|
| Mortality Analysis | Top causes, CFR review, avoidable deaths, M&M actions, 3 evidence-based interventions |
| Triage & Acuity Review | Load patterns, T1/T2 flags, ED flow recommendations, LAMA follow-up |
| KHIS Compliance Report | Traffic-light status per indicator, gaps, actions, DHIS2 upload readiness |
| Disease Burden Analysis | ICD-11 patterns, IDSR alerts, outbreak risk signals, public health actions |
| Generate MOH 301 Report | Structured monthly summary for Medical Superintendent |
| M&M Review Brief | Per-case summaries, panel questions, learning points, WHO safety flags |

**Free-text chat** — ask anything: clinical questions, interpretation of indicators, coding advice, referral decisions. The assistant cites your actual patient data in every response.

**Conversation memory** — retains the last 6 message pairs for follow-up questions.

---

## Patient Record Form

The New Patient / Edit form is structured in four sections aligned to MOH 301:

- **Section A** — Patient Identification (IP number, name, DOB, sex, sub-county)
- **Section B** — Triage & Clinical Presentation (triage level, arrival mode, complaint, 5 vital signs)
- **Section C** — Diagnosis / ICD-11 (live search with multi-code selection, external cause, clinician, notes)
- **Section D** — Outcome & Disposition (outcome, dates, ward, referral destination, cause of death fields for deaths)

All fields are editable at any time. Records can be permanently deleted with a confirmation step.

---

## Deployment

### Option A — Single file (simplest)
The CSS is embedded inside `webuye_ed_tracker.html`. Just open the file:

```
Double-click webuye_ed_tracker.html
```

Works in Chrome, Edge, Firefox, Safari. No internet required for the register — only the AI chat requires network access to the Anthropic API.

### Option B — Separate CSS file
If deploying as part of a web project or local server:

1. Place both files in the same folder:
   ```
   /your-project/
     webuye_ed_tracker.html
     webuye_ed_tracker.css
   ```
2. In `webuye_ed_tracker.html`, replace the `<style>…</style>` block inside `<head>` with:
   ```html
   <link rel="stylesheet" href="webuye_ed_tracker.css">
   ```

### Option C — Electron desktop app
To wrap into a standalone Windows/macOS/Linux desktop application:

```bash
npm install -g electron
mkdir wch-ed && cd wch-ed
cp /path/to/webuye_ed_tracker.html index.html
cp /path/to/webuye_ed_tracker.css .
```

Create `main.js`:
```javascript
const { app, BrowserWindow } = require('electron')
const path = require('path')

function createWindow () {
  const win = new BrowserWindow({
    width: 1440,
    height: 900,
    title: 'Webuye County Hospital — ED Command Centre',
    webPreferences: { nodeIntegration: false }
  })
  win.loadFile('index.html')
  win.setMenuBarVisibility(false)
}

app.whenReady().then(createWindow)
app.on('window-all-closed', () => { if (process.platform !== 'darwin') app.quit() })
```

Create `package.json`:
```json
{
  "name": "wch-ed-tracker",
  "version": "1.0.0",
  "main": "main.js",
  "scripts": { "start": "electron ." },
  "devDependencies": { "electron": "^latest" }
}
```

```bash
npm install
npm start
```

### Option D — Network / LAN deployment
Serve from any static file server so all ED workstations share the same URL:

```bash
# Python (any computer on the network)
cd /path/to/files
python3 -m http.server 8080
```

Access from any device on the same network:
```
http://[server-ip]:8080/webuye_ed_tracker.html
```

> **Note:** Data is currently stored in browser memory (session only). For persistent multi-user data across sessions, a backend API (Node.js + SQLite or PostgreSQL) or localStorage persistence layer should be added for production use.

---

## AI Clinical Intelligence — Configuration

The AI assistant calls the Anthropic Claude API directly from the browser. In the current build, the API key is handled server-side by the Claude.ai environment. For standalone deployment outside Claude.ai, add your API key:

In `webuye_ed_tracker.html`, locate the `callAI()` function and add the `x-api-key` header:

```javascript
const response = await fetch('https://api.anthropic.com/v1/messages', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'x-api-key': 'YOUR_ANTHROPIC_API_KEY',        // ← add this
    'anthropic-version': '2023-06-01',              // ← add this
    'anthropic-dangerous-direct-browser-calls': 'true'  // ← add this
  },
  ...
```

> **Security note:** Do not expose your API key in client-side code in a public-facing deployment. For production, proxy the AI requests through a backend service that holds the key server-side.

---

## ICD-11 Codes Included

| Code | Description | Chapter |
|---|---|---|
| KA20 | Acute myocardial infarction | Cardiovascular |
| BA00 | Hypertensive heart disease | Cardiovascular |
| BA80 | Heart failure | Cardiovascular |
| JA00 | Pneumonia | Respiratory |
| JB22 | Acute respiratory failure | Respiratory |
| JA40 | Acute severe asthma | Respiratory |
| ME05 | Cerebrovascular accident (Stroke) | Neurology |
| 8B20 | Seizure / Epilepsy — acute | Neurology |
| 5A10 | Malaria — severe | Infections |
| 1C62 | Typhoid fever | Infections |
| 1A00 | Cholera | Infections |
| 5B70 | Sepsis | Infections |
| 1C16 | Tuberculosis — pulmonary | Infections |
| 1C60 | HIV/AIDS — complications | Infections |
| NA00 | Traumatic brain injury | Injuries |
| NB00 | Fracture — long bone | Injuries |
| NE01 | Burns — major | Injuries |
| NC80 | Abdominal trauma | Injuries |
| SD00 | Poisoning — acute | Toxicology |
| SU99 | Drug overdose | Toxicology |
| DD90 | Diabetic ketoacidosis | Endocrine |
| 5A00 | Hypoglycaemia | Endocrine |
| LA90 | Acute kidney injury | Renal |
| GC00 | Acute appendicitis | Surgical |
| GC50 | Intestinal obstruction | Surgical |
| JA80 | Acute abdomen | Surgical |
| KA25 | Ruptured ectopic pregnancy | Obstetric |
| JA4Z | Post-partum haemorrhage | Obstetric |
| EB01 | Severe anaemia | Blood |
| 3A00 | Sickle cell crisis | Blood |
| PB00 | Self-harm / Attempted suicide | External |
| PA10 | Road traffic accident | External |

External cause codes supported: PA10, PA20, PA30, PA40, PB00, PD30, NF01.

---

## Technical Stack

| Component | Technology |
|---|---|
| Frontend framework | Vanilla HTML5 / CSS3 / ES2022 JavaScript |
| Typography | DM Sans (UI), DM Mono (codes), Playfair Display (logo) via Google Fonts |
| Charts | Chart.js 4.4.1 (CDN) |
| AI | Anthropic Claude claude-sonnet-4-20250514 via REST API |
| Data persistence | In-memory JavaScript array (session only) |
| Build tools | None — zero dependencies, zero build step |
| Browser support | Chrome 90+, Edge 90+, Firefox 88+, Safari 14+ |

---

## Data Standards Compliance

| Standard | Implementation |
|---|---|
| **Kenya MOH 301** | All inpatient register fields mapped; form sections labelled A–D per MOH format |
| **WHO ICD-11 (2024)** | 32 ED-priority codes; multi-code selection; external cause dual coding |
| **KHIS / DHIS2** | 8 core indicators auto-calculated; IDSR notifiable disease surveillance |
| **WHO Avoidable Death Framework** | Avoidability classification on all deaths; M&M review workflow |
| **Kenya EmONC Indicators** | Maternal death flagging; obstetric emergency coding |
| **Kenya IDSR** | Malaria, Cholera, Typhoid, TB, RTI alert thresholds |

---

## Roadmap / Future Enhancements

- [ ] **Offline-first persistence** — IndexedDB or SQLite (Electron) for data survival across sessions
- [ ] **Multi-user sync** — REST API backend (Node.js + PostgreSQL) for concurrent clinician access
- [ ] **Direct DHIS2 integration** — POST aggregate data to KHIS server via DHIS2 API
- [ ] **Printable MOH 301** — Generate PDF register for physical filing
- [ ] **Patient lookup** — Search previous visits by name or national ID
- [ ] **Shift handover report** — Auto-generated end-of-shift summary
- [ ] **SMS alerts** — Notify on-call doctors for T1/T2 triage entries
- [ ] **Expanded ICD-11 library** — Full WHO ED chapter (300+ codes)
- [ ] **Data export** — CSV/Excel export of register and KHIS report

---

## Facility Information

| | |
|---|---|
| **Facility** | Webuye County Hospital |
| **County** | Bungoma County, Western Kenya |
| **Facility Level** | Level 4 County Hospital |
| **Department** | Emergency Department |
| **HMIS Code** | *(to be configured)* |
| **DHIS2 Org Unit** | *(to be configured)* |

---

## Support & Contact

For technical issues, customisation requests, or deployment support, contact the developer or the Bungoma County Health Informatics team.

For clinical content questions regarding ICD-11 coding or KHIS indicators, refer to:
- [WHO ICD-11 Reference](https://icd.who.int/en)
- [Kenya KHIS / DHIS2 Portal](https://hiskenya.org)
- [Kenya MOH Health Information Systems](https://www.health.go.ke)

---

*Built for Webuye County Hospital ED — Version 1.0 · April 2026*  
*ICD-11 WHO 2024 · MOH 301 · KHIS/DHIS2 · Powered by Claude AI*
