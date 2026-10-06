# Staged issues — mu2e-tdaq-suite

Findings from the September 2026 DAQ mega-review (`mu2e-sept-mega-review`),
2026-10-06. Tier C: one issue.

**Routing.** `Mu2e/mu2e-tdaq-suite` has issues enabled and none filed. Its own
purpose is clear and documented in `CMakeLists.txt:1`, so there is nothing to
ask its owner. The open question is whether `mu2edaq-main` should carry it as
a submodule, which is a decision about the superproject's `.gitmodules`. That
repository is public with issues enabled. Its 13 issues, open and closed, were
checked on 2026-10-06, and none concerns this package.

**Fingerprint** is
`sha1("Mu2e/mu2edaq-main:.gitmodules:tdaq-suite-placeholder-submodule")[:8]`
= `1b162eb5`.

---

## TDS-001

Routing: file this against `Mu2e/mu2edaq-main` (the decision is about its
`.gitmodules`).

**Title:** `[submodules] mu2e-tdaq-suite is a Spack MPD placeholder: declare why mu2edaq-main carries it or remove it from .gitmodules`

**Severity:** S4 — cosmetic, stale, hygiene
**Category:** documentation
**Location:** `.gitmodules:52-54` (in `Mu2e/mu2edaq-main`); submodule `mu2e-tdaq-suite` at `997faeb`
**Found by:** mega-review (`mu2e-sept-mega-review`), 2026-10-06

## What
`mu2e-tdaq-suite` contains a one-line `CMakeLists.txt` comment, a README of
links and a licence. It exists so that Spack MPD development areas, which
build every directory in `srcs/` as a CMake project, can include the
`mu2e-tdaq-suite` Spack bundle. The bundle itself is defined in
`Mu2e/mu2e-spack`. `mu2edaq-main` builds no Spack MPD area, so the submodule
has no function there. It is also pinned two commits behind its own
`develop`.

## Evidence
```
$ cat mu2e-tdaq-suite/CMakeLists.txt
# Blank CMakeLists.txt file to make Spack MPD happy
$ git ls-tree t01.00.00 mu2e-tdaq-suite
160000 commit 997faeb0b30d2a39b33629fedc5edbf577340d5b	mu2e-tdaq-suite
$ gh api 'repos/Mu2e/mu2e-tdaq-suite/commits?sha=develop&per_page=3' --jq '.[].sha[0:7]'
7deabce
6ba4505
997faeb

# how it is used outside mu2edaq-main
$ grep -n 'tdaq-suite' otsdaq-mu2e/tools/mu2e-quick-spack-start_v1.1.sh | sed -n 2,4p   # long lines shortened
168:    tag=`spack list --format=version_json mu2e-tdaq-suite|jq ".[]|.latest_version"| ...
287:    spack add mu2e-tdaq-suite@${tag} ${svariant} ${avariant} ${ovariant} ${arch_opt} ${g4_opt}
319:        for pkg in mu2e-trig-config otsdaq-mu2e-calorimeter ... mu2e-tdaq-suite;do

# uses inside mu2edaq-main, outside the submodule
$ grep -rn 'mu2e-tdaq-suite' .gitmodules README.md man/ testing/ *.sh
.gitmodules:52:[submodule "mu2e-tdaq-suite"]
README.md:44:| mu2e-tdaq-suite | [Mu2e/mu2e-tdaq-suite](https://github.com/Mu2e/mu2e-tdaq-suite) |
man/man1/mu2edaq-bootstrap.1:95:mu2e-tdaq-suite	github.com/Mu2e/mu2e-tdaq-suite
```

## Why it matters
Every recursive clone and release tag of `mu2edaq-main` carries an empty
CMake project. Readers of the superproject README can take it for the TDAQ
software itself. The real suite definition lives in `Mu2e/mu2e-spack`, and
nothing in the superproject points there. There is no operational effect:
S4.

## Suggested fix
Choose one:
1. **Remove** the submodule from `mu2edaq-main` (`git submodule deinit`,
   `git rm mu2e-tdaq-suite`, drop the README and bootstrap-page rows). In the
   README, point to `Mu2e/mu2e-spack` `packages/mu2e-tdaq-suite/package.py`
   and to `otsdaq-mu2e/tools/mu2e-quick-spack-start_v1.1.sh` for building the
   TDAQ stack; or
2. **Keep** it, and say in the superproject README that it is the Spack MPD
   placeholder for the `mu2e-tdaq-suite` bundle, not code. Bump the gitlink to
   `develop` (`7deabce`).

<!-- mega-review-fingerprint: 1b162eb5 -->
