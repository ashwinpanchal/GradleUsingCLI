# Setup of basic gradle using cli:

- Clone the repo
- `./gradlew build`
- `./gradlew jar`

- On running the `java -jar build/libs/BuildSystemBasics-1.0-SNAPSHOT.jar` we got error as follows

Exception in thread "main" java.lang.NoClassDefFoundError: okhttp3/OkHttpClient
	at org.example.Main.main(Main.java:15)
Caused by: java.lang.ClassNotFoundException: okhttp3.OkHttpClient
	at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:641)
	at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:188)
	at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:526)
	... 1 more

- We can use a ShadowJar package to include all dependencies in a sinlge jar file

```
plugins {
    id 'java'
    id 'com.github.johnrengelman.shadow' version '7.0.0'
}

shadowJar {
    mergeServiceFiles()
}
```
- Run the following code to run the files now

```
./gradlew clean shadowJar
java -jar build/libs/file.jar
```

If we dont want to use the shadowJar File then we can do something as follows: 

```
jar {
    duplicatesStrategy = 'warn'
    from {
        configurations.runtimeClasspath.collect { it.isDirectory() ? it : zipTree(it) }
    }
    manifest {
        attributes(
            'Main-Class': 'org.example.Main'
        )
    }
}
```

The above code will also work 

## Homework

- Try to build a jar and run it directly from cli and see if the okhttp code is working or not just like it is working by running directly from intellij
- instead of okhttp try to integrate retrofit library to make the same http call