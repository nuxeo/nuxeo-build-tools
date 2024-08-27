# nuxeo-build-tools
Nuxeo Build Tools

## Use nuxeo-build-tools artefact with spotless in your project

Configure the Spotless Maven Plugin in your project pom with nuxeo-build-tools content:

```xml
...
  <build>
    ...
    <plugins>
      <plugin>
          <groupId>com.diffplug.spotless</groupId>
          <artifactId>spotless-maven-plugin</artifactId>
          <version>2.43.0</version>
          <dependencies>
            <dependency>
              <groupId>org.nuxeo</groupId>
              <artifactId>nuxeo-build-tools</artifactId>
              <version>VERSION</version>
            </dependency>
          </dependencies>
          <configuration>
            <java>
              <endWithNewline/>
              <indent>
                <spaces>true</spaces>
                <spacesPerTab>4</spacesPerTab>
              </indent>
              <removeUnusedImports />
              <importOrder>
                <file>nuxeo.importorder</file>
              </importOrder>
              <eclipse>
                <file>nuxeo_formatter.xml</file>
                <version>4.32</version>
              </eclipse>
            </java>
          </configuration>
      </plugin>
    </plugins>
    ...
  </build>
...
```

Then you can check your code:

```bash
mvn spotless:check
```

## Release the project

Make sure the project builds and its tests pass.

Then create a temporary branch to perform the release:

```bash
git checkout -b tmp-release
```

Then update the project version to final, for instance 2:

```bash
mvn versions:set -DnewVersion=2 -DgenerateBackupPoms=false
```

Then commit and tag the release:

```bash
git commit -a -m "Release 2"
git tag -a -m "Release 2" v2
```

Then deploy the maven artefacts:

```bash
mvn clean deploy -DskipTests -DaltDeploymentRepository=maven-public-releases::default::PUBLIC_URL
```

> [!IMPORTANT]
> You should replace the `PUBLIC_URL`.
> Your Maven `settings.xml` file should contain appropriate authentication (if any) for the `maven-public-releases` repository.

Then push the tag:

```bash
git push --tags
```

Then cleanup your branch and prepare the next development iteration:

```bash
git checkout main
git branch -D tmp-release
mvn versions:set -DnewVersion=3-SNAPSHOT -DgenerateBackupPoms=false
git commit -a -m "Post release 2"
git push
```
