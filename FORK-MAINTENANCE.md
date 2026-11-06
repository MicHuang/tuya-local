# Self-Maintained Tuya Local Candidate

This branch is a candidate, not a stable release or an installation instruction.
It pins upstream 2026.10.0 and adds one reviewed device YAML without changing
the parser, integration code, original OWON config or dependency declarations.
The upstream MIT license and notices are retained.

## Branches And Versions

- Keep `main` as the upstream reference; never deploy rolling `main` implicitly.
- `codex/homeos-maintained-2026.10` is the self-maintained candidate branch.
- The earlier proposal branch is historical and remains untouched.
- `homeos-maintenance.json` records the exact base and YAML hash. Its candidate
  ID is a bookkeeping label, not the integration version or a GitHub release.
- No default-branch change, stable tag, release, automatic sync or HA deployment
  is part of this preparation. The manifest is not an executable safety lock.

## Local Patch

The variant exposes separate heat/cool targets and leaves missing fault data
unknown. Missing/null unit readings are unknown; only c/f enables climate.
Unknown units gate ALL normal climate controls, including OFF, indefinitely
until valid unit data returns. Direct Python/DPS calls bypass HA availability.
Unavailable capability metadata still carries 41/3200 and step 1, not physical
bounds. Product/DP fallback matching is not strict product isolation.

The already-reviewed 44-case synthetic regression file is kept in the companion
operations repository, not shipped in this YAML-only fork change. Validate its
recorded hash and copy it only into a disposable source worktree for testing.
Record Python, HA, plugin and resolved dependency versions for each run; the
2026.10.4 test stack is not a reconstruction of the release's development lock.

## Updating The Base

1. Confirm the local tree is clean and save the current candidate commit ID.
2. Fetch the chosen upstream release, verify its exact commit, and inspect its
   integration, dependency, manifest and OWON changes before creating a branch.
3. Create a new candidate branch from that release and replay only the isolated
   YAML patch commit. Stop on conflicts, overlaps or changed DP semantics.
4. Update the base/hash record. Run the native full suite and focused local
   regression, Ruff checks including import order/format, YAML and translation
   checks. Compare failures/warnings against an unmodified baseline; record all
   retained limitations. Dependency refreshes require a new recorded resolution.
5. Review changed runtime behavior under the operations repository's review
   policy. Old artifact clearance does not cover changed bytes or a new base.
6. Push only to this fork after validation. Do not force-push published branches,
   rewrite upstream tags, or automatically advance a release/installed system.

## Installation And Rollback Gate

The installed original-name local patch and this new variant are NOT equivalent
installation targets. Existing entries store the original config type; merely
installing this branch will not select the new variant and may remove the old
local patch. Do not delete/re-pair the device or edit HA storage as a shortcut.

Before any installation, separately authorize a migration plan for stored config
type, entity IDs and HACS source ownership. Back up the full existing integration
and relevant HA entry state privately, record exact versions/hashes, and verify
how to restore both. Backups may contain secrets and must never enter Git.
HACS replaces the integration directory when downloading; local edits/backups
inside that directory are not a durable rollback source. Do not let upstream
and fork downloads compete for the same integration directory.

Only a separately approved cutover and hardware acceptance can establish a
stable self-maintained release. Check setpoint readback, modes, schedules and
restored physical state; offline tests do not prove compressor behavior.
Fahrenheit, endpoints/deadband, emergency heat and migration remain unverified.
On failure use the prepared matching code/state backup under an authorized
rollback plan, not a blind update or an untested older integration.
