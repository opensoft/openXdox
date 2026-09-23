# openXdox Contract Changelog

Status: standard

The project's release history. A bundle is `xdox-v<major>.<minor>` and is
identified by four coordinated values: `contract_bundle_version` in
[`manifest.yaml`](./manifest.yaml), that file's `entries:` rows with their
per-file `contract_schema_version` and SHA-256, an annotated
`xdox-v<major>.<minor>` tag over the root commit that names both legs, and the
matching entry below. **Consumers pin the exact commit and digests — a movable
branch or tag is not a compatibility pin.** The root holds no contract bytes of
its own, so a bundle is the LEGS at the commits `contracts/spec-pin.yaml` and
`contracts/code-pin.yaml` name.

**Release-surface rule.** `xdox-vN.M` selects `{ entry | entry.release_member ==
true }` from `manifest.yaml` — by declared field, never by a path heuristic,
which would break on this very bundle: one member and four non-members share
`contracts/schemas/`. Membership is catalog-driven from openxFactory's
`contracts/hermes-runtime/contract-index.yaml` and is not this project's to
assert.

**`Status: standard`, on the operator's word, with all four coordinated values
now in place.** This entry landed BEFORE its tag existed — openxFactory's own
lane/operator split (RULED ASK-9a → 1: *"the lane authors and lands the cut PR …
the annotated tag is Brett's act"*) — and while the tag did not exist a
`standard` header would have asserted a published bundle that did not, so the
header read `draft` through that window and the promotion was registered as an
open question on `opensoft/openxFactory` issue #656 (comment 5767092136).

**That window is closed.** `xdox-v1.0` is cut and published: annotated tag object
`2d2e9b854eb97053af905beb0ae8902887935f3a`, over `2073e3a92948b266d4e4d8063a6d4522b089cf3c`, tagger
Brett Heap, 2026-09-21T20:59:28Z. So `contract_bundle_version` in
[`manifest.yaml`](./manifest.yaml), that file's `entries:` rows with their
per-file digests, the annotated tag, and the entry below now all name the same
bundle together — which is the coordinated identity
`opensoft/openXwallet`'s contract changelog describes and the reason it carries
`Status: standard` too. Promoted on Brett Heap's word, verbatim 2026-09-21:

> **"(a) for all four, (i) for the tag, keep going"**

— item 3, recorded at [#656 comment
5767804734](https://github.com/opensoft/openxFactory/issues/656#issuecomment-5767804734).
`openxFactory`'s own contract changelog still carries `draft` across twenty-plus
published releases and `docs/document-lifecycle.md` still does not mention a
changelog; neither was ever a rule, and neither is now an argument about this
one.

## Unreleased — 2026-09-23

**The code leg advances `ab04453d` → `195276b7` → `39d8937e`**, both on
`opensoft/openXdox-code`'s `DISPLAY` facet for openDox's `completion` stage:

- `195276b7`, #26: the xFactory host's label **"implemented"**, RULED at
  `opensoft/openxFactory` #656 comment `5784683830` (verbatim *"1, keep
  completed and overlay implemented"*).
- `39d8937e`, #27: the same stage's item nouns, **"implemented item"** and
  **"implemented items"**, RULED at #656 comment `5801057769` (verbatim *"yes,
  overlay implemented items too"*).

**No contract byte moves.** The code leg carries no `contracts/` path at any of
the three commits (measured with `git ls-tree -r`), so it contributes no
`entries:` row, and the spec leg is unchanged at `f088b097`. This cuts no
bundle: `xdox-v1.0` keeps the two legs it was cut over (`ab04453d`,
`f088b097`). Both legs' `main` are again exactly the commits this root pins:
`openXdox-code` `39d8937e` and `openXdox-spec` `f088b097`.

## xdox-v1.0 — 2026-09-21 (the first bundle: openXdox's contract surface is the carved spec leg's five files — one openxFactory catalog release member, three non-member schemas and one example)

Realizes `split-opendox-two-layer-product` **§ 4.6** (`tasks.md` § 4), on § 3.8's
reasoning: a tag on a leg describes half a project, so the bundle tag, this file
and [`manifest.yaml`](./manifest.yaml) live where a consumer's pin points. Cut
under RULED **(a)** — Brett Heap, 2026-09-21, `opensoft/openxFactory` issue #656
comment `5767052465`, on the RULING NEEDED at `5738327369`:

> **"(a) for both, cut the tags when the drafts are green."**

**Change class: the first bundle.** There is no predecessor to be compatible
with, so no migration path is owed and none is claimed.
`contract_bundle_version` moves `none` → `xdox-v1.0` and `entries: []` gains
five rows.

### What the bundle is

| leg | repository | commit | contract bytes |
|---|---|---|---|
| code | `opensoft/openXdox-code` | `ab04453de25b5c93479a5a6ff3700aec02f6e2b3` | none — no `contracts/` path at any commit |
| spec | `opensoft/openXdox-spec` | `f088b09732e236279898b53ab9fb0f5ebc89509a` | the five files below |

| `id` | path in the spec leg | `type` | `sha256` | `release_member` |
|---|---|---|---|---|
| `gate-action-record` | `contracts/schemas/gate-action-record.schema.yaml` | release-schema | `6a6cf13c76e3e6f6792cd2a4be1ed2296b24525bee800a3e9173d608e0539189` | **true** |
| `ideation-dashboard-snapshot` | `contracts/schemas/ideation-dashboard-snapshot.schema.yaml` | release-schema | `054259fe96686e3cace3269a09740b9cd8846fbc2f88e52dd0c93f8974aeab3b` | false |
| `ideation-dashboard-snapshot-index` | `contracts/schemas/ideation-dashboard-snapshot-index.schema.yaml` | release-schema | `7fd9b797730e25f7a97c6acc3fb728ccce1b688643e950e7384eb0f9e9a0e296` | false |
| `domain-profile` | `contracts/schemas/domain-profile.schema.yaml` | release-schema | `51c53917126e73bffb3167e22992c902c66a00c5634e9af1cefe2f61178c685a` | false |
| `domain-profile-example` | `contracts/schemas/domain-profile.example.yaml` | example | `83954b27ce75497d25131a049179bf6f3b3105b123c323e3bd044c49cf4699a8` | false |

**THE ONE RELEASE MEMBER'S BYTES HAVE NOT MOVED SINCE openxFactory SHED IT.**
`gate-action-record` carries at `f088b097` exactly the digest openxFactory
recorded at `contract-v3.7` and re-asserted at `contract-v4.0` (`6a6cf13c…`),
and it is byte-identical at `481a07f9` — the leg commit that table names — and at
`f088b097`. openxFactory *"no longer OWNS them but still CONSUMES them"*; this is
the bundle at the other end of that sentence.

**TWO NON-MEMBERS DID MOVE, BY A DECLARED EDIT AND NOT A CONTRACT CHANGE.**
`openXdox-spec` `ae59dfa` (*"BUILD § 3.6: correct the 4 stale citations across
three arrived files in place, inside the Q-L1 declared lines"*) took
`ideation-dashboard-snapshot` `9c44da23…` → `054259fe…` and
`ideation-dashboard-snapshot-index` `43acf0bb…` → `7fd9b797…`. Both matched
openxFactory's recorded values exactly at `481a07f9`, so **that table was true
when it was written**; the change is a citation correction taken after it, on the
pair contract-v4.0 itself describes as *"never catalog members and … in no
inventory"*.

### What this bundle depends on

openXdox is a domain descendant of openDox and pins it at
[`opendox-pin.yaml`](./opendox-pin.yaml), commit
`c4c5014d9b39ac55e5df957db23a56db38847a6b`. That is **one openDox root behind**
openDox's current `main`, so `xdox-v1.0` composes over `c4c5014d` and **not**
over the openDox root that `dox-v1.0` tags. Advancing that pin is a
three-repository lockstep — openxFactory's `openDox` gitlink, openxFactory's
`contracts/opendox-pin.yaml`, and this repository's `contracts/opendox-pin.yaml`
(which `verify-opendox-pin.py` check 5 reads as a **blob** at whatever commit
openxFactory's `openXdox` gitlink names) — registered on #656 and **not taken
here**. No pin moves in this release.

### The floor, four parts, one evidence line each

`split-opendox-two-layer-product` § 8.2 — RULED four-part floor (OQ-1), ticked
2026-09-19 by `tasks.md` amendment #8 (openxFactory **#1124** → `3e3b4587`).
**None of these is "the tests passed"** and no part substitutes for another:

1. **The carve manifest** — openxFactory **#865** → `17167481`: 454 rows, every
   file in exactly one disposition and every edit in one of three closed
   classes, at `carve_commit b075fd91dc8fced8e1373825ba80220c33536bae`.
2. **The source→destination test mapping** (§ 5.4) — machine-checked at
   openxFactory **#1080** → `4b53ea99` (exit 0, Σ 4440 over 146 test-bearing
   rows), with clause (d) pinned at all three destinations: `openXdox-code`
   **#25** → `ab04453d` (539/533/6), `openDox-code` **#29** → `373b05aa`, and
   openxFactory's own `pytest-suite.yml` triple.
3. **The neutral conformance corpus green in EVERY destination** (§ 3.7) —
   `OK — 17 of 17` at a LANDED head in each: `openXdox-code` `3ee8cd31`
   (#656 comment `5736681367`), `openDox-code` `93ccc3dd` (re-read at main,
   `5738152229`), openxFactory `eb1880cb`. § 4.5a's own exit evidence is the
   same reading at this project's leg.
4. **The snapshot-equivalence run's matching digests** (§ 5.5) — openxFactory
   **#1105** → `02967478`, with **#1110** → `313b2665` and **#1115** →
   `4101fbe9`. Per RULED **Q-P4 (a)** (#656 comment `5728856581`) this line also
   says § 4.4's domain profile — `domain-profile` above — reproduces
   openxFactory's own vocabulary (`picked` / `staged`, via
   `domain_profile.current()`).

### Provenance

The project was bootstrapped as three repositories by openRepoShape at
`e9c4827b` on 2026-09-06 (`split-opendox-two-layer-product` § 1) and released no
bundle until this one. Its contract bytes arrived by the carve recorded in
[`manifest.yaml`](./manifest.yaml)'s `carved_from:` — `opensoft/openxFactory` at
`b075fd91dc8fced8e1373825ba80220c33536bae`, tag `opendox-carve-0`, 92 code rows
and 47 spec rows.

### The tag

`xdox-v1.0`, annotated, in `opensoft/openXdox`, over the root commit that
carries these entries and names both legs. **Cut by the operator; no workflow
makes it.** Runbook `docs/opendox-cutover-runbook.md` § 9 Phase 6. Rollback is
narrow and is written first: delete the tag only before anything pins it — *a
published bundle is not unpublished*; after that the honest reversal is a
following release.
