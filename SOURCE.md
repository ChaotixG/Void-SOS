# Obtaining source code

A Void SOS image contains software licensed under the GNU General Public
License, the GNU Lesser General Public License, and other licenses that entitle
you to the corresponding source code. This document explains how to get it.

## What is covered

Every third-party component in a Void SOS image whose license requires source
availability. That includes, among many others:

| Component | License |
|---|---|
| Linux kernel | GPL-2.0 |
| GNU Bash, coreutils, grep, sed, tar | GPL-3.0 |
| GNU C Library (glibc) | LGPL-2.1 |
| Kanidm | MPL-2.0 |
| Tailscale | BSD-3-Clause |
| containerd, Docker, runc | Apache-2.0 |
| Tor | BSD-3-Clause |

## Exactly which versions

Every third-party component is pinned by version, upstream URL and SHA-256 in
`sources.lock`, attached to each release. It names the precise upstream release
each binary was built from, so you can fetch it from its original author and
check it matches.

## Written offer

For any Void SOS release, and for three years from the date that release was
published, the complete corresponding machine-readable source code for the
copyleft-licensed components in that release, including any patches applied and
the build configuration used to compile them, will be provided to any third
party for no more than the cost of physically performing the distribution.

To request it, open an issue in this repository stating the release version.

## Modifications

Where a copyleft component is patched, the patch is part of the corresponding
source and is supplied with it. Void SOS ships upstream releases unmodified
wherever possible; build configuration is recorded in the build definitions
supplied under the offer above.

Patched in this release:

| Component | Change | Files |
|---|---|---|
| Kanidm 1.11.2 (MPL-2.0) | A password used alone needs 10 characters instead of 15, and zxcvbn's score 3 ("safely unguessable") instead of 4 | `libs/crypto/src/lib.rs` (`PW_SFA_MIN_LENGTH_NIST`), `server/lib/src/idm/credupdatesession.rs` (the score check) |
