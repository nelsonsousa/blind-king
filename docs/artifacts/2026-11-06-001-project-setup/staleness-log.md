# Staleness log

HOLDS and NOT STAMPED are recorded on the first check of an artifact and when its
status changes; an unchanged result is not repeated. A STALE line is recorded
when the state differs from the artifact's last line. The three-cycle bound
counts distinct upstream states among the STALE lines, not lines. An artifact
with no line has no recorded check, and its last line is its current status.

- 2026-11-06T22:32:04Z `implementation-plan.md`: NOT STAMPED (no basis_of recorded, so nothing was checked)
- 2026-11-06T22:32:04Z `test-plan.md`: NOT STAMPED (no basis_of recorded, so nothing was checked)
- 2026-11-06T22:32:04Z `verification-report.md`: HOLDS
