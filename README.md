# cargo-unmaintained

**Find unmaintained dependencies in Rust projects**

This tool defines an unmaintained dependency X as one that satisfies the following two conditions:

1. X relies on a version of a package Y that is incompatible with the Y's latest version.
2. Either X has no associated repository, or its repository's last commit was over a year ago (a configurable value).

The purpose of the second condition is to give package maintainers a chance to update their packages. That is, an incompatible upgrade to one of X's dependencies could require time-consuming changes to X. Without this check, `cargo-unmaintained` would produce many false positives.

Note that the above conditions never consider a "leaf" package (i.e., a package with no dependencies) unmaintained.

This tool is meant be a complement to [`cargo-audit`], whose [RustSec Advisory Database] includes notifications for unmaintained packages. A key difference is that `cargo-unmaintained` tries to identify such packages automatically, instead of requiring a human to identify them manually.

## Example

The following is the output produced by running `cargo-unmaintained` on the head of the [Cargo repository] on 2023-10-20:

```
sized-chunks (https://github.com/bodil/sized-chunks updated 539 days ago)
    bitmaps (requirement: ^2.1.0, version used: 2.1.0, latest: 3.2.0)
im-rc (https://github.com/bodil/im-rs updated 539 days ago)
    bitmaps (requirement: ^2, version used: 2.1.0, latest: 3.2.0)
    sized-chunks (requirement: ^0.6.4, version used: 0.6.5, latest: 0.7.0)
partial_ref_derive (https://github.com/jix/partial_ref updated 825 days ago)
    syn (requirement: ^1.0.40, version used: 1.0.109, latest: 2.0.38)
matchers (https://github.com/hawkw/matchers updated 967 days ago)
    regex-automata (requirement: ^0.1, version used: 0.1.10, latest: 0.4.1)
serde-value (https://github.com/arcnmx/serde-value updated 1191 days ago)
    ordered-float (requirement: ^2.0.0, version used: 2.10.0, latest: 4.1.1)
rusty-fork (https://github.com/altsysrq/rusty-fork updated 1241 days ago)
    quick-error (requirement: ^1.2, version used: 1.2.3, latest: 2.0.1)
anes (https://github.com/zrzka/anes-rs updated 1421 days ago)
    bitflags (requirement: ^1.2, version used: 1.3.2, latest: 2.4.0)
```

## Usage

```
Usage: cargo unmaintained [OPTIONS]

Options:
      --max-age <DAYS>  Age in days that a repository's last commit must not exceed for the
                        repository to be considered current; 0 effectively disables this check,
                        though ages are still reported [default: 365]
      --no-exit-code    Do not set exit status when unmaintained dependencies are found
      --no-warnings     Do not show warnings
      --tree            Show paths to unmaintained dependencies
      --verbose         Show information about what cargo-unmaintained is doing
  -h, --help            Print help
  -V, --version         Print version

The `GITHUB_TOKEN` environment variable can be set to the path of a file containing a personal
access token, which will be used to authenticate to GitHub.

Unless --no-exit-code is passed, the exit status is 0 if-and-only-if no unmaintained dependencies
were found and no irrecoverable errors occurred.
```

## License

`cargo-unmaintained` is licensed and distributed under the AGPLv3 license. [Contact us](mailto:opensource@trailofbits.com) if you're looking for an exception to the terms.

[Cargo repository]: https://github.com/rust-lang/cargo
[`cargo-audit`]: https://github.com/rustsec/rustsec
[RustSec Advisory Database]: https://github.com/RustSec/advisory-db/
