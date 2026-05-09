# n8n OpenCart SMS notifications - RCSZilla

## Name

Send OpenCart order and delivery SMS notifications with RCSZilla

## Description

n8n OpenCart SMS notifications workflow for sending customer order and delivery updates through RCSZilla.

This workflow gives an OpenCart store a simple webhook endpoint for order status changes. When OpenCart posts an order update, n8n normalizes the payload, checks the customer phone number and SMS consent, maps the order status to a clear SMS message, queues the message in RCSZilla, and returns a JSON response to OpenCart.

RCSZilla also offers an official OpenCart plugin for standard store SMS and WhatsApp notifications. Use this n8n template when you want custom routing, extra validation, CRM updates, logs, or multi-step automations around OpenCart order events.

Use it for OpenCart order confirmations, shipped SMS alerts, out-for-delivery messages, delivered notifications, canceled order notices, failed payment alerts, and refund updates.

## Who is this for?

This template is useful for OpenCart store owners, ecommerce agencies, and n8n builders who need transactional SMS notifications without building a full custom notification service.

It works best when OpenCart already has an extension, event hook, or small custom module that can POST order status updates to an external webhook.

## What this workflow does

1. Receives an OpenCart order event through an n8n webhook.
2. Reads common OpenCart-style fields like `order_id`, `order_status`, `first_name`, `telephone`, `order_date`, `order_total`, `shipping_method`, `tracking_number`, and `tracking_url`.
3. Accepts common alternate field names like `orderStatus`, `phone`, `customer.telephone`, `trackingNumber`, and `trackingUrl`.
4. Normalizes status names such as `pending`, `processing`, `shipped`, `out_for_delivery`, `complete`, `delivered`, `cancelled`, `failed`, and `refunded`.
5. Checks for SMS consent before sending.
6. Skips customers marked as opted out.
7. Builds a short order or delivery SMS with a STOP opt-out line.
8. Supports `sms` or `whatsapp` through the optional `channel` field.
9. Queues the message through RCSZilla.
10. Returns a queued or skipped JSON response to the OpenCart caller.

## Setup Guide

1. Install the RCSZilla community node in n8n: `n8n-nodes-rcszilla`.
2. Create RCSZilla credentials in n8n.
3. Import the workflow into n8n.
4. Activate the workflow.
5. Copy the production URL from the `OpenCart order webhook` node.
6. If the official RCSZilla OpenCart plugin already covers your use case, use it directly. If you need n8n logic, configure an order-status extension, event hook, or custom module to POST order updates to the webhook URL.
7. Send a test payload with an internal phone number and `sms_consent` set to `true`.
8. Confirm that unsupported statuses, missing consent, missing phone numbers, and opted-out customers return a skipped response.
9. Test a supported status such as `shipped`.
10. Activate the OpenCart integration only after the test message and skipped responses behave correctly.

## Example webhook payload

```json
{
  "order_id": "12345",
  "order_status": "shipped",
  "first_name": "Maria",
  "telephone": "+40700000000",
  "sms_consent": true,
  "store_name": "Demo Store",
  "order_total": "129.00 RON",
  "shipping_method": "Courier",
  "tracking_number": "AWB123",
  "tracking_url": "https://tracking.example/AWB123",
  "channel": "sms"
}
```

## Supported statuses

The workflow sends SMS for these normalized statuses:

- `processing`
- `shipped`
- `out_for_delivery`
- `delivered`
- `canceled`
- `failed`
- `refunded`

Common aliases are included, including `processed`, `paid`, `dispatched`, `dispatch`, `out for delivery`, `complete`, `completed`, `cancelled`, `voided`, and `refund`.

If your OpenCart store sends numeric order status IDs, edit the `statusAliases` object in the `Prepare OpenCart SMS notification` node and map your store-specific IDs to the supported names.

## Compliance notes

Use this workflow only for customers who agreed to receive order updates by SMS.

The workflow requires one of these fields to be true before sending: `sms_consent`, `smsConsent`, `transactional_sms_allowed`, `transactionalSmsAllowed`, `consent`, `opt_in`, `customer.sms_consent`, or `customer.transactional_sms_allowed`.

It skips records marked with opt-out fields such as `sms_opt_out`, `smsOptOut`, `opt_out`, `do_not_sms`, `unsubscribe`, `customer.sms_opt_out`, or `customer.opt_out`.

Keep the STOP opt-out text unless your legal process requires different wording. Your store is responsible for consent records, message content, and SMS compliance in the regions where you operate.
