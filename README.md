```
plugins {
    id 'java'
    id 'war'
}

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(17)
    }
}

repositories {
    mavenCentral()
}

dependencies {
    // Jakarta EE 10の依存関係を追加
    providedCompile 'jakarta.platform:jakarta.jakartaee-api:10.0.0'
    implementation files('../../REST_AP/WebContent/lib/fw.jar')
}

war {
    archiveFileName = 'alarm.war'
    destinationDirectory = file('../war')
    from '../../REST_AP/WebContent'
}

sourceSets {
    main {
        java {
            srcDirs = ['../../REST_AP/src']
        }
        resources {
            srcDirs = ['../../REST_AP/WebContent/classes']
        }
    }
}

task copyLibs(type: Copy) {
    from('../../REST_AP/WebContent/lib') {
        include 'fw.jar'
    }
    into("$buildDir/classes/java/main/lib")
}

task copyResources(type: Copy) {
    from('../../REST_AP/WebContent/classes') {
        include 'log4j2.xml'
        include 'message.properties'
    }
    into("$buildDir/resources/main")
}

processResources.dependsOn copyResources
classes.dependsOn copyLibs
```
