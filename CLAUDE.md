# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A single-page employee-facing shipping form for a retail book/media store with multiple locations. Staff fill it out when processing customer shipments. Built with plain HTML, CSS, and Alpine.js — no build step, no other JS frameworks. The entire application lives in `index.html`.

## Architecture

All logic, styling, and markup are in one file:

- **Alpine.js** (loaded from jsDelivr CDN) drives reactivity via `x-data`, `x-model`, `x-show`, and `x-text` — no separate JS files.
- The `x-data` object on the root `.card` div holds `form` (live input state) and `submitted` (snapshot set on submit). Submitting shallow-copies `form` into `submitted`, which makes the summary panel appear via `x-show`.
- There is no backend, build tool, bundler, package manager, or test suite.

## Form Fields

The `form` state object has these keys (in order of appearance):

| Key | Type | Required |
|-----|------|----------|
| `initials` | text | yes |
| `store` | radio: 112th / Broadway / LIC / Pittsford | yes |
| `date` | date | yes |
| `name` | text (ship-to name) | yes |
| `address` | textarea | yes |
| `email` | text (customer email) | no |
| `phone` | tel | no |
| `weight` | number (kg) | yes |
| `method` | radio: USPS Media Mail / USPS Ground Advantage / USPS Priority / UPS Ground | yes |
| `emailTracking` | radio: Yes / No | yes |
| `specialRequests` | textarea | no |
| `sendReceipt` | radio: Yes / No | yes |

## CSS Patterns

Two radio button layouts are used:

- **`.radio-group`** — horizontal flex row, used for short single-word options (Store, Email tracking?, Send receipt?).
- **`.radio-option` + `.radio-option-text`** — vertical stacked layout with a bold label `<span>` and a `<small>` description line, used for Shipping Method where each option has a description.

## Development

Open `index.html` directly in a browser — no server needed. Alpine.js requires an internet connection (CDN).

## Constraints

- Do not introduce JS frameworks beyond Alpine.js.
- Do not add a build step or package manager unless explicitly requested.
