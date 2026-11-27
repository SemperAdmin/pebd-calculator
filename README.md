# PEBD Calculator

Calculates a Marine's Pay Entry Base Date (PEBD) under PAA 04-25 and MARADMIN 052/26, and shows the basic pay effect of a correction. Built for Marines and the administrators who process PEBD corrections.

**Live:** https://pebd-calculator.app.cloud.gov

## What it does

- Computes creditable service and the PEBD from any number of service periods across 54 service types, including the Military Academy, Delayed Entry Program and Platoon Leaders Class rules. On the officer pathway it asks the PLC financial assistance (MCTAP) question and will not calculate until it is answered.
- Deducts time lost (AWOL, confinement, desertion and the other DODFMR Table 1-2 absences) on the 30-day-month basis, including the "made good" rule.
- Cross-checks the result against the DOEAF, AFADBD and PEBD of record from MCTFS and explains any mismatch.
- Produces a print report and a pre-filled email package for the PAC-to-MMPB-21 correction request (MARADMIN 052/26 para 3).
- Compares basic pay month by month between the original and corrected PEBD, using pay tables from 1993 to 2026, on the [Basic Pay Comparison](https://pebd-calculator.app.cloud.gov/pay-comparison.html) page, on its own or handed off from the calculator.
- Includes a "How to Use" guide and eleven worked examples.

## Using it

Open the live site in a current browser. No account or CAC is needed.

1. Acknowledge the DoD notice and the terms.
2. Enter the foundational PEBD, then select status and pathway.
3. Optionally enter DOEAF and AFADBD from MCTFS for the record cross-checks.
4. Add each service period and any time lost.
5. Select **Calculate PEBD**, then print the report, email the MMPB-21 package, or open the pay comparison.

Results are a reference. Verify them against official records (DD Form 4, DD Form 214, NAVMC 763) before official use.

## Privacy and security

- Calculations run in your browser. The calculator does not send what you enter anywhere.
- Two features leave the browser only when you choose them: **Email MMPB-21 Package** opens your own mail program with the results filled in, and sending is up to you; **Feedback** opens an external form that needs a .mil network.
- The browser stores only your terms acknowledgement and theme choice. No analytics or cookies.
- The Content Security Policy blocks connections, scripts, fonts and images from other sites.
- CUI markings on output are a reminder for the user. This site is not an authorized system for CUI or PII; do not enter such information unless your command has approved this use.
- The hosting platforms (cloud.gov and GitHub Pages) keep standard web server access logs, such as IP address, time, requested page and browser type, for operations and security. Semper Admin does not add tracking of its own.

See the [Privacy Policy](PRIVACY_POLICY.md), [Terms of Service](TERMS_OF_SERVICE.md), [Accessibility statement](ACCESSIBILITY.md) and [Security](SECURITY.md).

## How the calculation works

The calculator follows PAA 04-25, MARADMIN 052/26 and DODFMR Volume 7A, Chapter 1, with 37 U.S.C. 205 and the related statutes and orders. The step-by-step method, the service-type rules and the full list of references are in [docs/CALCULATION.md](docs/CALCULATION.md). The published text of PAA 04-25 and MARADMIN 052/26 is in [`references/`](references/).

## Development

No build step and no dependencies. The site is two static pages, `index.html` (calculator) and `pay-comparison.html` (pay comparison), in plain HTML, CSS and JavaScript. To run it locally, open `index.html` in a browser.

Run the tests with Node.js:

```bash
node test-calculations.js
```

The tests load the calculation code straight out of the two pages, so the code under test is the code that ships.

## Deployment

| Environment | URL | How |
|---|---|---|
| cloud.gov | https://pebd-calculator.app.cloud.gov | `manifest.yml` (staticfile buildpack) and the manual **Deploy to cloud.gov** workflow. The sandbox is managed by the private cloudgov-redeploy repository. |
| GitHub Pages | https://semperadmin.github.io/pebd-calculator/ | Published from the repository root of `main`. |

## Feedback and support

[Open an issue](https://github.com/SemperAdmin/pebd-calculator/issues), or use **Feedback** in the app from a .mil network. Report security problems as described in [SECURITY.md](SECURITY.md).

## License and disclaimer

MIT (see [LICENSE](LICENSE)).

> This is an independent tool built to help Marines with administrative work. It is not an official Department of the Navy or U.S. Marine Corps system and is not endorsed by, affiliated with, or approved by either. Always confirm requirements against the current governing orders.

## Part of Semper Admin

One of the tools in the Semper Admin suite. Start at the [Semper Admin Portal](https://semper-admin-portal.app.cloud.gov) for the full set.
