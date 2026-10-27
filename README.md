# Pulse & Vital

Pulse & Vital is a **static, browser-only health-vault prototype**. It demonstrates separate patient and hospital portals, a health ID/QR-style card, record categories, an emergency summary, an access log, and a sample doctor record-entry workflow.

> **Demo only:** Do not enter real Aadhaar details, patient identifiers, phone numbers, passwords, or medical information. The current application does not provide production authentication, server-side storage, encryption, or a clinical system integration.

## Live demo

- Vercel production site: https://pulse-vital.vercel.app/
- AppDeploy demo: https://pulse-vital-d0f8fj.v2.appdeploy.ai/

The repository is intentionally small: the current prototype is implemented in a single `index.html` file with inline CSS and JavaScript. For this release, `index.html` was restored to the exact supplied source and left unchanged; the fixture and documentation are aligned around it.

## Features in the prototype

- Patient portal with a demo OTP flow and a consent checkbox.
- Vault overview and grouped medical records: scans, labs, medicines, allergies, surgeries, eye care, Ayurvedic care, full-body checks, and history.
- Health ID card with a changing code and a QR-style visual.
- Emergency contact editor and patient-side access log.
- Separate hospital/doctor portal, patient ID lookup, emergency summary, recent lookups, and record-entry workflow.
- Browser-local persistence for sample records, lookup history, and emergency contact.
- Responsive layout in the deployed demo for desktop, tablet, and phone screens.

## Requirements

- A modern browser with JavaScript enabled.
- Python 3 (recommended for a local static server), or Node.js if using the optional Node-based server below.
- Internet access is optional for the app itself; the Lora and Poppins fonts are loaded from Google Fonts and fall back to local system fonts if unavailable.

No build step or package installation is required for the current single-file version.

## Run locally

### Option A: Python

```bash
git clone https://github.com/vishnupriyakv2008-ctrl/pulse-vital.git
cd pulse-vital
python -m http.server 8000
```

Open http://localhost:8000 in your browser.

### Option B: Node.js

From the repository directory:

```bash
npx --yes serve . -l 8000
```

Then open http://localhost:8000.

For local debugging, use the browser developer tools console and the Application/Storage panel. The main UI and state logic live in `index.html`.

## Environment variables

**No environment variables are required or read by the current static prototype.** There is no required `.env` file, API key, database URL, or backend service. Do not add real secrets to the HTML file.

| Variable | Required | Purpose |
| --- | --- | --- |
| *(none)* | No | The current app has no environment-variable configuration. |

The live hosting provider may have its own deployment settings, but they are not consumed by this repository's current `index.html`.

## Demo credentials and flows

The following values are public, hard-coded demo data in the prototype. They are **not credentials for a real hospital**.

| Flow | Demo input |
| --- | --- |
| Patient vault | Enter any 12-digit test-only number and a 10-digit test-only phone number, request the demo OTP, then use the displayed/populated code. |
| Hospital portal | Hospital ID `SUN-KOC-0142` or `CCH-KOC-0207`; use a syntactically valid email and a password with at least six characters. |
| Patient lookup | `HP-2026-000001` is the seeded demo patient ID. |
| QR lookup | The UI shows a rotating demo token. The prototype does not access the camera or validate a standards-compliant QR payload. |

The OTP is generated in the browser and shown to the user as a demo toast; it is not sent by SMS. Hospital sign-in performs client-side input checks only. These flows must not be described as secure authentication.

## Data and persistence

- The browser stores the sample dataset under the local-storage key `pulse-vital-v1`.
- Records, lookup history, and the emergency contact are saved to that browser profile and origin only.
- Data is not synchronized between devices or shared between different browsers.
- Reset the demo using the in-app **Reset demo data** action, or clear the site data for the origin in browser settings.
- `tests/sample-data.json` mirrors the initial `seed()` state in `index.html`, including its 18 records, flags, access log, recent lookups, emergency contact, patient ID, and local-storage key. It is documentation/test input and is not automatically imported by the application. Re-sync it whenever the seed is intentionally changed.

## Documentation and test cases

- [Technical Design Document](docs/TECHNICAL_DESIGN.md) — architecture, Mermaid diagrams, data model, workflows, and limitations.
- [Sample data fixture](tests/sample-data.json) — synthetic records, access events, recent lookups, and emergency contact.
- [Manual test cases](tests/TEST_CASES.md) — functional, validation, persistence, and responsive checks.

## Technology and architecture

- HTML with embedded CSS and vanilla JavaScript.
- Screen navigation is managed by in-memory state; the UI is rendered from the screen definitions in `index.html`.
- `localStorage` is the only persistence mechanism.
- Google Fonts is the only external resource referenced by the app.
- There is no API server, database, real file upload, SMS gateway, camera integration, or clinical/FHIR service in this prototype.

See the Technical Design Document for details and next steps.

## Production-readiness warning

This project is a demonstration UI, not an electronic health record system or emergency response service. Before real-world use, implement and independently review backend authentication and authorization, encryption in transit and at rest, secure medical-file storage, audit logging, data retention/deletion, consent handling, input/output validation, threat modelling, backups, monitoring, and relevant legal/regulatory requirements. Never use the demo for real clinical decisions.
