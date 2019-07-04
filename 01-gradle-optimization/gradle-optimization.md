## Profile, profile, profile!

**Profile your build, numbers don't lie**

```gradle
./gradlew assembleDebug --profile 
```



## Always use the latest Gradle Plugin for Android



## Avoid Legacy Multidex


If your minSdkVersion is set to 20 or lower, then you must add the support library.


Legacy multidex happens when you build your project with multidex enabled and minSdkVersion is set to 20 or lower. Clean and incremental builds are significantly slower on this.


The simplest way to solve this is by creating a build variant for development.


```gradle
android {
    defaultConfig {...}
    buildTypes {...}
    sourceSets {...}
    productFlavors {
        // Create a separate build variant for development which has a minSdkVersion of 21
        dev {
            ...
            minSdkVersion 21
        }

        prod {...}
    }
}
```


Estimates that this will reduce 5-10% of your build time.



## Disable lint checks on development

You may only want to run your lint checks when you are creating a diff, such as running a script with lint checks specified as one of the tasks to execute before diff creation.

```
./gradlew assembleDebug -x lint
```



## Disable multi-APK generation

```gradle
if (project.hasProperty('devBuild')) {
    // Prevent multi apk generation on development
    splits.abi.enable = false
    splits.density.enable = false
}
```



## Include minimal resources

```gradle 
android {
    productFlavors {
        dev {
            ...
            resConfigs ("en", "xxhdpi")
        }
    }
}
```



## Disable PNG crunching

```gradle
android {
    buildTypes {
        release {
            // Disabled by default on debug build type
            crunchPngs false // Enabled by default for RELEASE build type
        }
    }
}
```



## Configure DexOptions wisely

```gradle
android {
    dexOptions {
        preDexLibraries true
    }
}
```

Configure wisely based on your development workflow preference. You should disable when doing clean builds on your CI builds.



## Use Crashlytics only when needed

```gradle
android {
    buildTypes {
        debug {
            ext.enableCrashlytics = false
        }
    }
}
```



## Use static dependency versions

You must declare static or hard-coded version numbers for your dependencies in your build.gradle. You should avoid dynamic dependencies, represented by using plus sign (+)



## Enable build caching

`./gradlew assembleDebug --build-cache`


You can also configure build caching on your `gradle.properties` file.

```gradle
org.gradle.caching=true // Add this line to enable build cache
```



## Configure the heap size for the JVM

if you have a large project, you should update the heap size by setting `org.gradle.jvmargs` property in your `gradle.properties` file.

```
org.gradle.jvmargs=-Xmx2g -XX:MaxMetaspaceSize=512m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8
```



## Modularize your project

Modularizing your project codebase allows the Gradle build system to compile only the modules you modify and cache those outputs for future builds.



## Runners-up: Always-on daemon, parallel build execution, and configure on demand


### Gradle Daemon

By default Gradle Daemon is enabled
```
org.gradle.daemon=true
```


### Parallel Build Execution

This configuration is effective only when your project is modularized
```
org.gradle.parallel=true
```


### Configure On Demand

You will only benefit from this configuration if you have a multi-project builds that have decoupled projects -- take note, it's multi-project, not just a modularized project.



## Conclusion

Keep your team updated with the latest Gradle releases, and lookout for the possible API deprecation. Always profile your build, and adjust the configurations accordingly based on the results you get.
