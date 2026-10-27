# Semantic versioning numbers a release `MAJOR.MINOR.PATCH` and raises the major number only for a change that breaks compatibility

Semantic versioning writes the version of a release as three numbers, `MAJOR.MINOR.PATCH`.
The number that a release raises tells a user whether the release keeps what the previous release offered.

| Number | Raised for | Example | What a user of the software does |
|---|---|---|---|
| Major, the first | a change of the public interface that breaks compatibility | 2.1.286 to 3.0.0 | checks how the software is used before upgrading |
| Minor, the second | new functionality that keeps compatibility | 2.1.286 to 2.2.0 | upgrades and gains the new functionality |
| Patch, the third | a fix of incorrect behaviour that keeps compatibility | 2.1.285 to 2.1.286 | upgrades without noticing a change |

Raising a number resets every number to its right to 0.
A minor release therefore resets the patch number, and a major release resets the minor and the patch numbers.
A release with the major number 0, such as 0.4.1, is in its initial development, and anything in it may change at any time.
Version 1.0.0 is due once the software is used in production or users depend on its interface.

A hyphen and identifiers after the three numbers mark a pre-release, such as 1.0.0-alpha, which precedes the release 1.0.0.
A plus sign and identifiers after the three numbers or the pre-release mark build metadata, such as 1.0.0+20261008, which plays no part in the order of versions.

Semantic versioning is a convention that a project declares, and a project may number its releases in another way.
A version of three numbers alone does not show that its project follows the convention.

---

Source: Semantic Versioning 2.0.0, <https://semver.org>, read on 2026-10-08.
