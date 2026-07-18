# Security Policy

This project handles meter-reading-and-vending-machine-collection
route operating workflows. Treat vulnerabilities as potentially high
impact even when the demo data is synthetic — this domain's failure
modes include physical worker-safety risk (site-access/personal-safety
hazard: entering unfamiliar customer premises) and cash-handling trust/
security risk (vending-machine cash-collection integrity, equipment
condition).

## Do Not Disclose Publicly

Report privately before opening public issues for:

- credential exposure
- real worker, route or operator data exposure
- authorization bypass
- MeterReadingGovernor bypass
- audit-ledger tampering
- over-disclosure in reports or exports
- unsafe robot action dispatch
- any path that lets a proposal reach a cash-collection/
  reconciliation-execution decision, a site-safety-clearance decision
  (e.g. declaring a site cleared for entry), or a
  route-safety-supervisor override decision

## Reporting

Use GitHub private vulnerability reporting when available for the repository.
If that is unavailable, contact the repository maintainers through the
cloud-itonami organization before publishing details.

Include:

- affected commit or version
- reproduction steps
- expected and actual behavior
- impact on worker/route data, policy enforcement or audit logging
- suggested fix, if known

## Production Guidance

- Store secrets outside Git.
- Keep real worker/route/operator data outside this repository.
- Run policy tests before deployment.
- Export and review audit logs regularly.
- Use least privilege for operators and service accounts.
