# Google Sheets SMS marketing automation n8n - RCSZilla

## Name

Run consent-based SMS marketing campaigns from Google Sheets with RCSZilla

## Description

This n8n SMS marketing automation workflow lets you run a consent-based promotional SMS campaign from Google Sheets and queue the messages through RCSZilla.

The workflow reads campaign contacts from a Contacts sheet, validates SMS consent, skips opted-out contacts, deduplicates phone numbers, respects a configurable sending window, spaces queued messages over time, and writes every queued or skipped contact to a Send Log sheet. It also includes a separate opt-out webhook that captures STOP-style replies into an Opt Outs sheet.

Use this template when you need a practical SMS marketing workflow for local businesses, ecommerce stores, agencies, event reminders, seasonal promotions, limited-time offers, customer winback campaigns, or segmented customer updates.

## Who is this for?

This template is useful for marketers, ecommerce operators, agencies, service businesses, and n8n builders who want a lightweight SMS campaign workflow without building a full campaign platform.

It is best for teams that already store customer consent and campaign data in Google Sheets and want n8n to handle validation, personalization, queueing, and logging.

## What this workflow does

1. Starts a campaign manually so you stay in control of each send.
2. Reads rows from a Google Sheets Contacts tab.
3. Builds a personalized SMS from `message_template` or the default template.
4. Checks that the contact has SMS marketing consent.
5. Skips contacts who opted out or were already marked sent, queued, or delivered.
6. Deduplicates phone numbers inside the run.
7. Applies a maximum send limit per run.
8. Adds scheduled spacing between messages before queueing them in RCSZilla.
9. Marks the original Contacts row as `queued` after RCSZilla accepts the message.
10. Logs queued messages to the Send Log tab.
11. Logs skipped contacts and skip reasons to the Send Log tab.
12. Captures STOP, UNSUBSCRIBE, CANCEL, END, and QUIT replies through an opt-out webhook.
13. Saves opt-out records to an Opt Outs tab.

## Setup Guide

1. Install the RCSZilla community node in n8n: `n8n-nodes-rcszilla`.
2. Create Google Sheets credentials in n8n.
3. Create RCSZilla credentials in n8n.
4. Create a Google Sheet with three tabs: `Contacts`, `Send Log`, and `Opt Outs`.
5. In every Google Sheets node, replace `PASTE_SMS_MARKETING_SHEET_ID` with your Google Sheet ID.
6. Add these recommended columns to the `Contacts` tab: `phone`, `first_name`, `last_name`, `consent`, `opt_out`, `segment`, `campaign_status`, `send_after`, `offer_text`, `offer_url`, `message_template`.
7. Add these recommended columns to the `Send Log` tab: `timestamp`, `campaign_id`, `phone`, `first_name`, `segment`, `status`, `scheduled_at`, `message`, `skip_reason`, `source_row`, `rcszilla_response`.
8. Add these recommended columns to the `Opt Outs` tab: `received_at`, `phone`, `keyword`, `message`, `source`.
9. Open the `Prepare campaign messages` node and review the campaign settings: `campaignId`, `segment`, `maxSendsPerRun`, `minSecondsBetweenMessages`, and quiet-hour settings.
10. Add one internal test contact with `consent` set to `yes`.
11. Run `Start SMS campaign manually`.
12. Check the `Send Log` tab before using the workflow with real customers.
13. Connect your inbound SMS or RCSZilla reply handler to the `SMS opt-out webhook` URL if you want automatic opt-out capture.

## Contacts sheet format

Required columns:

- `phone`: recipient number with country code, such as `+40700000000`
- `consent`: use `yes`, `true`, `1`, `subscribed`, or `opted in`
- `campaign_status`: leave blank before sending; the workflow updates it to `queued`

Recommended optional columns:

- `first_name`: used for personalization
- `last_name`: available for templates
- `opt_out`: set to `yes` to skip the contact
- `segment`: used when you configure a campaign segment
- `send_after`: future date/time before the contact becomes eligible
- `offer_text`: offer copy used by the default template
- `offer_url`: landing page or coupon link
- `message_template`: custom message with variables

## Message template variables

The workflow supports these variables:

- `{{first_name}}`
- `{{last_name}}`
- `{{offer_text}}`
- `{{offer_url}}`
- `{{segment}}`
- `{{campaign_id}}`

Example:

`Hi {{first_name}}, your VIP offer is ready: {{offer_url}} Reply STOP to opt out.`

## Compliance notes

Use this workflow only for contacts who gave clear SMS marketing consent.

Do not use purchased lists, scraped phone numbers, or contacts who opted out. Keep the opt-out text in every message. Review SMS marketing rules for your jurisdiction and industry before activating any campaign.

The workflow includes safeguards, but your business is responsible for consent records, opt-out handling, message content, and campaign timing.
