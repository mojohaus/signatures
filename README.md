# Animal Scents: API signatures

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://opensource.org/licenses/MIT)

Signature artifacts for the Java Runtime and other platforms, such as `java17` or `java16-sun`.
[Animal Sniffer](https://www.mojohaus.org/animal-sniffer/) consumes them to check that code calls
only APIs that exist on a target platform. Each directory is a separate Maven project; there is
no root POM and no aggregated build.

The released signatures, with their versions and usage examples, are on the
[project site](https://www.mojohaus.org/signatures/). Released signatures are on Maven Central under
[`org.codehaus.mojo.signature`](https://repo1.maven.org/maven2/org/codehaus/mojo/signature/).

## Using a signature

Reference a signature from the Animal Sniffer Maven plugin:

```xml
<plugin>
  <groupId>org.codehaus.mojo</groupId>
  <artifactId>animal-sniffer-maven-plugin</artifactId>
  <executions>
    <execution>
      <id>check-java17</id>
      <phase>test</phase>
      <goals>
        <goal>check</goal>
      </goals>
      <configuration>
        <signature>
          <groupId>org.codehaus.mojo.signature</groupId>
          <artifactId>java17</artifactId>
          <version>1.0</version>
        </signature>
      </configuration>
    </execution>
  </executions>
</plugin>
```

## Building

No CI workflow here builds the modules; the only workflow labels pull
requests for release notes. A module generates its signature from the JDK named in its POM
(for example `[1.7,1.8)` in `java17`) through a Maven toolchain, so it builds only where that old JDK
is registered in `~/.m2/toolchains.xml`; `mvn verify` in `java17` on a current JDK fails with
`Could not find jdk toolchain to match {version=[1.7,1.8)}`. `j2ee14` depends on an unreleased
`java14` snapshot that is no longer hosted, so it does not resolve at all. `j2ee14` and `javaee5`
have never been released to Maven Central.

Only `signatures-parent` builds on a current JDK (`mvn verify` there passes). The per-module sites
are rendered from a shared Velocity template, `src/site/markdown/index.md.vm`, which is why those
pages keep the `.vm` extension.

See [AGENTS.md](AGENTS.md) for contributor notes.

## License

[The MIT License](https://opensource.org/licenses/MIT).
