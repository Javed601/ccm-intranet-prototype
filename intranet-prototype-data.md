# CCM Europe Intranet — Collected Requirements & Notes

Date: 2026-03-05
From: Michael / EHS team

## Organization & Regions
- Company: CCM Europe
- Regions: Netherlands (NL), Germany (DE), United Kingdom (UK), Romania (RO)

## Primary Goals
- Communicate with employees across regions
- Provide a clean, permission-aware homepage (not exposing all departmental content to everyone)
- Centralize news, tools, and document libraries while preserving scoped access
- Support multi-language content (EN/DE/NL/RO) and region-aware filtering

## Current content & permission model (as described)
- Internal (private workspace)
  - Purpose: Confidential docs, internal procedures, draft documents
  - Your Department: Read/Write
  - Others: No access

- Public
  - Purpose: Publish department announcements, policies, FAQs, training
  - Your Department: Read/Write
  - All Employees: Read only

- Share (organization-wide, folder-level access)
  - Purpose: Cross-department projects, shared templates, company initiatives
  - Folder-based access control

## Existing Tools & Lists
- Contract Management (multi-department)
- Safety Alert Manager
- Several department-specific lists & document libraries in SharePoint used actively

Notes:
- Tools should remain managed per-site with per-list/library permissions.
- Quick Links on the homepage should be permission-aware (display only tools the user has access to).

## Requirements / Constraints
1. Permission-first design: homepage should not expose departmental pages/content to users lacking access.
2. Group-based visibility: HR or admins should be able to see department pages if they’re in the corresponding group.
3. Multi-language support: content may be authored in local language; the homepage should show appropriate language or fallback to global language.
4. Regional scoping: news/items can be region-specific (NL / DE / UK / RO) or global (EU).
5. Data sources: SharePoint lists (News, QuickLinks, Events), document libraries, Azure AD for user/profile/groups, Exchange for personal events.
6. Maintainability: styling, fonts, and color tokens should be compatible with SharePoint (so prototypes are easily re-created in SharePoint/modern pages or SPFx).

## Suggested SharePoint mapping (lists & fields)
- News (list "NewsFeed")
  - Fields: Title (multi-lang via separate fields or JSON), Excerpt, Body/Link, Category, PublishedDate, Author, Language, Region, AudienceGroups, ImageUrl, TargetAudience

- QuickLinks (list "QuickLinks")
  - Fields: Title, Url, IconClass (or Image), Region, AllowedGroups (multi-value), SortOrder

- Tools / Apps register (optional centralized list)
  - Fields: AppName, Url, Owners, AllowedGroups, Regions, Description, Icon

- Events (list "Events")
  - Fields: Title, StartDate, EndDate, Location, Region, Language, AudienceGroups, Link

## UX / Design notes (prototype decisions)
- Use a fixed topbar with quick navigation and search.
- Right sidebar: permission-aware Quick Links; main column: news feed + KPIs.
- Keep components modular and tokenized (CSS variables) for easy translation into SharePoint Themes or SPFx.
- Use audience targeting (SharePoint feature) plus group membership checks (Azure AD) to decide visibility.

## Localization strategy
- Option A (preferred): Author content per language in SharePoint (fields per language or separate language pages) and expose `Language` metadata; client filters by `user.preferredLanguage` with fallback to EN.
- Option B: Store content once with translations as JSON; render the matching language.

## Permission strategy
- Use SharePoint permission groups + Microsoft 365 groups.
- For quick preview in prototype: simulate via allowedGroups arrays.

## Next steps / Implementation options
- Export the prototype mapping to actual SharePoint lists (create templates and fields).
- Provide PnP / REST scripts to provision lists and columns.
- Create an SPFx web part (React) to fetch lists and respect audience targeting + language/region filters.

## Presenting to management
- Deliver an HTML prototype (intranet-prototype.html) showing region & language controls and permission-aware quick links.
- Provide a short migration plan: how to map current departmental libraries into the new structure and apply folder-level permissions + audience targeting.

---

If you want, I can also:
- Generate PnP provisioning JSON for the `NewsFeed` and `QuickLinks` lists and fields.
- Create REST / Graph sample queries to fetch items filtered by region/language and audience.
- Start an SPFx scaffold to produce a deployable web part (React) for SharePoint Online.

Tell me which of the above you want next.