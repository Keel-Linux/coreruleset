# coreruleset

Debian packaging of the [OWASP Core Rule Set](https://coreruleset.org)
version 4 (LTS line, 4.25.1) for Keel Linux on Debian trixie. Source
package `coreruleset`, binary package `coreruleset`.

## Why Keel carries it

Keel Web runs Coraza inline in Nginx (handbook decision 0030), and
everything Keel installs is a `.deb` in the Keel repository (0039).
Step 5 of the first implementation of 0041 builds Coraza's packages:
this one, [libcoraza](https://github.com/Keel-Linux/libcoraza) and
[libnginx-mod-http-coraza](https://github.com/Keel-Linux/libnginx-mod-http-coraza).
The rules are a package of their own so that a rule update never waits
for, or forces, a rebuild of the module.

## Debian status

Trixie only has `modsecurity-crs` 3.3.7 (CRS 3.3, for ModSecurity), which
Coraza does not target. There is no package of CRS 4 and no ITP. Both can
be installed together: the rules go to `/usr/share/coreruleset/rules`, the
local settings are conffiles in `/etc/coreruleset`, and
`/usr/share/coreruleset/coreruleset.load` includes both in CRS's order.

## Layout

[DEP-14](https://dep-team.pages.debian.net/deps/dep14/), as
git-buildpackage repositories on salsa: `upstream/latest` (upstream
tarballs, imported with `gbp import-orig`), `pristine-tar`, and
`keel/trixie` (this packaging, the default branch). Tags are
`upstream/<version>` and `keel/<debian-version>`.

## Building

On trixie:

```
sudo apt-get install git-buildpackage pristine-tar
gbp clone https://github.com/Keel-Linux/coreruleset.git
cd coreruleset
sudo apt-get build-dep ./
gbp buildpackage -us -uc
```

## Tests

No autopkgtest of its own. The rules are tested by the autopkgtest of
libnginx-mod-http-coraza, whose CI builds this repository's
`keel/trixie`. This repository's CI builds the package in a
`debian:trixie` container and runs lintian, failing on any error or
warning.

## License

Apache-2.0, as upstream (`LICENSE`) and the packaging (`debian/copyright`).
