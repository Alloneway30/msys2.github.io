# License Metadata

Software we package is always licensed and there are two places where the
licenses are recorded in a package:

* The `license` array field in the package metadata, as specified in the
  PKGBUILD file
* Copies of the license text contained in the package, usually under
  `<prefix>/share/licenses/<real-package-name>/*`. In some cases packages install
  their license files in different places though.

## Guidelines for Specifying License Metadata

#### The license array field

* Values starting with `spdx:` are [SPDX license
  expressions](https://spdx.github.io/spdx-spec/v3.0/annexes/SPDX-license-expressions/).
  Example: `('spdx:GPL-2.0-or-later')`
* Multiple values starting with `spdx:` are treated as if they are combined with
  `OR`. Example: `('spdx:LGPL-2.1-only', 'spdx:MPL-1.1')` is the same as
  `('LGPL-2.1-only OR MPL-1.1')`
* Mixing of spdx values and other values is undefined
* Values not starting with `spdx:` or starting with `custom:` follow the Arch
  Linux rules.
* In case of a split package each package can have a different license field
  depending on the package content. The global license field is inherited by
  each split package but can be overridden there.

#### The license text files

In case the package provides one or more license text files, they can be
installed to `<prefix>/share/licenses/<real-package-name>/*`.
`real-package-name` is the package name without the environment specific prefix,
so in case of `mingw-w64-ucrt-x86_64-meson` it is just `meson`. Example:
`/ucrt64/share/licenses/meson/COPYING`

These guidelines are inspired by

* https://www.python.org/dev/peps/pep-0639/#advanced-example
* https://getcomposer.org/doc/04-schema.md#license

### Differences compared to Arch Linux

See https://wiki.archlinux.org/title/PKGBUILD#license

* Arch Linux doesn't use SPDX identifiers, but manages its own license list in a
  [licenses](https://archlinux.org/packages/core/any/licenses) package.
  This list is not very exhaustive and is missing various variants of common licenses
  such as `GPL-2.0-or-later` vs `GPL-2.0-only`.
* Everything else is prefixed with `custom:`, e.g. `custom:name of license`.
* It's unclear (does anyone know?) if multiple licenses mean that the package is
  available under either license or you must comply with both licenses.

Given the above shortcomings we use our own rules inspired by other packaging
systems.

## FAQ

#### The package includes different components with different licenses, what should I do?

Let's say the package source is licensed under `Apache-2.0`, there is a vendored
library that is licenses under `MIT`, and there is documentation that is
licensed under `CC-BY-4.0`.

You can set the license field to `spdx:Apache-2.0 AND MIT AND CC-BY-4.0`.

In case the documentation is split out into a separate package then you can use
`spdx:Apache-2.0 AND MIT` for the main package and `spdx:CC-BY-4.0` for the
documentation package.

#### How can I specify a custom license in an SPDX expression that is not on the SPDX license list?

You can define your own license ID with the following format
`LicenseRef-<idstring>` where `idstring` is allowed to contain `[A-Za-z0-9.-]`.
For example: `LicenseRef-my-special-license`

#### The projects is licensed under `GPL-2.0-or-later` and only advertises that but contains `MIT` and `BSD` code as well. Should I include them all?

We can't be expected to hunt down every license in every file of every project. In case the project says it's `GPL-2.0-or-later` then it's fine to just use that.
