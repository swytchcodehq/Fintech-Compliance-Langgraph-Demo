# Fintech Compliance — LangGraph + Swytchcode

4-step compliance workflow for fintech onboarding:

1. **Plaid Sandbox** — simulate bank account linking and retrieve account context
2. **Persona KYC** — create an inquiry, approve it in sandbox, verify status
3. **Dwolla** — create customer + funding source (only runs if Persona KYC passes)
4. **Policy enforcement** — blocks transfers if KYC is not approved, rejects unsupported account types, holds transactions above threshold

Built with [LangGraph](https://github.com/langchain-ai/langgraph) and [Swytchcode](https://swytchcode.com).

---

## Setup

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Copy and fill in your API keys
cp .env.example .env

# 3. Fetch all integrations
swytchcode bootstrap
```

## Run

```bash
python main.py
```

## Canonical IDs Used

| Service | Canonical ID                              |
|---------|-------------------------------------------|
| Plaid   | `sandbox.public_token.create`             |
| Plaid   | `item.public_token.exchange.create`       |
| Plaid   | `accounts.get.create`                     |
| Persona | `inquiries.inquiry.create`                |
| Persona | `inquiries.approve.create`                |
| Persona | `inquiries.inquiry.get`                   |
| Dwolla  | `customers.customer.create`               |
| Dwolla  | `customers.customer.list`                 |
| Dwolla  | `customers.funding-source.create`         |
