# Recover Shopify abandoned carts with AI SMS via RCSZilla

Target keyword: Shopify abandoned cart SMS n8n

## Description

Recover abandoned Shopify checkouts with concise AI-written SMS reminders sent through RCSZilla.

This workflow checks Shopify abandoned checkout data, waits one hour, verifies that the shopper is still abandoned, confirms that usable SMS contact data is present, generates a short recovery message, queues it through RCSZilla, and logs the result to Google Sheets.

## Who This Is For

Shopify merchants, ecommerce operators, and n8n builders who want a consent-based SMS recovery step without wiring a separate SMS provider into every workflow.

## What This Workflow Does

1. Starts manually for testing. Replace with a Schedule Trigger for production.
2. Fetches abandoned checkouts from Shopify.
3. Waits one hour so customers have time to finish checkout naturally.
4. Rechecks Shopify to confirm the checkout is still abandoned.
5. Extracts the customer name, phone number, checkout recovery URL, cart value, and SMS consent signal.
6. Skips the SMS path when phone, recovery URL, or SMS consent is missing.
7. Uses an AI Agent to write a short abandoned checkout SMS.
8. Queues the SMS with the RCSZilla community node.
9. Logs the customer, phone, recovery URL, SMS copy, and RCSZilla response to Google Sheets.

## Requirements

- n8n Cloud or self-hosted n8n.
- RCSZilla account and API token.
- Community node package: `n8n-nodes-rcszilla`.
- Shopify Admin API access that can read abandoned checkouts.
- OpenAI credential for the SMS writer model.
- Google Sheets credential and a spreadsheet for send logs.

## Setup Guide

1. Install the RCSZilla community node in n8n: `n8n-nodes-rcszilla`.
2. Import the workflow JSON: `Smart Shopify agent_ AI-powered abandoned cart recovery(1).json`.
3. Open both Shopify HTTP Request nodes and replace `your-store.myshopify.com` with your store domain.
4. Create an HTTP Header Auth credential in n8n:
   - Header name: `X-Shopify-Access-Token`
   - Header value: your Shopify Admin API access token
5. Assign that HTTP Header Auth credential to both Shopify HTTP Request nodes.
6. Create and assign the RCSZilla API credential to the RCSZilla node.
7. Create and assign the OpenAI credential to the SMS Writer Model node.
8. Create a Google Sheet with these columns: `customer`, `phone`, `checkout_url`, `sms_message`, `rcszilla_response`.
9. In the Google Sheets node, replace `PASTE_GOOGLE_SHEET_ID` with your spreadsheet ID and confirm the sheet name.
10. Run the workflow manually against one internal test abandoned checkout.
11. After testing, replace the Manual Trigger with a Schedule Trigger, for example every 30 or 60 minutes.

## Customization

- Adjust the Wait node from one hour to your preferred recovery delay.
- Edit the AI Agent prompt to change tone, discount wording, or maximum SMS length.
- Add a second RCSZilla status-check workflow if you want delivery monitoring after queueing.
- Add a suppression-list lookup before RCSZilla if you already keep opt-outs outside Shopify.

## Compliance Notes

Only use this workflow for customers who opted in to SMS marketing. Keep the opt-out language in the SMS prompt: `Reply STOP to opt out.`

Do not use this workflow for unsolicited bulk messaging. Confirm SMS consent, abandoned checkout messaging rules, quiet hours, and opt-out handling for every country or region you send to.

## Troubleshooting

- Shopify request returns 401: check the HTTP Header Auth credential and Admin API token.
- No checkouts found: confirm Shopify has current abandoned checkouts and that the store domain is correct.
- SMS path is skipped: inspect the Prepare SMS Recovery Data node output for `skipReason`.
- RCSZilla node fails: confirm the RCSZilla credential and that the recipient phone number includes a valid country code.
- Google Sheets append fails: confirm the spreadsheet ID, sheet name, credentials, and required columns.

