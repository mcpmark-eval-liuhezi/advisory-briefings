# advisory-briefings

This repository collects our security advisory briefings. Each briefing documents a notable
security advisory — what it is, which packages and versions it affects, how to remediate it,
and where the authoritative upstream record lives — so the on-call runbook and release
audits always have a permanent, traceable home for them.

## Contents

- [`docs/`](docs/) — one briefing per advisory, e.g. the [Log4Shell (CVE-2021-44228) briefing](docs/log4shell.md).

## Briefings

| Advisory | Severity | Package | First patched version | Briefing |
| --- | --- | --- | --- | --- |
| CVE-2021-44228 (Log4Shell) | Critical | `org.apache.logging.log4j:log4j-core` (Maven) | 2.15.0 (2.16.0 recommended) | [`docs/log4shell.md`](docs/log4shell.md) |

## Checks

`.github/workflows/briefing-check.yml` runs a lightweight markdown sanity check on briefings.
It is manually triggered (`workflow_dispatch`) via the Actions tab.
