# Updating CLDR-JSON

## Prerequisites

- A UNIX-like environment
- git
- java and maven
- plenty of memory

## Instructions

### Setup

Check out repos (If you use a different directory layout, see Customization)

1. Check out this repo [`cldr-json`](https://github.com/unicode-org/cldr-json). (Note: Big.)
2. Check out the [`cldr-staging`](https://github.com/unicode-org/cldr-staging) repo as a sibling to cldr-json. This will be the data source. (Note: Very big.) Use a release tag if possible.
3. Check out the [`cldr`](https://github.com/unicode-org/cldr) repo as a sibling to cldr-json and set it up so [maven builds are possible](https://cldr.unicode.org/development/maven).

Make sure your `cldr-json` directory is otherwise clean (`git status`)

### Building

1. `cd` to `cldr-json`
2. *Delete* the `cldr-json/cldr-json` subdirectory (the subdirectory of `cldr-json`).  This way, git will tell you what has changed, and you can see if any files failed to regenerate.
3. Run the script `cldr-generate-json.sh`
4. Data will be updated in the recreated `cldr-json` subdirectory.

#### Troubleshooting

- `git status` is your friend. At least, until too many files change.
- If a `cldr-json/supplemental` or `cldr-json/other` or other such subdirectory shows up not prefixed with `cldr-`, there's probably something wrong with the `cldr-json` mapping.
  - Sadly, there is no documentation about how to update the tooling. TODO: [CLDR-16445](https://unicode-org.atlassian.net/browse/CLDR-16445)

### Publishing

1. Run the script `cldr-generate-zips.sh` to generate zipfiles under `dist/` - these are suitable for uploading to a release page
2. create a release tag such as `43.0.0` in this repository. or `43.0.0-ALPHA2`.  Create a GitHub release, use other [releases](https://github.com/unicode-org/cldr-json/releases) as a guide.
3. npm packages can be updated as well. Each sub-subdirectory of `cldr-json/cldr-json` is a separate npm package. The following script will preview
(dry run) publishing to npm under the `beta` tag. Check the version carefully!

```shell
(cd cldr-json; for repo in $(ls); do (cd $repo; npm publish --tag beta --dry-run); done)
```

### Customization

See `cldr-config.sh` for customization options.

You can create an executable script named `local-config.sh` with
values to update, for example `VERSION`, `TYPES`, `MATCH` or `DRAFTSTATUS`

Example, if you have a different directory layout:

```shell
# VERSION defaults to calculating the version
VERSION=43.0.0-ALPHA2

# CLDR_DIR defaults to ../cldr
CLDR_DIR=../cldr-maint-43

# INDATA defaults to ../cldr-staging/production
INDATA=../cldr-staging-other/production
```

## Licenses

- Usage of CLDR data and software is governed by the [Unicode Terms of Use](http://www.unicode.org/copyright.html)
a copy of which is included as [unicode-license.txt](./unicode-license.txt).

SPDX-License-Identifier: Unicode-DFS-2016

## Copyright

Copyright &copy; 1991-2021 Unicode, Inc.
All rights reserved.
[Terms of use](http://www.unicode.org/copyright.html)
