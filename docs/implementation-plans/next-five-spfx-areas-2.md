# Next Five SPFx Example Areas — Research and Ranked Plans

**Research date:** 2026-08-30
**Repository:** `pnp/sp-dev-fx-webparts`

## Method

The live GitHub tree contains 466 sample directories. Category counts were calculated from directory names, then compared with Microsoft Graph and SPFx documentation. Existing samples were treated as evidence of coverage, not proof that every adjacent use case is solved.

Useful baseline counts:

- Accessibility: 0 named samples
- Compliance: 0
- Retention/records: 0
- Translation/multilingual: 0
- Workflow: 0
- Approval: 2
- Booking: 2
- Calendar: 10
- Search: 5
- Report: 2
- Analytics: 0

The ranking favors a practical enterprise need, a clear SPFx implementation boundary, read-only safety, and distinctiveness from the first ten samples.

## Ranked opportunities

### 1. Accessibility and Content Quality Auditor

**Gap:** No sample is explicitly named for accessibility auditing, while content owners need a lightweight way to identify missing alternative text, empty links, heading-order issues, and non-descriptive document/page metadata. This is not a duplicate of a document metadata reviewer: it evaluates rendered/content quality rules and gives remediation links.

**Proposed sample:** `react-accessibility-content-auditor`

**Scope:** Read-only scan of a bounded SharePoint page or document-library result set. Start with deterministic rules that can be evaluated from SharePoint fields and page HTML/links; report rule, severity, item, evidence, and remediation URL. No automated mutation.

**Implementation plan:**

- Use SharePoint REST/PnPjs to read bounded pages or list items.
- Parse only allow-listed content fields; cap item count and text size.
- Implement pure rule functions with tests for alt text, link text, heading sequence, empty values, and severity.
- Add loading, empty, partial-result, access-denied, retry, keyboard, and responsive states.
- Document that this is a heuristic audit, not WCAG conformance certification.

**Sources:**
- SPFx client-side web parts: https://learn.microsoft.com/en-us/sharepoint/dev/spfx/web-parts/overview-client-side-web-parts
- SPFx field customizer/content rendering pattern: https://learn.microsoft.com/en-us/sharepoint/dev/spfx/extensions/get-started/building-simple-field-customizer
- Modern SharePoint customization boundaries: https://learn.microsoft.com/en-us/sharepoint/dev/solution-guidance/modern-experience-customizations

### 2. Multilingual Intranet Navigation and Content Switcher

**Gap:** No sample is explicitly named for translation or multilingual content. Existing navigation/hub samples do not demonstrate language-aware labels, locale selection, fallback behavior, or translation-source configuration.

**Proposed sample:** `react-multilingual-intranet-switcher`

**Scope:** Read-only language switcher for configured navigation/content links. Support a small JSON translation map, current locale detection, explicit language selection, fallback to default language, and safe same-tenant links. Do not claim automatic translation or call an external translation service.

**Implementation plan:**

- Define a bounded JSON schema for locale, label, URL, and optional description.
- Validate locale tags and same-tenant URL policy before rendering.
- Use browser locale only as a default; provide an accessible explicit selector.
- Test fallback, malformed JSON, duplicate locales, unsafe URLs, and keyboard selection.
- Document how this differs from SharePoint multilingual publishing and does not replace translation workflows.

**Sources:**
- Microsoft Graph locale information resource: https://learn.microsoft.com/en-us/graph/api/resources/localeinfo?view=graph-rest-1.0
- SPFx client-side web parts: https://learn.microsoft.com/en-us/sharepoint/dev/spfx/web-parts/overview-client-side-web-parts

### 3. Retention and Records Review Dashboard

**Gap:** No sample is explicitly named for compliance, retention, or records review. Document browsing and metadata review do not provide a read-only view of retention labels, record status, review dates, or missing governance fields.

**Proposed sample:** `react-retention-records-review`

**Scope:** Read-only review of configured document-library fields representing retention/record metadata. Surface missing review dates, expired review dates, unlabeled records, and inconsistent status combinations. Never apply labels, delete content, or alter compliance settings.

**Implementation plan:**

- Use SharePoint REST with an explicit `$select` allow-list and bounded `$top`.
- Make field mapping configurable but validate field names and supported types.
- Normalize dates without wall-clock-dependent tests.
- Add pure compliance-rule tests and classify access-denied/throttling/invalid-field errors.
- Include a prominent disclaimer: results are a review aid and not a complete Purview compliance determination.

**Sources:**
- Microsoft Graph permission resource (relevant to access boundaries): https://learn.microsoft.com/en-us/graph/api/resources/permission?view=graph-rest-1.0
- Modern SharePoint customization boundaries: https://learn.microsoft.com/en-us/sharepoint/dev/solution-guidance/modern-experience-customizations
- SPFx client-side web parts: https://learn.microsoft.com/en-us/sharepoint/dev/spfx/web-parts/overview-client-side-web-parts

### 4. Workflow and Approval Status Dashboard

**Gap:** Two approval-related samples exist, but no explicitly named workflow status dashboard. A read-only cross-list view of pending, overdue, rejected, and completed requests remains a common intranet need distinct from submitting or managing approvals.

**Proposed sample:** `react-workflow-status-dashboard`

**Scope:** Aggregate bounded read-only request records from configured SharePoint lists. Display status, owner, submitted date, due date, age, and safe source link. Use configurable status/date/owner field names; do not approve, reject, assign, or mutate records.

**Implementation plan:**

- Read only configured lists with validated server-relative paths.
- Normalize statuses and dates into a common view model.
- Add filters for status and overdue state, with result bounds and partial-failure handling.
- Test status normalization, overdue calculation with injected reference time, invalid URLs, and error classification.
- Document that it is not a replacement for Power Automate or Microsoft Approvals.

**Sources:**
- Microsoft Graph approval resource: https://learn.microsoft.com/en-us/graph/api/resources/approval?view=graph-rest-1.0
- SPFx client-side web parts: https://learn.microsoft.com/en-us/sharepoint/dev/spfx/web-parts/overview-client-side-web-parts
- Modern SharePoint customization boundaries: https://learn.microsoft.com/en-us/sharepoint/dev/solution-guidance/modern-experience-customizations

### 5. Room and Shared-Resource Capacity Planner

**Gap:** Two booking/room samples exist, but no focused read-only capacity view combining configured rooms/resources, reservations, utilization, and availability across a bounded period. This avoids duplicating a booking transaction UI.

**Proposed sample:** `react-resource-capacity-planner`

**Scope:** Read-only display of configured Microsoft Bookings or SharePoint resource records, with utilization by day/resource and conflict indicators. No reservation creation, update, cancellation, or external credential handling.

**Implementation plan:**

- Prefer SharePoint list data for a permission-simple reference sample; document the alternative Microsoft Graph Bookings API and its consent boundary.
- Validate ISO date ranges, timezone display, resource IDs, and maximum horizon.
- Normalize reservations into deterministic intervals and calculate occupancy/conflicts.
- Test date ranges, overlap edges, timezone formatting, malformed records, empty data, and partial failures.
- Provide accessible list/calendar-like views without relying on color alone.

**Sources:**
- Microsoft Graph Bookings API overview: https://learn.microsoft.com/en-us/graph/api/resources/booking-api-overview?view=graph-rest-1.0
- SPFx client-side web parts: https://learn.microsoft.com/en-us/sharepoint/dev/spfx/web-parts/overview-client-side-web-parts

## Selection rationale

These five fill named repository gaps while avoiding direct repetition of the current batch:

1. Accessibility quality is absent and has clear safe read-only rules.
2. Multilingual navigation is absent and can be useful without inventing an AI translation backend.
3. Retention/records review is absent and complements, rather than duplicates, metadata browsing.
4. Workflow status is distinct from approval submission/management.
5. Capacity planning is distinct from room-booking transactions and can remain read-only.

## Shared implementation guardrails

- One sample per branch and PR, based on `origin/main`.
- SPFx 1.23.2, React 17, Fluent UI v9, PnPjs where appropriate.
- Read-only by default; no create/update/delete calls unless a later plan explicitly changes scope.
- Bound all list/site/date inputs; validate server-relative URLs and same-tenant links.
- Deterministic tests; inject reference dates instead of using wall-clock assertions.
- Run tests, build, lint, production package, gallery metadata validation, diff inspection, and secret scanning before any push.
- Request real SharePoint Online validation and screenshots; never fabricate tenant evidence.
