# rokt_gtm_screen_view_event

## Project Overview

This repository contains a Google Tag Manager (GTM) custom tag template for logging
mParticle Screen View events. It enables partners to configure and fire screen view
events through Google Tag Manager, integrating with the mParticle by Rokt SDK. The
template is published to the Google Community Template Gallery.

Resident expert: Alex Sapountzis (alex.sapountzis@rokt.com).

## Architecture

This is a single-file GTM tag template (`.tpl` format) with no build system or
runtime infrastructure. The template runs in GTM's sandboxed JavaScript environment
inside a user's browser.

**Data flow:**

1. GTM fires the tag based on a configured trigger.
2. The template collects configured parameters (screen name, URL, custom attributes,
   custom flags, Google Analytics flags).
3. It calls `mParticle.logPageView()` via GTM's `callInWindow` sandbox API.
4. The mParticle SDK (which must already be initialized) processes the screen view
   event.

## Tech Stack

- **Language:** GTM Sandboxed JavaScript (not standard JS — uses GTM `require()` for
  `logToConsole` and `callInWindow`)
- **Platform:** Google Tag Manager (Web container)
- **SDK dependency:** mParticle by Rokt SDK (must be initialized before this tag fires)
- **License:** Apache 2.0

## Template Parameters

The tag template exposes the following configurable fields in the GTM UI:

| Parameter | Type | Required | Description |
|---|---|---|---|
| `screenName` | Text | Yes | Generic page name (e.g., "Product Detail Page") |
| `url` | Text | Yes | Page URL, added as a custom attribute |
| `enableGA` | Select (Yes/No) | Yes | Whether to populate GA custom flags |
| `singlePageApp` | Select (Yes/No) | Conditional | If GA enabled, adds `Google.Page` and `Google.Title` flags |
| `googleTitle` | Text | Conditional | `Google.Title` value for SPA tracking |
| `customAttributes` | Table (key/value) | No | Additional attributes for the screen view event |
| `customFlags` | Table (key/value) | No | Additional custom flags for the mParticle event |

## Development Guide

### Prerequisites

- A Google Tag Manager account with a web container
- Access to the [GTM Template Editor](https://tagmanager.google.com/)
- The mParticle by Rokt SDK initialization tag must be configured in the same GTM
  container

### Making Changes

1. Edit `template.tpl` directly — it contains the template metadata, parameters,
   sandboxed JS code, web permissions, and tests in a single file.
2. Import the modified `.tpl` file into your GTM workspace for testing.

### Testing

Follow the [GTM Wrapper testing guide](https://github.com/ROKT/gtm_wrapper/tree/master/docs/guides/how-to-test.md)
to set up the Testing Playground and validate template changes. The template also
includes inline test scenarios in the `___TESTS___` section of `template.tpl`.

### Deployment

Follow [Google's template update process](https://developers.google.com/tag-platform/tag-manager/templates/gallery#update_your_template)
to publish changes to the Community Template Gallery.

## Project Structure

| File | Purpose |
|---|---|
| `template.tpl` | Complete GTM tag template (metadata, parameters, JS code, permissions, tests) |
| `metadata.yaml` | Template Gallery metadata (homepage, documentation URL, version history) |
| `LICENSE` | Apache 2.0 license |
| `README.md` | Project documentation |
| `.gitignore` | Ignores `.DS_Store` |

## Maintaining This Document

When making changes to this repository that affect the information documented here
(template parameters, SDK dependencies, deployment process, etc.),
please update this document to keep it accurate. This file is the primary reference
for AI coding assistants working in this codebase.
