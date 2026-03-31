# Arc | Clinical Supply Chain — Oak Avenue Pharma

## Overview
A complete, fully interactive clinical supply chain management dashboard built as a single static HTML file. Covers the full lifecycle from patient screening to drug administration, with role-based access for 6 roles and 21 CFR Part 11 e-signature compliance throughout.

## Architecture
- **Single file app**: `index.html` — all HTML, CSS, and JavaScript in one file
- **Server**: Python 3 HTTP server (`python3 -m http.server 5000 --bind 0.0.0.0`)
- **No build system, no dependencies, no packages required**

## Design System
- Navy: `#020434` (primary brand, sidebar, nav, panel headers)
- Pink: `#E60F65` (accent, CTA buttons, active states, links)
- Lavender: `#EDF0FA` (page backgrounds, info panels)
- MARK AI: dark purple gradient (`#020434 → #1a1674`) with purple accent `#7C3AED`

## Modules (All Sprints Complete)

### Sprint 1 — SARF
- Patient screening request forms with full lifecycle (Draft → Submitted → Under Review → Approved/Rejected)
- Role-gated actions (Clinic Site submits, Clinical Ops approves)
- 21 CFR Part 11 e-signature on approvals/rejections
- Priority-flagged rows (At-Risk, Urgent), tab filters, search/filter
- SARF detail side panel with audit trail, cross-links to Dose Orders

### Sprint 2 — Dose Orders
- Dose order submission by Clinic Site from approved SARFs
- MARK AI dose calculation display (BSA-adjusted, variance check)
- Clinical Ops approval via e-signature
- Status lifecycle: Draft → Submitted → Approved → Batch Assigned → Shipped → Administered
- DO detail panel with step progress bar, MARK AI variance, audit trail, cross-links to SARF and Batch

### Sprint 3 — Batch Planning + MARK AI
- Batch list (7 batches across lifecycle stages)
- MARK AI Queue tab: human-in-the-loop allocation confirmations with full reasoning display
- MARK AI Confirm modal: reason + PIN, 21 CFR Part 11 compliant; Override flow with mandatory rationale
- Batch detail panel: cross-links to Dose Orders, Shipments, and QC
- Quality Hold display with alert banners
- New Batch modal with radioisotope, linked DOs, planning notes

### Sprint 4a — Shipments + Chain of Custody
- Shipment list (4 shipments: Delivered, In Transit, Scheduled, Not Created)
- Temperature log visualization with excursion detection (color-coded, alert banners)
- Chain of Custody tab: 4-step CoC visual (Created → In Transit → Receipt → Administration)
- Receipt confirmation modal: temp at receipt, packaging/seal checks, PIN e-signature
- Administration confirmation modal: patient ID, adverse events, PIN e-signature
- Shipment detail panel: full timeline + temp log + CoC steps + batch/DO cross-links

### Sprint 4b — Quality Control
- CoA review table: RCP%, sterility, potency, identity per batch
- QC detail panel: test results summary cards, full audit trail
- QC Approve (e-signature) → batch status updates to Released
- Quality Hold (e-signature) → batch status updates to Quality Hold
- CoA Upload modal: lot number, test date, results summary, QC decision
- Cross-links: QC → Batch → Shipment → Dose Order

## Roles (6)
| Role | User | Key Capabilities |
|------|------|-----------------|
| Clinical Operations | Sarah Mitchell | Approve/reject SARFs and Dose Orders |
| Clinic Site (UCSF) | Dr. James Park | Submit SARFs, DOs, confirm receipt/administration |
| Planning | Ana Reyes | Create batches, confirm MARK AI allocations |
| Logistics | Priya Nair | Create/manage shipments, temp monitoring |
| Quality | David Chen | Upload CoAs, approve/hold batches, release for shipment |
| Admin | Meredith Blake | System admin (user access, environments) |

## Data Model
- **SARFS**: 8 records across all statuses and priorities
- **DOSE_ORDERS**: 6 records linked to SARFs, with MARK AI dose calculations
- **BATCHES**: 7 records (Released → Quality Hold → Planned → Draft) with MARK AI allocations
- **MARK_ALLOCS**: 3 MARK AI recommendations (2 pending, 1 failed due to QC hold)
- **SHIPMENTS**: 4 records with realistic temperature logs and IoT excursion alerts
- **QC_ITEMS**: 4 records (Approved ×2, Pending Review ×1, Quality Hold ×1)
- **COC_RECORDS**: Chain of custody per shipment (receipt + administration steps)

## Key Patterns
- `openEsig({type, action, id, btnClass, btnLabel})` — universal 21 CFR Part 11 e-signature modal
- `confirmEsig()` — dispatches to correct handler based on `esigContext.type`
- Side panels: overlay + slide-in panel, each with header/body/footer zones
- `renderSidebar()` reads live badge counts from all data sources
- `switchRole()` re-renders all 6 sections on role change

## Files
- `index.html` — Complete single-file application (all 4 sprints)
- `arc-csc-sprint2.html` — Sprint 2 archive (SARF + Dose Orders only)
- `replit.md` — This documentation file
