# promote-nexus-artifact-gilded-rose

* The [artifact promotion](https://github.com/praqma/promote-nexus-artifact) project will use this Gilded Rose project as demonstration.

Forked from upstream [praqma-training/jenkins-workshop](https://github.com/praqma-training/jenkins-workshop) project.

* Get [artifact promotion](https://github.com/praqma/promote-nexus-artifact) project.

```
git clone git@github.com:praqma/promote-nexus-artifact.git
```

* The [Jenkinsfile](Jenkinsfile) has been modified to support Jenkins without Docker support. Please see the upstream [praqma-training/jenkins-workshop](https://github.com/praqma-training/jenkins-workshop) project, if you want to use [Jenkinsfile](Jenkinsfile) with the Docker support.

* Update the current [m2/settings.xml](m2/settings.xml) file as this [Jenkinsfile](Jenkinsfile) use it directly. As an alternative, ensure `~/.m2/settings.xml` file has similar content as shown below. Note that [encrypted password](https://maven.apache.org/guides/mini/guide-encryption.html) generated from Apache Maven environment is supported.

```
<?xml version="1.0"?>
<settings>
  <servers>
    <server>
      <id>workshop-gildedrose</id>
      <username>username</username>
      <password>password</password>
    </server>
  </servers>
  <profiles>
    <profile>
      <id>workshop-gildedrose</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <repositories>
        <repository>
          <id>central</id>
          <url>http://localhost:8081/nexus3/repository/maven-central/</url>
        </repository>
        <repository>
          <id>sponge-alpha</id>
          <url>http://localhost:8081/nexus3/repository/sponge-alpha/</url>
        </repository>
      </repositories>
    </profile>
  </profiles>
</settings>
```
