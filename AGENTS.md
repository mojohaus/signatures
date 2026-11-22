# Agent guide for signatures

This repository holds one small Maven artifact per JDK/JEE signature — for example `java12`, `java15-harmony` or `j2ee14` — used by tools such as Animal Sniffer to check that code only calls APIs available on a target platform. Each directory is its own project with a `pom.xml` and `src`; there is no root reactor build.

## Build and test

- Work on one signature directory at a time; build it from inside that directory. A signature module generates its signature from the JDK named in its POM
  through a Maven toolchain, so it builds only where that JDK is registered; `signatures-parent` builds anywhere.
- A signature change is a compatibility contract for every consumer that pins it. Do not add, remove or rename a class in a signature without a deliberate version change.

## Documentation updates

A change that users or contributors can notice ships with the documentation it affects, in the same
pull request. A refactor with no visible effect needs none.

- The signature list and its versions are in `signatures-parent/src/site/markdown/index.md`; keep it
  in line with what is on Maven Central, and mark a signature that has no release as not yet
  released instead of showing a snapshot version.
- General usage examples are in `signatures-parent/src/site/markdown/usage.md`.
- Every signature module renders its own site from the same template,
  `<module>/src/site/markdown/index.md.vm`. It stays a Velocity page because it fills in the
  module's coordinates and description; change the 22 copies together, and keep `${...}` references
  free of hyphens, which the current site plugin cannot parse.
- `signatures-parent/src/site/site.xml` holds the menu; `README.md` describes the repository and its
  build limits; update this file when a convention changes.
