# ⚡ Facilio Readings Mass Onboarder

A Streamlit app to bulk-configure new readings into Facilio — validated, batched, and confirmed at every step.

---

## Prerequisites

- Python 3.8+
- pip

---

## Setup

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Run the app
streamlit run app.py
```

The app will open at: `http://localhost:8501`

---

## What You Need Before Running

| # | Item | How to Get It |
|---|------|--------------|
| 1 | **Facilio Base URL** | e.g. `https://app.facilio.co.ae` |
| 2 | **Cookie string** | F12 → Network → any `/api/` request → Request Headers → `Cookie` |
| 3 | **X-Csrf-Token** | Same request → `X-Csrf-Token` header (63-char hex) |
| 4 | **Excel file** | Columns: `Asset Category`, `Reading Display Name`, `Reading Type`, `Unit` |

---

## Excel Format

| Column | Required | Notes |
|--------|----------|-------|
| `Asset Category` | ✅ | Must match Facilio displayName exactly |
| `Reading Display Name` | ✅ | Name of the reading |
| `Reading Type` | ✅ | One of: `Decimal`, `Number`, `Boolean`, `String` |
| `Unit` | Optional | SI unit symbol — used to map to Facilio metric |

---

## How It Works

1. **Connect** — validates credentials against Facilio
2. **Upload** — validates Excel, sorts by Asset Category
3. **Fetch** — pulls all Facilio categories (fully paginated) + metric/unit map
4. **Cross-check** — flags any Excel rows whose category doesn't exist in Facilio
5. **Pilot** — runs first 3 readings, waits for your confirmation
6. **Batch** — processes 50 rows/batch, confirms before each
7. **Report** — downloads Excel report with Success/Failed/Duplicate/Skipped sheets
