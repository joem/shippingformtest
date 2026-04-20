# Shipping Form

A single-page employee-facing shipping form for a retail book/media store with multiple locations. Staff fill it out when processing customer shipments, then print it to produce a shipping label summary and shipper worksheet.

Built with plain HTML, CSS, and [Alpine.js](https://alpinejs.dev/) — no build step, no package manager, no server required.

## Usage

Open `index.html` directly in a browser. Alpine.js is loaded from the jsDelivr CDN, so an internet connection is required on first load (or until the browser caches the script).

## Rate Lookup Assets

Four JavaScript files must be present in the `assets/` folder for shipping rate and zone lookups to work. They are not included in this repository and must be added manually:

| File | Purpose |
|------|---------|
| `assets/media_mail.js` | Defines `mediaMailRates` and `getMediaMailRate(weightLbs)` |
| `assets/usps_zones.js` | Defines `uspsZones` and `getZone(destZip)` |
| `assets/priority_mail_retail.js` | Defines `priorityMailRetailRates` and `getPriorityMailRetailRate(weightLbs, zone)` |
| `assets/usps_ground_advantage.js` | Defines `uspsGroundAdvantageRates` and `getUspsGroundAdvantageRate(weightLbs, zone)` |

If these files are absent, the form still works — shipping method rate displays will simply be blank.

## Configuration

Two constants in `index.html` are intended to be edited by hand when needed. Both live in the same small inline `<script>` block just above the Alpine script tag:

```js
const PRIORITY_MAIL_RATE_MULTIPLIER = 0.77;    // 23% discount off retail
const GROUND_ADVANTAGE_RATE_MULTIPLIER = 0.68; // 32% discount off retail
```

These multipliers approximate the Pirate Ship discounted rates and may need adjustment when USPS rates change. Find them with a text search for either constant name.

## Form Fields

| Field | Notes |
|-------|-------|
| Employee Initials | Auto-uppercased |
| Store | Radio: 112th / Broadway / LIC / Pittsford |
| Date | Auto-filled to today; editable |
| Ship-To Name | Recipient name for the package |
| Shipping Address | ZIP code is parsed from this field for Ground Advantage and Priority Mail zone lookups |
| Customer Name | The customer placing the order |
| Customer Email | At least one of email/phone required; required if Email Tracking = Yes |
| Phone # | At least one of email/phone required |
| Email Tracking? | Yes / No |
| Package Weight | Pounds + ounces + packaging ounces (packaging defaults to 6 oz) |
| Shipping Method | USPS Media Mail / USPS Ground Advantage / USPS Priority / UPS Ground |
| Notes/Special Requests | Optional free-text notes |
| Send Receipt with Shipment? | Yes / No |

## Print View

Clicking **Print** validates the form and opens the browser print dialog. The printed output includes:

- Method banner (large, all-caps) for non-Media Mail shipments
- Initials, store, date, and time in the header
- Ship-To address block in a monospace font
- Summary of all filled fields, with the shipping rate appended to the Method row where applicable
- A **For shipper use only** box with write-in fields for Tracking #, Shipping Date, and Shipper Initials
- A **Receipt #** write-in field

The form fields themselves are hidden in print; only the summary is shown.

## Version

The version string (`vYYYY.MM.DD`) in the page header is updated by Claude Code at the start of each development session to reflect the date work was performed (America/New_York timezone).
