# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Hugo static site for Monroe County ARES/RACES (Amateur Radio Emergency Service), an amateur radio emergency communications organization. The site uses a custom theme with amateur radio-inspired design elements and no external theme dependencies.

## Development Commands

```bash
# Start development server
hugo server --buildDrafts --bind 0.0.0.0 --port 1313

# Build site for production
hugo --minify

# Create new content
hugo new content/<section>/<filename>.md
```

The development server runs on http://localhost:1313 and automatically rebuilds when files change.

Hugo version is pinned via `mise.toml` (currently 0.150.1). Run `hugo --minify` before making a commit — this builds the generated `docs/` folder used for deployment.

## Architecture

### Publishing & Deployment
`publishDir = 'docs'` in hugo.toml — Hugo builds output to `docs/` (likely GitHub Pages). The `docs/` directory is checked into git and must be rebuilt before committing content changes.

### Layout Structure
- **baseof.html**: Base template with complete HTML structure, all CSS inline in a `<style>` tag, and favicon links
- **index.html**: Homepage template with service cards and "Get Involved" callout
- **single.html**: Individual page template with conditional content blocks keyed on `.File.BaseFileName` (adds special sections for `donations`, `contact`, and `about` pages)
- **calendar.html**: Custom layout for the calendar page with embedded Google Calendar iframe and ICS copy button

### Shortcodes
- **`{{< leadership >}}`**: Renders a leadership table from `params.leadership` data in `hugo.toml`. Used in `about.md`. Leadership data (names, roles, call signs) lives in `hugo.toml`, not in content files.
- **`{{< mailing-address >}}`**: Renders the organization's mailing address from `params.mailingAddress.lines` in `hugo.toml`. Used in `about.md` and `donations.md`. To update the address, edit the `lines` array in `hugo.toml`.
- **`{{< email secretary >}}`** / **`{{< email webmaster >}}`**: Renders a mailto link for an address from `params.emails` in `hugo.toml`.
- **`{{< meeting-exceptions >}}`**: Renders the months with no monthly meeting from `params.meetings.exceptions` in `hugo.toml`.
- **`{{< social-links >}}`**: Renders the social media list from `params.social` entries in `hugo.toml`, plus the Google Calendar subscribe link derived from `params.calendar.id`.

Layouts can't use shortcodes; templates read the same `site.Params.*` values directly (footer links in `baseof.html`, meeting schedule in `index.html`, calendar embed/ICS URLs in `calendar.html` via `params.calendar.id`, and the `calendar-subscribe-url.html` partial for the Google Calendar subscribe link).

### Content Organization
- **_index.md**: Homepage content and organization overview
- **about.md**: Organization info, leadership table (via shortcode), membership, and contact details
- **donations.md**: Financial support information and donation methods
- **resources.md**: Links to Google Drive operational documents
- **training.md**: Recommended FEMA and ARRL training courses
- **calendar.md**: Uses custom `layout: calendar` to render Google Calendar embed

All membership/joining references should include email instructions to contact `secretary@monroecountyemcomm.org`.

### Styling System
Uses CSS custom properties defined in baseof.html with amateur radio color scheme:
- `--navy-blue: #222983` (primary brand color from logo)
- `--light-blue: #86cbf2` (secondary brand color from logo)
- `--radio-red: #cc0000` (accent color)

Responsive design with grid layouts for service cards and leadership information.

### Navigation
Menu structure defined in hugo.toml with weighted navigation items. Site uses semantic HTML with accessibility considerations.

## Content Guidelines

- Organization name: "Monroe County ARES/RACES"
- Tagline: "Monroe County Amateur Radio Emergency Service - Emergency Communications for Public Service"
- Contact email for membership: secretary@monroecountyemcomm.org
- Meeting schedule: Monthly except July, August, December
- Leadership titles use full names and amateur radio call signs in parentheses
- To update leadership, edit `[[params.leadership]]` entries in `hugo.toml`

## Brand Identity

Professional emergency services aesthetic with amateur radio elements. Uses gradient headers, card-based layouts, and emergency service color scheme. The logo contains both "MONROE COUNTY" and "EMERGENCY" text in circular arrangement with "AMATEUR RADIO SERVICE" text.
