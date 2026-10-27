# Manual Test Cases — Pulse & Vital

These are manual acceptance tests for the browser-only demo. Use only synthetic inputs. The app is not production authentication or a medical-record system.

## Test environment and sample inputs

- Browser: current Chrome, Firefox, Edge, or Safari.
- Desktop viewport: 1440 × 900.
- Tablet viewport: 768 × 1024.
- Mobile viewport: 390 × 844 (also spot-check 320 px width).
- Patient demo ID: `HP-2026-000001`.
- Valid demo hospital IDs: `SUN-KOC-0142`, `CCH-KOC-0207`.
- Hospital email: `demo.doctor@example.test`.
- Demo password: `demo1234`.
- Aadhaar-shaped test input: `1234 5678 9012` (synthetic only; never use a real Aadhaar number).
- Mobile-shaped test input: `9000000000` (synthetic only).

## Cases

| ID | Area / viewport | Steps | Expected result |
| --- | --- | --- | --- |
| PV-001 | Portal selection / desktop | Open the landing screen; choose the patient portal and then return to choose the doctor portal. | Correct sign-in screen opens for each role; the app remains in one page. |
| PV-002 | Patient input validation / mobile | Open patient sign-in; enter fewer than 12 digits for the Aadhaar-shaped field or fewer than 10 phone digits; select **Send OTP**. | A clear validation message is displayed; no valid OTP flow starts. |
| PV-003 | Patient demo sign-in / desktop | Enter the synthetic 12-digit and 10-digit values; select **Send OTP**; note the demo code; accept the consent checkbox; select **Verify and continue**. | The OTP inputs are populated by the demo, validation succeeds, and the patient vault appears. The code is browser-generated, not SMS-delivered. |
| PV-004 | OTP/consent guardrails / desktop | Request the demo OTP, alter one digit, and verify. Repeat with the correct code but leave the consent checkbox unchecked. | A mismatch or consent error is shown and the vault does not open. |
| PV-005 | Records navigation / desktop | Open the vault and select **Scans**, **Blood and lab**, **Medicines**, and **Allergies**. Expand a record such as **CT scan, brain**. | Each category shows its sample records; expanding a record reveals the date, provider, and verification/demo status. |
| PV-006 | Health ID / mobile | From the vault open **Show QR**; observe the time remaining; wait for a minute boundary; use **Print card**. | The displayed demo code rotates at the minute boundary; the ID card remains readable; print opens the browser print flow. The visual is illustrative, not a production identity credential. |
| PV-007 | Emergency contact / desktop | Open Profile/Access log; edit the emergency contact with a blank name and then with a phone string shorter than 10 digits; finally save a valid synthetic contact. | Invalid values are rejected; valid values are reflected in the UI and stored in this browser's local storage. |
| PV-008 | Hospital sign-in / desktop | Open doctor portal; sign in with hospital ID `SUN-KOC-0142`, a valid email-shaped string, and a password of 6+ characters. Repeat with an unknown hospital ID. | Valid demo inputs open Find a patient; unknown hospital ID or invalid email/password length shows an error. This is client-side demo checking only. |
| PV-009 | Patient lookup / desktop | Sign in as doctor; search for `HP-2026-000001`; open records. | The emergency summary shows the seeded group, allergies, medicines, emergency contact, and relevant sample records. A local lookup/access entry is added. |
| PV-010 | Unknown patient / mobile | Sign in as doctor; search for `HP-INVALID-999999`; select **Open records**. | The page stays on Find a patient and displays a no-vault-found validation message. |
| PV-011 | QR demo flow / desktop | On a patient device, open the health ID and note the current displayed token. In the doctor portal choose QR scan, use the current device-code action, then open records. Try an invalid token as a negative check. | A current or just-previous minute token is accepted by the demo; an unrecognized/expired token is rejected. No actual camera is used. |
| PV-012 | Add record validation / desktop | Sign in as doctor, open the known patient, choose **Add a record**, and attempt to sign with no note/file. Then add a note and title and sign. | Blank submission is rejected; a note is sufficient to create a sample record; confirmation appears and the record/access log is updated. |
| PV-013 | Simulated upload / desktop | On **Add a record**, choose the sample scan action or select a test file; wait for the displayed progress to complete; sign. | Progress reaches completion and the record can be added. No file bytes are sent to a server; treat this as UI simulation only. |
| PV-014 | Persistence after reload / desktop | Edit the emergency contact or add a demo record; reload the same origin and revisit the relevant view. | Saved `D` data remains in the same browser profile under `pulse-vital-v1`. Clearing browser storage or switching browsers removes/does not share it. |
| PV-015 | Responsive presentation / desktop, tablet, mobile | Open the deployed responsive demo at 1440 × 900, 768 × 1024, 390 × 844 and 320 px width. Navigate patient and doctor screens and open a modal. | Layout adapts to the viewport, content and controls remain usable, and no page-level horizontal scrolling is introduced. The responsive deployed variant removes the screen-picker dock and simulated device/browser frames. |

## Pass/fail recording template

For each execution, record:

- **Test ID:** PV-___
- **Build / URL:** 
- **Browser and viewport:** 
- **Result:** Pass / Fail / Blocked
- **Observed result:** 
- **Defect or screenshot reference:** 

## Notes

- These cases are documentation-level manual tests; no automated test runner is configured in the current single-file repository.
- `tests/sample-data.json` documents the shape of the seeded local-storage object.
- The OTP, hospital login, QR token, file upload, print, and wallet interactions are demo-only.
- Never use real personal or medical information in test execution.
