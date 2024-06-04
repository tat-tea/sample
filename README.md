```
sourceSets {
    main {
        java {
            srcDirs = ['src']
        }
        resources {
            srcDirs = ['src/META-INF', 'WebContent/WEB-INF/classes']
        }
    }
}

war {
    from('WebContent') {
        into('')
    }
    webInf {
        from 'WebContent/WEB-INF'
    }
    classpath {
        from sourceSets.main.output
    }
    archiveFileName = 'myapp.war'
}

```
