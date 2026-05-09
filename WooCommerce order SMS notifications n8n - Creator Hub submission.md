# WooCommerce order SMS notifications via RCSZilla

Target keyword: n8n WooCommerce SMS notifications

## Description

Send automatic SMS order status updates to WooCommerce customers through RCSZilla.

This workflow listens for WooCommerce order webhook events, extracts the customer billing phone and order details, builds a status-specific SMS message, queues it through RCSZilla, and logs each notification to Google Sheets. Supported order statuses: processing, completed, on-hold, cancelled, refunded, and failed.

## Who This Is For

WooCommerce store owners, ecommerce operators, and n8n builders who want automatic order status SMS without a separate notification plugin or expensive SMS platform.

## What This Workflow Does

1. Listens for WooCommerce order status changes via the WooCommerce trigger node.
2. Extracts billing phone, customer name, order ID, order status, and order total from the WooCommerce payload.
3. Builds a pre-written SMS message matched to the order status: processing, completed, on-hold, cancelled, refunded, or failed.
4. Skips the SMS path when no billing phone is present or the status has no configured template.
5. Queues the SMS through the RCSZilla community node on the sms channel.
6. Logs the order ID, customer, phone, status, message copy, and RCSZilla response to Google Sheets.

## Requirements

- n8n Cloud or self-hosted n8n.
- RCSZilla account and API token.
- Community node package: `n8n-nodes-rcszilla`.
- WooCommerce store with REST API enabled (consumer key and consumer secret).
- Google Sheets credential and a spreadsheet for SMS send logs.
- WooCommerce checkout must collect a billing phone number.

## Setup Guide

1. Install the RCSZilla community node in n8n: `n8n-nodes-rcszilla`.
2. Import the workflow JSON: `WooCommerce order SMS notifications n8n - RCSZilla.json`.
3. Create a WooCommerce credential in n8n:
   - Store URL: your WooCommerce store URL
   - Consumer Key: from WooCommerce → Settings → Advanced → REST API
   - Consumer Secret: from WooCommerce → Settings → Advanced → REST API
4. Assign the WooCommerce credential to the WooCommerce Order Updated trigger node.
5. Create and assign the RCSZilla API credential to the RCSZilla node.
6. Create a Google Sheet with these columns: `order_id`, `customer`, `phone`, `status`, `total`, `sms_message`, `rcszilla_response`.
7. In the Google Sheets node, replace `PASTE_GOOGLE_SHEET_ID` with your spreadsheet ID and confirm the sheet name.
8. Activate the workflow. n8n registers the webhook in WooCommerce automatically.
9. Trigger a test order status change (for example, move a test order from pending to processing) to confirm the SMS is queued and the log row is written.

## Customization

- Edit the message templates in the Prepare SMS Notification Data code node to match your store tone.
- Add or remove order statuses from the messages object in the code node.
- Add a suppression-list lookup before RCSZilla if you maintain a separate opt-out list.
- Replace the Google Sheets node with Airtable, Notion, or a webhook to your own logging endpoint.
- Add a second workflow that polls RCSZilla delivery status after queueing to track failures.

## Compliance Notes

Only send SMS to customers who opted in to SMS marketing at checkout or elsewhere. WooCommerce does not collect SMS consent by default — confirm your consent mechanism before enabling this workflow for live orders.

Keep the `Reply STOP to opt out.` text in every message template. Confirm SMS rules, quiet hours, and opt-out handling for every country or region you ship to.

## Troubleshooting

- Webhook is not firing: confirm the workflow is active and that the WooCommerce credential is correctly assigned to the trigger node.
- No phone found: check that WooCommerce collects a billing phone at checkout and that the test order has a billing phone number.
- SMS path is skipped: inspect the Prepare SMS Notification Data node output for `skipReason`.
- RCSZilla node fails: confirm the RCSZilla credential and that the recipient phone number includes a valid country code.
- Google Sheets append fails: confirm the spreadsheet ID, sheet name, credentials, and the required column headers.
