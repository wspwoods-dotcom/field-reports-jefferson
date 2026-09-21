# FEMA Public Assistance Export Spec

Locked 2026-09-21. Built against the current FEMA 009-0 form series so the
export matches what FEMA asks for — no hand-editing. If FEMA revises the
forms, update this spec file only; the exporter reads it.

## Source records in the app

- Incident-tagged reports (Incident mode): id, name, declaration number,
  incident date range, categories of work (A/B/C–G).
- Per report: damage description, GPS, timestamps, before/during/after photos,
  pre-disaster condition (pulled from routine reports at the same site).
- Costing per report: labor entries (crewId, regular hrs, overtime hrs),
  equipment entries (equipmentId, hours, operator), materials (desc, qty,
  unit price, receipt photo), rented equipment, contracts.

## Export package (one folder per incident)

1. `cover.csv` — applicant (org name), declaration number, incident name,
   incident period, categories claimed, total per category, preparer, date.
2. `labor-009-0-123.csv` — Force Account Labor Summary: employee name, title,
   regular hours, overtime hours, hourly rate, fringe $/hr, total cost.
   Overtime claimed only where the org's pre-disaster labor policy allows it.
3. `fringe-009-0-128.csv` — Benefit Calculation Worksheet: per-employee
   fringe components (FICA, IPERS/pension, insurance, workers comp).
4. `equipment-009-0-127.csv` — Force Account Equipment Summary: type,
   year/make/model, size/capacity, dates used, hours, operator name,
   hourly rate and rate basis (FEMA schedule vs applicant rate), total.
   Standby/idle hours excluded — FEMA will not pay them. Equipment hours
   must reconcile with the operator's timesheet.
5. `rented-equipment-009-0-125.csv` — description, vendor, rental agreement
   ref, invoice refs, total cost.
6. `materials-009-0-124.csv` — description, quantity, unit price, total,
   source (stock w/ historical cost ref, or purchased w/ receipt ref).
7. `sites.csv` — per damage site: facility name/function, GPS, pre-disaster
   condition summary + photo refs, cause, damaged components/dimensions,
   scope of work in quantifiable terms, linked photo files.
8. `photos/` — all incident photos, filenames referenced by the CSVs.
9. `manifest.json` — spec version, app build, export date, record counts,
   SHA of each file.

## Rate tables

- FEMA Schedule of Equipment Rates (fema.gov/schedule-equipment-rates),
  refreshed when FEMA republishes.
- Applicant's own equipment rates as the alternate basis, selectable per export.
- Labor: actual loaded rates from Crew wages + fringe worksheet.

## Standing rules

- Records retained 3 years from subgrant close (2 CFR Part 200) — the export
  package is the archive unit, not the phone's localStorage.
- Volunteer hours export separately: they can offset the non-federal cost
  share, tracked at the Independent Sector state rate.
- Project management (DAC) hours tracked as their own category.

## Equipment purchases (locked 2026-09-21)

FEMA may fund purchased equipment when the applicant lacks sufficient
equipment to respond — but only the least costly option wins. The export
carries a purchase file per item:

- Item description, vendor, invoice/receipt, purchase price, date placed
  in service for the incident.
- Lease-vs-purchase analysis (2 CFR 200.318(d)): the compared lease quote,
  expected useful service life, market basis — FEMA reviews the comparison
  for reasonableness.
- Procurement basis: competitive quotes/bids per federal procurement standards.
- Disposition: when the item is no longer needed for the incident, record
  current fair market value. Items with FMV of $5,000 or more reduce the
  eligible project funding by FEMA's share unless FEMA is informed the item
  stays in other federally funded use.

App logger: "Log equipment purchase" in Incident mode — item, vendor,
price, invoice photo, lease quote for the comparison, in-service date.

## Direct Administrative Costs / admin time (locked 2026-09-21)

The applicant's own admin time on the FEMA claim is reimbursable as DAC
when it is tracked, charged, and accounted directly to a specific project.
Eligible tasks include: visiting/surveying/assessing damage sites,
developing damage descriptions, reviewing the project worksheet, preparing
correspondence and small projects, collecting/copying/filing/submitting
claim documents.

DAC export rules (the app enforces these at entry):

- Every DAC entry needs: specific task description, person, position/skill
  level, hours, date, linked incident/project.
- Actual costs only — no blended rates. FEMA judges reasonableness by
  whether the skill level fits the task and the effort fits the job.
- Kept separate from force-account field labor; it is its own summary sheet
  (`dac.csv`) in the export package.

App logger: "Log admin time" in Incident mode — task description picker
(survey, damage description, worksheet review, correspondence, filing),
person, hours. This is the director's own FEMA paperwork hours, made
reimbursable by the act of logging them.
