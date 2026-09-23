# CAD Digital Vault

Digitization of loan documents and Safe In / Safe Out for Bank Alfalah Credit Administration (CAD), built from the BRD and Charge/Security document master list in this folder.

## Stack

- **Frontend:** React 19 + Vite + Tailwind CSS 4 + React Router
- **Backend:** FastAPI + SQLAlchemy + JWT auth
- **Database:** SQLite (`backend/cad_vault.db`) — portable; can move to SQL Server later

## Features

- Loan account search (account / ticket / customer)
- Document records with Charge / Security / Property / Ancillary categories
- Cascading document type dropdowns seeded from `Charge and Security Documents Details.xlsx`
- Scan upload and view
- Safe In / Safe Out with recipient signature pad
- Maker–Checker approval for documents and vault movements
- Roles: Maker, Checker, Admin, Viewer
- Admin masters: users, CAD regions, cabinets
- Reports and audit trail

## Setup

### Backend

```powershell
cd backend
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
copy .env.example .env
python -m app.seed
uvicorn app.main:app --reload --port 8001
```

Health check: http://localhost:8001/api/health

### Frontend

```powershell
cd frontend
npm install
npm run dev
```

Open http://localhost:5173

## Demo users

| Role    | Email                 | Password     |
|---------|-----------------------|--------------|
| Maker   | maker@bafl.local      | Maker@123    |
| Checker | checker@bafl.local    | Checker@123  |
| Admin   | admin@bafl.local      | Admin@123    |
| Viewer  | viewer@bafl.local     | Viewer@123   |

Sample loan: `LN-2024-88317`

## Maker–Checker flow

1. Maker adds/updates a document → status **Awaiting approval**
2. Checker approves the document
3. Maker submits Safe In / Safe Out → movement awaiting approval
4. Checker approves the movement → vault status updates (In vault / Checked out)
5. Return = new Safe In (history retained; cannot Safe Out again until returned)

## Reference documents

- `CAD digital vault BRD.pdf`
- `Charge and Security Documents Details.xlsx`
