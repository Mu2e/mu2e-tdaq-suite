---
title: mu2e-tdaq-suite
group: artdaq-data-path
tier: C
audience: expert
summary: Placeholder source repository for the mu2e-tdaq-suite Spack bundle; a one-line CMakeLists.txt lets Spack MPD development areas include it, and nothing in mu2edaq-main uses it
---

# mu2e-tdaq-suite

> Placeholder source repository for the `mu2e-tdaq-suite` Spack bundle; a
> one-line `CMakeLists.txt` lets Spack MPD development areas include it, and
> nothing in `mu2edaq-main` uses it.

| | |
|---|---|
| Repository | `Mu2e/mu2e-tdaq-suite` (public, issues enabled, default branch `develop`) |
| Content | `CMakeLists.txt` (one comment line), `README.md` (links to the TDAQ repositories), `LICENSE` (Fermilab BSD) |
| Pinned by | `mu2edaq-main` `HEAD` and `t01.00.00` at `997faeb` (2026-02-16). Upstream `develop` is 2 commits ahead (`7deabce`, README link update, 2026-09-30) |
| Tier | C (stub note) |

## What it is and why it exists

`CMakeLists.txt:1` is the whole build:

```
# Blank CMakeLists.txt file to make Spack MPD happy
```

The **Spack package** `mu2e-tdaq-suite` is a different object from this git
repository. It is a `BundlePackage` in `Mu2e/mu2e-spack`
(`packages/mu2e-tdaq-suite/package.py`, default branch `artdaq/Spack0.28`).
It has no source of its own. It declares versions `v4_00_00` … `v15_00_00`
and `develop`, and pins, per version, the art suite, artdaq, otsdaq suite,
`mu2e-pcie-utils`, `artdaq-mu2e`, `otsdaq-mu2e` and the `otsdaq-mu2e-*`
subsystem packages. Installing the TDAQ stack means installing that bundle:
`spack add mu2e-tdaq-suite@${tag} …`
(`otsdaq-mu2e/tools/mu2e-quick-spack-start_v1.1.sh:287`), with the tag taken
from `spack list --format=version_json mu2e-tdaq-suite` (`:168`).

For development, the same script builds a Spack MPD area. It clones the
package repositories into `srcs/` (`:308-321`), and with `--all-packages` that
list includes `mu2e-tdaq-suite` (`:319`). `install_mu2e_ops_dev.sh:37` in
otsdaq-mu2e shows the resulting `srcs/` with `mu2e-tdaq-suite` in it. Spack
MPD treats each directory in `srcs/` as a CMake project, so the bundle's
repository must exist and contain a `CMakeLists.txt`, even an empty one. That
is this repository's only function. art-daq follows the same pattern:
`art-daq/artdaq-suite` holds only `CMakeLists.txt` ("Empty CMakeLists.txt file
to make Spack MPD happy"), and `art-daq/otsdaq-suite` holds only
`CMakeLists.txt` and `.github/`.

The README's repository list (artdaq-core-mu2e, mu2e-pcie-utils, artdaq-mu2e,
otsdaq-mu2e, otsdaq-mu2e-dqm, mu2e-trig-config and the six otsdaq-mu2e
subsystem packages) is the human-readable equivalent of the bundle's
dependency list.

`otsdaq-mu2e/github-ci/repo.sh:39` lists `mu2e-tdaq-suite` under
`packages_without_ci`.

## In mu2edaq-main

The superproject carries it as a submodule (`.gitmodules:52-54`) and lists it
in `README.md:44` and the bootstrap man page. Nothing in the superproject
builds a Spack MPD area: `testing/common.sh` does not list it, and no script
mentions Spack beyond a sentence in `README.md:19`. The submodule therefore
contributes an empty CMake project and a link list, pinned two commits
behind its own `develop`.

## Review findings

One staged issue: [`ISSUES-PENDING.md`](ISSUES-PENDING.md), TDS-001 (S4),
"declare intent or remove from `.gitmodules`", routed to `Mu2e/mu2edaq-main`.
