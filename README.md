# Fintech Compliance — LangGraph + Swytchcode

4-step compliance workflow for fintech onboarding:

1. **Plaid Sandbox** — simulate bank account linking and retrieve account context
2. **Persona KYC** — create an inquiry, approve it in sandbox, verify status
3. **Dwolla** — create customer + funding source (only runs if Persona KYC passes)
4. **Policy enforcement** — blocks transfers if KYC is not approved, rejects unsupported account types, holds transactions above threshold

Built with [LangGraph](https://github.com/langchain-ai/langgraph) and [Swytchcode](https://swytchcode.com).

---

## Prerequisites

- **Python 3.9+**
- **Swytchcode CLI:** install with the verified script for your platform:
  
  npm install -g swytchcode

## Setup

1. Clone the repo:
   ```bash
   git clone https://github.com/swytchcodehq/Fintech-Compliance-Langgraph-Demo.git
   cd Fintech-Compliance-Langgraph-Demo
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Copy the example env file and fill in your keys:
   ```bash
   cp .env.example .env
   ```
4. Fetch the integrations declared in `.swytchcode/tooling.json`:
   ```bash
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
