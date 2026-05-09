# Get an SMS when a WooCommerce order is placed via RCSZilla

Target keyword: n8n WooCommerce SMS notifications

## Description

Get an instant SMS on your phone every time a new WooCommerce order is placed.

This is the simplest possible WooCommerce SMS workflow for n8n beginners. Three nodes: a WooCommerce trigger, a message builder with your phone number, and RCSZilla to send the SMS. No customer phone data. No coding. No AI model. Just place a test order and your phone rings.

## Who This Is For

WooCommerce store owners and n8n beginners who want to receive instant order alerts by SMS without installing a plugin or building complex automation. A great first workflow for anyone new to RCSZilla or SMS automation in n8n.

## What This Workflow Does

1. Fires the moment a new WooCommerce order is created.
2. Builds a short SMS message using the order ID, customer name, total, currency, and item names.
3. Sends the SMS to the store owner's phone number via RCSZilla.

## Requirements

- n8n Cloud or self-hosted n8n.
- RCSZilla account and API token.
- Community node package: `n8n-nodes-rcszilla`.
- WooCommerce store with REST API enabled (consumer key and consumer secret).
- Your own mobile phone number with country code.

## Setup Guide

1. Install the RCSZilla community node in n8n: `n8n-nodes-rcszilla`.
2. Import the workflow JSON: `WooCommerce new order SMS alert n8n - RCSZilla.json`.
3. Create a WooCommerce credential in n8n:
   - Store URL: your WooCommerce store URL
   - Consumer Key: WooCommerce → Settings → Advanced → REST API
   - Consumer Secret: WooCommerce → Settings → Advanced → REST API
4. Assign the WooCommerce credential to the WooCommerce New Order trigger node.
5. In the Build Alert Message node, replace `+1234567890` with your own mobile number including country code.
6. Create and assign the RCSZilla API credential to the RCSZilla node.
7. Activate the workflow. n8n registers the webhook in WooCommerce automatically.
8. Place a test order in your store and wait for the SMS to arrive.

## Customization

- Edit the message text in the Build Alert Message Set node to include different order fields.
- Add an IF node before RCSZilla to only alert for orders above a certain value.
- Route high-value orders to a different phone number by adding a second branch.
- Add a Google Sheets append node after RCSZilla to keep a log of every alert sent.
- Upgrade to the companion workflow `WooCommerce order SMS notifications n8n - RCSZilla.json` when you are ready to also send SMS to customers on status changes.

## Compliance Notes

This workflow sends SMS to the store owner only. No customer phone numbers are used or stored. No opt-in or consent handling is required for internal business alert messages sent to yourself.

## Troubleshooting

- Webhook is not firing: confirm the workflow is active and that the WooCommerce credential is correctly assigned to the trigger node.
- SMS not arriving: confirm the recipient phone number in Build Alert Message includes a valid country code (for example +44 or +1).
- RCSZilla node fails: confirm the RCSZilla credential and check the RCSZilla queue dashboard for error details.
- Order data fields are empty: open the WooCommerce New Order trigger node output and inspect the available fields — field names may differ slightly between WooCommerce versions.
