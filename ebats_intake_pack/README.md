# EBATS Intake Pack (Starter)

This pack includes:
- CSV field checklists for **Individual**, **Company**, **Partnership**, **Trust**, and **SMSF setup**
- `ebats_openapi.yaml` – a headless backend API skeleton
- `consent_and_engagement.md` – sample client consent wording
- `webhook_events.json` – standard WP ↔ backend event names

## Suggested Architecture
- WordPress (ebats.com.au) = **Headless front end** only
- Custom backend (Node/Go/Python) = **Intake API**, validation, encryption, ATO workflows
- Object storage (S3/Cloud Storage) for documents; **server-side encryption**
- Database with **column-level encryption** for TFNs and other identifiers
- Queue for async tasks (virus scan, OCR, PDF merge)
- E-sign + IDV vendors pluggable

### Frontend pattern
1. Render forms from JSON schema.
2. Chunked uploads to pre-signed URLs.
3. On submit → call `/intake/{type}` then redirect to a status page.

### Security
- TLS 1.2+ everywhere, HSTS, CSP, reCAPTCHA/Turnstile
- TFNs masked on display; never log raw TFNs
- JWT between WP and backend; IP allowlist for webhooks
- Audit trail (who, what, when, IP)

### Workflow
Draft → Submitted → In review → Awaiting client → Lodged → Complete