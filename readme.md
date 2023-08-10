# NEXUS REPOSITORY TEST

## 저장소 업로드 

[nexus-put/build.gradle](./nexus-put/build.gradle)

```
apply plugin: 'maven-publish'

repositories {
    maven {
        url "http://localhost:8081/repository/maven-public/"
        allowInsecureProtocol true
    }
    mavenCentral()
}

publishing {
    repositories {
        maven {
            if (project.version.toString().endsWith("SNAPSHOT")) {
                url "http://localhost:8081/repository/maven-snapshots/"
            } else {
                url "http://localhost:8081/repository/maven-releases/"
            }
            credentials {
                username project.repoUser
                password project.repoPassword
            }
            allowInsecureProtocol true
        }
    }
    publications {
        maven(MavenPublication) {
            artifact("build/libs/nexus-put-1.0.jar") {
                extension 'jar'
            }
        }
    }
}

publishMavenPublicationToMavenRepository.dependsOn(jar)
```

## 저장소 사용

[nexus-get/build.gradle](./nexus-put/build.gradle)

```
repositories {
    maven {
        url "http://localhost:8081/repository/maven-public/"
        allowInsecureProtocol true
    }

    mavenCentral()
}
```