# Shopify Customer Onboarding Automation

## Overview

This project is an automated Shopify onboarding workflow built using Pipedream.

The workflow receives Shopify order webhooks and validates:

- Order tag = `MakeOrder`
- Customer tag = `ColdCustomer`
- Order amount > `₹2500`

If all conditions are satisfied:

1. An immediate onboarding email is sent.
2. After 5 minutes, a discount unlocked email is sent.

Otherwise, the workflow stops automatically.

---

## Features

- Shopify webhook integration
- HTTP trigger handling
- Conditional workflow validation
- Automated Gmail integration
- Delayed workflow execution
- Error-safe flow handling

---

## Workflow Logic

```text
Webhook Received
       ↓
Check Order Tag = MakeOrder
       ↓
Check Customer Tag = ColdCustomer
       ↓
Check Amount > 2500
       ↓
YES ----------------→ Send Welcome Email
                          ↓
                     Wait 5 Minutes
                          ↓
                    Send Discount Email

NO -----------------→ Stop Workflow
