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


```
# Technologies Used
- Pipedream
- Node.js
- Shopify Webhooks
- Gmail API

# Node.js Validation Logic
export default defineComponent({
  async run({ steps, $ }) {

    const order = steps.trigger.event.body || {};

    if (!order.customer) {
      $.flow.exit("No valid Shopify order payload received");
    }

    const orderTags = order.tags || "";
    const customerTags = order.customer?.tags || "";
    const totalAmount = parseFloat(order.total_price || 0);

    const hasOrderTag = orderTags.includes("MakeOrder");
    const hasCustomerTag = customerTags.includes("ColdCustomer");
    const amountValid = totalAmount > 2500;

    if (!(hasOrderTag && hasCustomerTag && amountValid)) {
      $.flow.exit("Conditions not satisfied");
    }

    return {
      customerEmail: order.email || "customer@example.com",
      totalAmount
    };
  }
});
