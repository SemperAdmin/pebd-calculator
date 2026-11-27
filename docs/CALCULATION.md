# How the PEBD Calculator works

This is the detailed reference behind the [README](../README.md): the rules the
calculator applies, step by step, and the sources it applies them from.

## Calculation steps

1. **Input** — Foundational PEBD (beginning date of the most recent period of continuous service for pay), Marine status, pathway type, service periods, and time loss periods
2. **Creditability Check** — Each service period is evaluated against PAA 04-25 rules for the given pathway type (Military Academy service credits only on enlisted pathways). PLC / officer candidate rows carry their own creditability by variant under DODFMR Vol 7A Ch 1 and 37 U.S.C. 205(f), and the 10 U.S.C. 16401 financial assistance question must be answered before such rows are scored
3. **End-Date Adjustment** — Ending day 31 becomes 30; February 29 becomes 30; February 28 of a non-leap year becomes 30; February 28 of a leap year stays 28 (DODFMR Vol 7A Ch 1 para 2.4.1.2.2). PAA 04-25 para 6.b step 2 states the flat rule without the leap-year exception; the tool follows the DODFMR and raises a note naming both texts when the case arises
4. **Summation** — Begin totals and end totals are summed component-wise across all creditable periods
5. **Difference** — End totals minus begin totals
6. **Inclusive Day Adjustment** — Plus one day per creditable service period
7. **Time Loss Deduction** — Each deductible, non-officer-time loss is computed on the 30-day basis (years/months/days, +1 inclusive day, a loss beginning on the 31st counts that day) and subtracted from the total. A loss marked made good uses the smaller of the 30-day and day-to-day figures; the contract floor of para 2.4.1.3.2.3 is flagged in the record notes rather than applied, since the form does not capture contract length
8. **Normalization** — Days and months are normalized on the 30-day/12-month convention (e.g., 35 days becomes 1 month 5 days)
9. **PEBD Derivation** — Net creditable service is subtracted from the foundational PEBD using pure 30-day arithmetic; a nominal February 29/30 result clamps to the real last day of February
10. **Record Cross-Checks** — The result is compared against the optional DOEAF and AFADBD values and the financial assistance answer, and any inconsistency is listed as a warning on the results view, the print report, and the MMPB-21 package

## Service types and record checks in detail

- **Unlimited Service Periods** — Add or remove service period rows dynamically with start/end dates, service type, and notes
- **54 Service Types** — Automatic creditability determination including special Military Academy logic (Officer vs. Enlisted pathways, plus the Table 1-1 retained-commission case), Delayed Entry Program rules split by enlistment authority (Reserve 10 U.S.C. 12103 with or without IDT, Regular 10 U.S.C. 513 excluded, DODFMR Vol 7A Ch 1 paras 2.1.4.12 and 2.2.1.8), a fraudulent or voided enlistment exclusion (para 2.2.1.1), medical retention due to misconduct (PAA 04-25 para 5.2), Coast Guard and Space Force Reserve (PAA 04-25 para 4.a.4), and Platoon Leaders Class / officer candidate variants: inactive time before the initial ADT excluded without IDT (DODFMR Vol 7A Ch 1 para 2.2.1.8.1), inactive time after it creditable (para 2.1.3.2), drilling Selected Reserve time creditable, and a separate 37 U.S.C. 205(f) variant for a 10 U.S.C. 12203 appointee who received 16401 financial assistance
- **MCTAP Gate** — A Marine Corps Tuition Assistance Program question (MCO 1560.33, 10 U.S.C. 16401, 37 U.S.C. 205(f)) opens on the Officer pathway or whenever a row carries an officer candidate type, and calculation is blocked until it is answered. Yes applies PAA 04-25 para 7.b Rule 2: inactive PLC rows are excluded at commissioning, OCS active duty and drilling SMCR time stay. The answer travels into the results, print report, and MMPB-21 package
- **MCTFS Record Cross-Checks** — Optional DOEAF and AFADBD inputs drive non-blocking notes: a PEBD later than DOEAF with nothing excluded, a PEBD equal to AFADBD (expected for a post-1989 Reserve enlistment with no IDT before the initial ADT, shown as a confirmation prompt), a zeroed AFADBD on a member with active service, and a 205(f) row entered with the financial assistance field set to No
- **Time Loss Deductions** — AWOL, confinement, desertion, and other non-creditable absence types per DODFMR Vol 7A Ch 1 Table 1-2, computed on the 30-day-month basis as years/months/days with +1 inclusive day (para 2.4.1.3.1), an "Officer Time?" flag that records but does not deduct commissioned-service losses, a "Made Good?" flag that computes the loss on both the 30-day and day-to-day bases and keeps the smaller (para 2.4.1.3.2.1), and creditable outcomes for excused absence and acquittal (Table 1-2 Rule 2, Notes 2 and 3)
- **Real-Time Validation** — Strict YYYYMMDD calendar validation with per-row status icons and invalid-field highlighting as you type
- **Combined Consent Gate** — Single interstitial carrying the DoD Notice and Consent Banner (DoDI 8500.01) plus Terms of Service, Privacy Policy, and Accessibility tabs
- **CUI Markings** — Dynamic CUI//SP-PRVCY banners appear once the form contains data and carry through to printed reports (DoDI 5200.48)
- **Content Security Policy** — Strict CSP meta, nosniff, and no-referrer headers (NIST SP 800-53 SC-18)
- **Print Report** — Formatted, CUI-marked print report with governing references and calculation breakdown
- **MARADMIN 052/26 Workflow** — An optional PEBD of Record input drives the para 3.c routing note (D188 remark update at PAC level when the PEBD matches, MMPB-21 when it does not), a pre-filled EPAR text for the Marine's para 3.a request to the servicing PAC, and a CUI-marked mailto package for the PAC-to-MMPB-21 forward with the para 3.d MISSO 9 step stated
- **Basic Pay Comparison Companion** — Hand off the foundational and calculated PEBD to `pay-comparison.html` for a month-by-month basic pay difference (embedded pay tables 1993-2026). Also runs standalone: when HQMC or MMPB-21 has already corrected the PEBD, enter the original and corrected PEBDs directly, with a one-grade shortcut for the career timeline
- **Built-In Instructions** — A collapsible "How to Use" walkthrough on both pages covering every step from data entry to escalation
- **Guided Examples** — Eleven one-click examples with verified expected results: seven on the calculator (including the PAA 04-25 and DODFMR official worked examples and a PLC officer candidate under DODFMR para 2.2.1.8.1) and four on the pay comparison (anniversary splits, mid-period promotion, the E0 four-month rule, and a Feb 29 PEBD)
- **Toast Notifications** — Non-intrusive toast messages replace alert() dialogs
- **Dark/Light Theme** — Semper Admin portal styling with a pre-paint theme bootstrap

## Governing references

| Reference | Description |
|-----------|-------------|
| PAA 04-25 | Creditable Service & PEBD calculation methodology, the enlisted and officer pathway tables, and the MCTAP rule (para 7.b Rule 2, Notes 2 and 5). Text and PDF stored under `references/` |
| [MARADMIN 052/26](https://www.marines.mil/News/Messages/Messages-Display/Article/4409903/marine-corps-total-force-system-advisory-to-identify-required-corrections-to-pa/) | MCTFS Advisory for PEBD corrections |
| DODFMR Volume 7A, Chapter 1 (May 2024) | Time loss deduction and DEP creditability rules. MARADMIN 052/26 reference B |
| MCRCO 1100.1B | Marine Corps Recruiting Command Enlistment Processing Manual. MARADMIN 052/26 reference C |
| MCO 1560.33 | Marine Corps Tuition Assistance Program, the PLC financial assistance 37 U.S.C. 205(f) keys on. MARADMIN 052/26 reference D |
| 10 U.S.C. 12103 | Reserve component enlistment terms, the authority the DEP and PLC creditability rules turn on. MARADMIN 052/26 reference E |
| MCTFS PRIUM | Reporting instructions PAC follows when forwarding a PEBD case to MMPB-21 (MARADMIN 052/26 para 3.c) |
| DODFMR Volume 7A, Chapter 1 paras 2.1.3.2, 2.1.4.10, 2.1.4.12, 2.2.1.8, 2.4.1.1.2 | Reserve service creditable active or inactive; ROTC with concurrent Selected Reserve drilling on or after 1 Aug 1979; post-1989 Reserve enlistments creditable before the initial ADT only with IDT; officer basic pay date starts at acceptance of the commission |
| 37 U.S.C. 205 | Service creditable for basic pay. Subsection (f): post-1999 PLC time is excluded for an officer appointed under 10 U.S.C. 12203 after 16401 financial assistance, except active duty and Selected Reserve time. Not implemented in the DODFMR chapter |
| 10 U.S.C. 16401 | Marine Corps Platoon Leaders Class financial assistance program |
| MCO 1900.16 | Separation and retirement manual |
| MCO P1070.12K | IRAM (Individual Records Administration Manual) |
| SECNAVINST 1000.30B | Creditable service determination |

The published text of PAA 04-25 and MARADMIN 052/26 is in [`references/`](../references/).

## What the tests cover

The test suite loads the shipped calculation logic out of `index.html` (every pure function sits between two markers in the page, so the code under test is the code that ships) and checks:

- Date parsing (strict calendar validation, leap years)
- Inclusive day counting
- Service type creditability (academy pathway logic, DEP variants, exclusions)
- Time loss deductibility (DODFMR Table 1-2) and 30-day-basis lost time computation
- 30-day date subtraction (borrowing, February clamping)
- Full PEBD calculations (per-period inclusive days, end-date adjustments)
- Time loss integration (officer time, accumulation, invalid input)
- The DODFMR Vol 7A Ch 1 worked example (para 2.4.1.3.1) reproduced end to end
- The PAA 04-25 Section 6 worked example reproduced end to end
- The DODFMR para 2.4.1.2.5 four-period 22-year example and the para 2.4.1.3.2 made-good illustration
- The DTMS export loaded from `pay-comparison.html`
- PLC officer candidate paths (DODFMR no-IDT path landing on the OCS report date, IDT path, 37 U.S.C. 205(f) path, Selected Reserve path)
- Record cross-check warnings and every guided example card expected value, computed from the shipped definitions

Run with Node.js:

```bash
node test-calculations.js
```
