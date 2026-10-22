# Log4Shell (CVE-2021-44228) — Advisory Briefing

## Summary

Log4Shell is a remote code execution vulnerability in Apache Log4j 2. The JNDI features used in
configuration, log messages, and message parameters do not protect against attacker-controlled
LDAP and other JNDI-related endpoints. An attacker who can control log messages or log message
parameters (including data in logged errors, exception traces, and authentication failures) can
execute arbitrary code loaded from LDAP servers when message lookup substitution is enabled.

## Advisory details

| Field | Value |
| --- | --- |
| GHSA ID | [GHSA-jfh8-c2jp-5v3q](https://github.com/advisories/GHSA-jfh8-c2jp-5v3q) |
| CVE | CVE-2021-44228 |
| Severity | **Critical** (CVSS 3.1 score: **10.0**, vector `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:C/C:H/I:H/A:H/E:H`) |
| Affected package | `org.apache.logging.log4j:log4j-core` (Maven) |
| Affected version range | `>= 2.13.0` (and `>= 2.0-beta9`, `>= 2.4` in other branch lines) — broadly "any version prior to 2.15.0" |
| First patched version | **2.15.0** (2.16.0 is the recommended fix; it fully disables JNDI lookups by default) |

## Affected versus patched versions

- **Affected:** any Log4j version prior to 2.15.0 is subject to this specific issue. In the
  `org.apache.logging.log4j:log4j-core` package, GitHub's advisory database records the affected
  ranges as `>= 2.13.0` (fixed in 2.15.0), `>= 2.4` (fixed in 2.12.2), and `>= 2.0-beta9`
  (fixed in 2.3.1). The v1 branch of Log4j is End of Life and is vulnerable to other RCE
  vectors, so the recommendation is to move to 2.16.0.
- **First patched:** **2.15.0** contained the first fix for this vulnerability, but that patch
  did not disable attacker-controlled JNDI lookups in all situations. The follow-up issue
  (CVE-2021-45046) is tracked in [GHSA-7rjr-3q55-vv33](https://github.com/advisories/GHSA-7rjr-3q55-vv33).
- **Recommended fix:** **2.16.0**, which disables JNDI by default and completely removes
  support for message lookups. Backports of the fix are also available in **2.3.1**, **2.12.2**,
  and **2.12.3**.
- Only `org.apache.logging.log4j:log4j-core` is directly affected; keep
  `org.apache.logging.log4j:log4j-api` at the same version as `log4j-core` if it is in use.

## On-call impact

- Logging untrusted or user-controlled data with a vulnerable version may result in remote code
  execution against the application.
- Unexpected vectors of user-controlled input include exception traces logged by error
  handlers, authentication failures, and HTTP headers such as `User-Agent` that end up in logs.

## Remediation

1. Upgrade `org.apache.logging.log4j:log4j-core` to **2.16.0 or later** (2.15.0 is the first
   patched version but is itself incomplete — see CVE-2021-45046).
2. If you cannot upgrade to 2.16.0, ensure that no attacker-controlled JNDI lookups resolve to
   attacker-provided data and ensure the `JndiLookup` class is not loaded
   ([LOG4J2-3221](https://issues.apache.org/jira/browse/LOG4J2-3221)).
3. Log4j v1 is EOL and will not receive patches; migrate to Log4j 2.16.0.

## References

- GitHub Advisory Database record: https://github.com/advisories/GHSA-jfh8-c2jp-5v3q
- NVD entry: https://nvd.nist.gov/vuln/detail/CVE-2021-44228
- Apache Log4j security guide: https://logging.apache.org/log4j/2.x/security.html
- Follow-up advisory (CVE-2021-45046): https://github.com/advisories/GHSA-7rjr-3q55-vv33
- Apache upstream fix: https://github.com/apache/logging-log4j2/pull/608
