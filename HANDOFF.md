# Handoff: PR 535 review and follow-up fixes

State as of 2026-11-08. The work has stopped at a clean point and is being passed to someone else. Nothing is running.

This file lives only on the fork's `handoff/pr535` branch. That branch starts from `review/pr535-fixes` plus this one commit, so it is not part of PR 544.

## Read this first

- **PR 544 is closed and was not merged.** The `menesdeniz1` account closed it at 2026-11-08 19:29:09 UTC, with no comment. The reason is not recorded here. Ask the user before reopening it or opening another PR.
- **Do not push to `review/pr535-fixes`.** It is the head of PR 544, which the repository owner can still see, so keep it at `012c84f9`.
- **One security finding is with the user for private reporting. Its details are not here.** The user has not decided yet and nothing has been sent. Do not raise it in a public place. `SECURITY.md` asks for private reporting.
- **Do not push, comment, review or message on `emindeniz99/apple-purchase-receipt-verifier`** unless the user says so. PR 544 was opened on one explicit approval, and that approval covered that PR only.
- **Do not delete anything:** branches, clones, caches or logs. No force-push.

## Pull requests

| PR | Where | Base | Head | State |
|---|---|---|---|---|
| [544](https://github.com/emindeniz99/apple-purchase-receipt-verifier/pull/544) | Upstream | `claude/rust-core-migration-plan-lkq2zr` (`8c1de982`) | `menesdeniz1:review/pr535-fixes` (`012c84f9`) | **Closed, not merged**, no owner response, 21 commits, 52 files |
| [1](https://github.com/menesdeniz1/apple-purchase-receipt-verifier/pull/1) | Fork | `main` (`fccf0c2b`) | `review/pr535-fixes` (`012c84f9`) | Open, CI only, not for upstream, keep it |
| [535](https://github.com/emindeniz99/apple-purchase-receipt-verifier/pull/535) | Upstream | `main` | `claude/rust-core-migration-plan-lkq2zr` (`8c1de982`) | Open, the owner's PR being reviewed |

On PR 544, the only comment is CodeRabbit saying it skips non-default base branches. Socket Security passed. The upstream `ci` workflow is not listed among its checks, so **the upstream CI has not run on PR 544**.

## The 21 commits

| Commit | Subject | Verified by |
|---|---|---|
| `4b8478f3` | fix(repo): fail the cert copy check when rust/certs is not a full copy | CI run 1 |
| `92374d0f` | fix(repo): keep checking revocation lists after one download fails | CI run 1 |
| `c406a57f` | fix(repo): ban Go revocation list parsing in the one-implementation gate | CI run 1 |
| `1137d057` | docs(repo): keep settings and last good lists when refreshing revocation | Not compiled |
| `0b744bc8` | test(repo): lint the role of revocation list and issuer fixtures | CI run 1 |
| `047859ec` | fix(java): refuse an added issuer whose path length overflows an int | CI run 1 |
| `9bb20b92` | docs(repo): document revocation in the Rust README and the watch | Docs only |
| `18f2056c` | docs(repo): correct statements that 0.14.0 revocation made stale | Docs only |
| `84aaea3b` | test(rust): pin the revocation rules no case reached | CI run 1 |
| `b04314c1` | fix(rust): refuse a member named twice in the aprv-server config line | CI run 1 |
| `ed36972a` | test(java): pin the revocation rules no case reached | CI run 1 |
| `a67db735` | test(php): compare the revocation instant to the millisecond | CI run 1 |
| `5619b6a3` | test(go): expect the module's refusal for a refused configuration | CI run 1 |
| `88f7a8e0` | test(repo): run revocation and buildConfig cases in the differential | **Not verified** |
| `7277041a` | docs(rust): decide how the C ABI takes revocation settings | Docs only (R48) |
| `275c8650` | feat(rust): pass revocation settings through the C ABI | CI run 4 |
| `a86e5ed6` | test(rust): run the revocation cases through the C ABI | CI run 4, 588/588 |
| `95aa9bd3` | style(java): format the revocation tests the way spotless asks | CI run 4 |
| `04cbd039` | docs(rust): record the added issuer divergence and the C ABI settings | Docs only |
| `670c429a` | style(rust): format the C ABI panic test the way rustfmt asks | CI run 4 |
| `012c84f9` | fix(rust): rename a binding clippy reads as a typo of another | CI run 4 |

These commits finish four items of "Revocation (0.14.0): open after #535" in `ROADMAP.md`:
- C ABI revocation settings
- The `aprv-server` repeated key
- The differential skips
- The `rust/README.md` and `CONTRIBUTING.md` documentation

"Releasing 0.14.0" stays with the owner.

All 21 commits are unsigned, so they do not show Verified.

## CI

All runs are on the fork, through fork PR 1, so they test the branch merged with `main`. Upstream `main` is the same commit, `fccf0c2b`.

| Run | Head | Result |
|---|---|---|
| 1, `42500027186` | `5619b6a3` | 140 passed, 5 skipped. `java-checks` failed on spotless only |
| 2, `42501796190` | `04cbd039` | `cargo fmt` failed in `rust-lint` and `rust-ffi`. Recorded as cancelled |
| 3, `42502081539` | `670c429a` | `clippy::similar_names` failed, so the C ABI did not compile |
| **4, [`42503275358`](https://github.com/menesdeniz1/apple-purchase-receipt-verifier/actions/runs/42503275358)** | **`012c84f9`** | **Green: 131 passed, 5 skipped, 0 failed** |

- **Skipped in run 4:** `smoke-crates`, `swift-macos`, `swift-ios`, `ruby-macos`, `go-platforms`.
- **C ABI harnesses:** C++ and ctypes both report 588 passed, 0 failed, 0 skipped.
- **CodeQL:** run `42503275297` succeeded on `012c84f9`.

**No tests were run locally.** The permission system refused local runs of repository code, and `cargo`, `java`, `mvn` and `go` are not installed on that machine. Fork CI is the only evidence.

## Not verified or not done

- **Differential (`88f7a8e0`):** `java-differential` only runs in the nightly, and the nightly is off on the fork. The fork lists only `ci` and `CodeQL Advanced`, and `gh workflow run nightly.yml` returned 404. The new code and the 129 added comparisons have never run, and they may surface new divergences.
- **README examples (`1137d057`):** none were compiled. The riskiest are .NET `DownloadAsync`, Ruby `download_list` and Swift `holder.lastGood`.
- **Elixir example:** not bound to the new C ABI, so it still skips the revocation cases.
- **Fuzz:** `rust/fuzz/fuzz_targets/ffi.rs` does not cover the new C ABI exports.
- **Upstream CI:** it has not run on PR 544.

## Left for the owner's decision

These were not changed. All but the last are also listed in the PR 544 description.

- **R48 (new):** the C ABI design needs the owner's review.
- **Java `pathLenConstraint` divergence:** Java refuses an added issuer whose value overflows an int, and the core accepts it. This is recorded in R47, with no shared case. Java can be switched to accept it instead.
- **§6 "Nothing is lost":** this does not hold for a stale `certificateHold` on a WWDR intermediate. Either keep the list or state the limit.
- **Times outside RFC 5280 in a signed list:** Java and the core differ. This is not in R20 or R47 and has no shared case.
- **Go and Swift from source:** the committed modules lag the new wrappers until a release refreshes the pins.
- **One rejected list stops a whole refresh:** a README sentence was added and the behaviour was not changed.
- **`aprv-server` first `init` trace:** it exits 2 where the README says 70.
- **Note on PR 544 being closed:** the owner can still read its description, but nothing is waiting on the owner while it is closed.
- **§5 and §11 wording:** a non-null `revocationValidUntil` does not mean every accepted path is covered.
- **Smaller items:**
  - `/v1/info` is not compared on `component_sha256`.
  - The revocation watch workflow only goes red and does not check `nextUpdate`.
  - No test accepts a real Apple list.
  - `RevocationTest` hard-codes list contents.
  - The `java/README.md` CVE note names a class the code does not use.
- **The security finding above:** this is the user's decision first. It is not in the PR 544 description, which only says one further finding is not described.

## Process notes

- **`cd` rule broken:** workers were told not to `cd` into clones. 15 workers ran 332 such commands during the analysis of `main`, and 6 workers ran 61 during the PR work. The lead did it once. All were read or git commands. A brief alone did not enforce the rule, so it needs a permission rule.
- **Refused actions, none bypassed:**
  - `gh repo fork`
  - `git remote add fork`
  - PR binding
  - A local `node --test`
  - `gh pr create` from this session

  The user approved the remote directly. The parent session opened the fork, and later opened PR 544 after the user approved it.
- **Leftovers:** a few temporary files remain on the machine (`/tmp/main.txt`, `/tmp/x_notes`, `/tmp/n`), plus a `__pycache__` folder in a worker's tools directory. They were kept because of the no-delete rule.
- **CPU limit from the user:** run at most one worker at a time and no heavy local commands. Keep verification on fork CI.
- **Review notes are not published.** The fork is public, so the full review and the analysis report stay on the user's machine. Ask the user for them.
- **Clones on the user's machine:**
  - `apple-pr535` has `origin` upstream with push disabled, and `fork` for pushes.
  - `apple-purchase-receipt-verifier` is the read-only analysis clone on `main`. Its `origin` push URL is still upstream, so do not push from it.

## Next steps

1. Ask the user why PR 544 was closed and whether the work should go upstream at all. Do not reopen it, change the branch or comment until the user decides.
2. Get the user's decision on the private security report.
3. If the user turns the nightly on for the fork, run the Differential and record the result. Otherwise `88f7a8e0` stays unverified. New divergences go into R20 or R47, or get fixed. Never edit a case to make an implementation pass.
4. Compile the README examples through a CI job, starting with the three named above.
5. Bind the Elixir example to `aprv_verifier_new_config` and extend `rust/fuzz/fuzz_targets/ffi.rs`. Use a new branch and do not add to PR 544 without the user's approval.
