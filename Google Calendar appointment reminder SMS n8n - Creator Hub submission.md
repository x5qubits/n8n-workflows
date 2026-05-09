# Send Google Calendar appointment reminder SMS with RCSZilla

Target keyword: n8n appointment reminder SMS

## Description

Send consent-based appointment reminder SMS messages from Google Calendar using RCSZilla.

This workflow checks tomorrow's Google Calendar appointments every morning, extracts a phone number and SMS consent signal from each event description, builds a concise reminder message, and queues the SMS through RCSZilla. It skips events that do not include a phone number or positive SMS consent.

## Who This Is For

Service businesses that book appointments in Google Calendar and want simple SMS reminders without a separate SMS gateway provider. Good fits include salons, clinics, dentists, consultants, repair shops, real estate teams, and auto service businesses.

## What This Workflow Does

1. Runs once per day at 09:00.
2. Fetches tomorrow's events from Google Calendar.
3. Parses appointment metadata from each event.
4. Requires a phone number and `SMS consent: yes`.
5. Creates a short reminder with the event title, date/time, and location.
6. Queues the SMS through the RCSZilla community node.
7. Routes ineligible events to a skipped summary node so they can be inspected during testing.

## Requirements

- n8n Cloud or self-hosted n8n.
- RCSZilla account and API token.
- Community node package: `n8n-nodes-rcszilla`.
- Google Calendar OAuth credential in n8n.
- Calendar events that include customer phone and SMS consent in the description.

## Setup Guide

1. Install the RCSZilla community node in n8n: `n8n-nodes-rcszilla`.
2. Import the workflow JSON: `Google Calendar appointment reminder SMS n8n - RCSZilla.json`.
3. Create and assign the Google Calendar credential to the Google Calendar node.
4. Create and assign the RCSZilla API credential to the RCSZilla node.
5. In the Google Calendar node, choose the calendar that stores appointments.
6. Add this format to appointment event descriptions:

```text
Name: Maria
Phone: +40700000000
SMS consent: yes
```

7. Run the workflow manually or temporarily set the schedule to a near time for testing.
8. Inspect skipped events. If a reminder is skipped, check that the description has a valid phone number and `SMS consent: yes`.
9. Activate the workflow when testing is complete.

## Customization

- Change the Schedule Trigger time to match your business process.
- Change the Google Calendar time window if you want same-day reminders instead of next-day reminders.
- Edit the Code node message template to match your brand voice.
- Keep sensitive appointment names out of SMS messages for regulated or sensitive services.
- Add a delivery-status workflow later using RCSZilla queue status if you need operational monitoring.

## Compliance Notes

Only send reminders to customers who have explicitly agreed to receive SMS appointment messages. Keep the opt-out text in the SMS: `Reply STOP to opt out.`

Do not use this as an unsolicited marketing workflow. Confirm consent, opt-out handling, message timing, quiet hours, and local SMS rules for every region you send to.

## Troubleshooting

- No events returned: confirm the selected calendar and the tomorrow time window.
- Event is skipped: add `Phone:` and `SMS consent: yes` to the event description.
- RCSZilla fails: confirm the API credential and the phone number country code.
- Message is too specific: make the calendar event title generic or edit the Code node message template.
- Reminders send at the wrong time: check the n8n instance timezone and the Schedule Trigger settings.

