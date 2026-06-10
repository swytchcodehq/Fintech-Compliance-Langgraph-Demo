# Fintech Compliance Demo — Setup Guide for Presenter

## Prerequisites

Make sure these are installed:
- Python 3.9+
- swytchcode CLI (`swytchcode --version` to verify)

---

## Step 1 — Get the demo code

Download and extract the zip:
```bash
# Option A — via CLI (once released)
swytchcode demo fintech-compliance

# Option B — download directly
# GET https://api-v2.swytchcode.com/v2/cli/demos/download?demo=fintech.compliance&framework=langgraph
```

Or unzip the provided `fintech-compliance-langgraph.zip` into a folder.

---

## Step 2 — Install dependencies

```bash
cd fintech-compliance-langgraph
pip install -r requirements.txt
```

---

## Step 3 — Set up credentials

```bash
cp .env.example .env
```

Open `.env` and fill in the following:

### swytchcode
```
SWYTCHCODE_TOKEN=        # Run: swytchcode whoami — copy the session token
```
> If not logged in, run: `swytchcode login`

### Plaid Sandbox
```
PLAID_CLIENT_ID=         # dashboard.plaid.com → Team Settings → Keys
PLAID_SECRET=            # same page, Sandbox secret
PLAID_VERSION=2020-09-14 # leave as-is
```

### Persona Sandbox
```
PERSONA_API_KEY=         # app.withpersona.com → API → API Keys
PERSONA_TEMPLATE_ID=     # Templates → copy the tmpl_xxx ID
```

### Dwolla Sandbox
```
DWOLLA_APP_KEY=          # accounts-sandbox.dwolla.com → Applications → your app
DWOLLA_APP_SECRET=       # same page
```

### Demo user (optional — defaults are fine for demo)
```
USER_NAME=John Smith
USER_EMAIL=john@example.com
```

---

## Step 4 — Fetch integrations

```bash
swytchcode bootstrap
```

This downloads the Plaid, Persona, and Dwolla integration bundles. Only needed once.

> **Note:** If the zip already contains a `.swytchcode/integrations/` folder, skip this step — bundles are already included.

---

## Step 5 — Run the demo

```bash
python main.py
```

---

## Expected output

```
🏦 Fintech Compliance LangGraph Demo
   Plaid → Persona KYC → Dwolla → Policy Enforcement
============================================================
STEP 1: Plaid Sandbox — Simulate bank account linking
  ✅ Account: checking (depository) | Balance: $100.00

STEP 2: Persona KYC — Create inquiry + simulate approval
  ✅ Inquiry created
  ✅ Inquiry status: approved
  🟢 KYC APPROVED → proceeding to Dwolla

STEP 3: Dwolla — Create customer + funding source
  ✅ Dwolla token obtained
  ✅ Customer ID: <uuid>
  ✅ Funding source: created

STEP 4: Swytchcode Policy Enforcement
  ✅ Policy 1 PASSED: KYC approved
  ✅ Policy 2 PASSED: Account type 'checking' is supported
  ✅ Policy 3 PASSED: Balance within threshold
  🟢 ALL POLICIES PASSED — Transfer may proceed
```

---

## Troubleshooting

| Error | Fix |
|---|---|
| `Missing required environment variables` | Fill in all fields in `.env` |
| `session expired` on swytchcode | Run `swytchcode login` |
| `context canceled` on Dwolla | Dwolla sandbox is flaky — run again |
| `invalid_client` on Dwolla token | Check DWOLLA_APP_KEY and DWOLLA_APP_SECRET — copy from dashboard, not screenshot |
| Persona status not `approved` | Make sure PERSONA_TEMPLATE_ID is correct (`tmpl_xxx`) |
