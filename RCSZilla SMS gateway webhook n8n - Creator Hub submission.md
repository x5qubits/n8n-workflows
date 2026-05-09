# Create an n8n SMS gateway webhook with RCSZilla

Target keywords: n8n SMS gateway, Android SMS gateway n8n, RCSZilla n8n

## Description

Create a lightweight SMS gateway API in n8n using the RCSZilla community node.

This workflow exposes a webhook endpoint that accepts JSON requests from your CRM, booking tool, ecommerce store, support desk, or internal app. It validates recipient phone number, message body, channel, and consent, then queues the message through RCSZilla. It returns a clean JSON response for both successful and rejected requests.

## Who This Is For

Automation builders and small businesses that want to send consent-based SMS from n8n using RCSZilla and a connected Android device or configured provider route.

## What This Workflow Does

1. Receives a POST request through an n8n Webhook node.
2. Accepts `to`, `message`, `consent`, and optional metadata.
3. Normalizes the phone number.
4. Defaults the channel to `sms`.
5. Allows `sms` or `whatsapp` as supported RCSZilla channels.
6. Rejects requests without phone number, message, consent, or valid channel.
7. Queues valid messages through RCSZilla.
8. Responds with JSON showing whether the request was queued or rejected.

## Requirements

- n8n Cloud or self-hosted n8n.
- RCSZilla account and API token.
- Community node package: `n8n-nodes-rcszilla`.
- A connected RCSZilla Android gateway or configured RCSZilla provider route.
- A source app that can send HTTP POST requests.

## Setup Guide

1. Install the RCSZilla community node in n8n: `n8n-nodes-rcszilla`.
2. Import the workflow JSON: `RCSZilla SMS gateway webhook n8n - Android SMS.json`.
3. Create and assign the RCSZilla API credential to the RCSZilla node.
4. Activate the workflow.
5. Copy the production webhook URL from the Webhook node.
6. Send a test POST request:

```json
{
  "to": "+40700000000",
  "message": "Your appointment is tomorrow at 10:00.",
  "consent": true,
  "source": "Booking CRM",
  "referenceId": "appt_123"
}
```

7. Confirm the response returns `status: queued`.
8. Confirm the message appears in RCSZilla and is delivered by your configured route.

## Optional Fields

- `channel`: `sms` or `whatsapp`. Defaults to `sms`.
- `scheduledAt`: ISO date/time or another date value parseable by JavaScript.
- `source`: name of the calling app.
- `referenceId`: CRM, ticket, appointment, or order ID.

## Customization

- Add an API key check before the validation node for production use.
- Add a Google Sheets or database log after RCSZilla for audit history.
- Restrict the workflow to `sms` only by removing `whatsapp` from the allowed channels.
- Add message templates for appointment reminders, order updates, support alerts, or internal notifications.
- Add a delivery-status workflow using RCSZilla `Get Queue Status` if you need follow-up monitoring.

## Compliance Notes

Use this workflow only for transactional or consent-based messaging. Do not use it for unsolicited outreach.

For production, protect the webhook with authentication or place it behind a trusted internal service. Keep consent records in the source system and respect opt-out requests, quiet hours, and regional SMS rules.

## Troubleshooting

- `to is required`: include a valid phone number with country code.
- `message is required`: include a non-empty message field.
- `consent must be true`: pass `consent: true` only when the recipient has agreed to receive the message.
- RCSZilla fails: confirm the RCSZilla API credential and gateway/provider setup.
- Webhook is not reachable: activate the workflow and use the production webhook URL, not the test URL.

