# Signature Usage

<!--
The MIT License

Copyright (c) 2013 codehaus.org.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
-->

# Usage

The examples below use the `java17` signature, version 1.0; replace them with the signature you need from the
[list of released signatures](index.html).

Signatures can be used by [Animal Sniffer Maven Plugin](https://www.mojohaus.org/animal-sniffer/animal-sniffer-maven-plugin/),
[Animal Sniffer ANT Tasks](https://www.mojohaus.org/animal-sniffer/animal-sniffer-ant-tasks/) or
[Animal Sniffer Enforcer Rule](https://www.mojohaus.org/animal-sniffer/animal-sniffer-enforcer-rule/)

## With Animal Sniffer Maven Plugin

To check your project against signature using the Animal Sniffer Maven Plugin,
include the following configuration in your `pom.xml`

```xml
<project>
  ...
  <build>
    ...
    <plugins>
      ...
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>animal-sniffer-maven-plugin</artifactId>
        <version><!-- latest release --></version>
        ...
        <executions>
          ...
          <execution>
            <id>check-signature</id>
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
          ...
        </executions>
        ...
      </plugin>
      ...
    </plugins>
    ...
  </build>
  ...
</project>
```

## With Animal Sniffer ANT Tasks

Download the signature from the [Maven Central Repository](https://repo1.maven.org/maven2/org/codehaus/mojo/signature/java17/1.0)
and configure the `check-signature` task to use the signature, e.g.

```xml
<project xmlns:as="antlib:org.codehaus.mojo.animal_sniffer">
  <typedef uri="antlib:org.codehaus.mojo.animal_sniffer">
    <classpath path="lib/animal-sniffer-ant-tasks-1.29.1.jar"/>
  </typedef>
  <target name="check-signature" depends="compile">
    <as:check-signature signature="java17-1.0.signature">
      <path path="classes"/>
    </as:check-signature>
  </target>
</project>
```

Alternatively, you can use [Maven ANT Tasks](https://maven.apache.org/ant-tasks/index.html) to download the signatures
for you automatically, e.g.

```xml
<project xmlns:as="antlib:org.codehaus.mojo.animal_sniffer" xmlns:artifact="antlib:org.apache.maven.artifact.ant">
  <!-- define Maven ANT Tasks -->
  <typedef uri="antlib:org.apache.maven.artifact.ant">
    <classpath path="lib/maven-ant-tasks-2.0.10.jar"/>
  </typedef>

  <!-- define Animal Sniffer ANT Tasks -->
  <typedef uri="antlib:org.codehaus.mojo.animal_sniffer">
    <classpath path="lib/animal-sniffer-ant-tasks-1.29.1.jar"/>
  </typedef>

  <!-- define local repository location -->
  <condition property="maven.repo.local" value="${maven.repo.local}" else="${user.home}/.m2/repository">
    <isset property="maven.repo.local"/>
  </condition>

  <!-- configure local repository -->
  <artifact:localRepository id="local.repository" path="${maven.repo.local}"/>

  <!-- declare dependency -->
  <artifact:dependencies filesetId="signatures.fileset">
    <dependency groupId="org.codehaus.mojo.signature" artifactId="java17" version="1.0" type="signature"/>
  </artifact:dependencies>

  <target name="check-signature" depends="compile">
    <as:check-signature signature="${org.codehaus.mojo.signature:java17:signature}">
      <path path="classes"/>
    </as:check-signature>
  </target>
</project>
```

## With Animal Sniffer Enforcer Rule Plugin

To check your project against a signature using the Animal Sniffer Enforcer Rule,
include the following configuration in your `pom.xml`

```xml
<project>
  ...
  <build>
    ...
    <plugins>
      ...
      <plugin>
        <artifactId>maven-enforcer-plugin</artifactId>
        <version><!-- latest release --></version>
        ...
        <dependencies>
            ...
            <dependency>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>animal-sniffer-enforcer-rule</artifactId>
                <version><!-- latest release --></version>
            </dependency>
            ...
        </dependencies>
        ...
        <executions>
           ....
          <execution>
            <id>check-signature</id>
            <phase>test</phase>
            <goals>
              <goal>enforce</goal>
            </goals>
            <configuration>
              <rules>
                <checkSignatureRule implementation="org.codehaus.mojo.animal_sniffer.enforcer.CheckSignatureRule">
                  <signature>
                    <groupId>org.codehaus.mojo.signature</groupId>
                    <artifactId>java17</artifactId>
                    <version>1.0</version>
                  </signature>
                </checkSignatureRule>
              </rules>
            </configuration>
          </execution>
          ...
        </executions>
        ...
      </plugin>
      ...
    </plugins>
    ...
  </build>
  ...
</project>
```
