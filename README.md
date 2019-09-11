# Gradle Demo

> Project to explain how to configure a gradle release plugin and maven plugin.

Requirements:

- [Gradle 5.+](https://gradle.org/)
- [gradle-release-plugin](https://github.com/researchgate/gradle-release)
- [maven-plugin](https://docs.gradle.org/current/userguide/maven_plugin.html)

### Setup

- Remove the `version` property from the file `build.gradle`
- Create the `gradle.properties` file in the project root folder
- Add `version` property into the file created above (eg.: `version=0.0.0-SNAPSHOT`)
- Add into the `build.gradle` file the follow configuration:

Configure the plugins `maven` and `net.researchgate.release`:

```groovy
plugins {
    id 'java'
    id 'maven'
    id 'net.researchgate.release' version '2.6.0'
}
```

After that, add information about the repository that the artifact will be uploaded to.

```groovy
uploadArchives {
    repositories {
        mavenDeployer {
            repository(url: "http://localhost:8081/repository/maven-releases") {
                authentication(userName: "admin", password: "admin123")
            }
        }
    }
}

afterReleaseBuild.dependsOn uploadArchives
```

Initialize a git repository, commit all files that you want. So, now execute the following command to release a new version:

```shell script
./gradlew release
``` 

Check your repository like as `http://localhost:8081/repository/maven-releases/com/experian/br/gradle/demo/1.0.0/demo-1.0.0.jar`,
and too your tag git was created.

# References

- [Interacting with Maven repositories](https://docs.gradle.org/current/userguide/maven_plugin.html#uploading_to_maven_repositories)
- [gradle-release plugin](https://github.com/researchgate/gradle-release#usage)

# Extras

You can run a nexus for tests.

```shell script
# start nexus
docker run -d -p 8081:8081 --name nexus sonatype/nexus3

# get password from user 'admin' for first login
docker exec -it nexus cat /nexus-data/admin.password
```
