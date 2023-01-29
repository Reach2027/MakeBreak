# Hilt

## 官方地址 <https://github.com/google/dagger>

## Gradle引用

```Kotlin
buildscript {
    repositories {
        mavenCentral()
    }

    dependencies {
        // Hilt gradle plugin
        classpath 'com.google.dagger:hilt-android-gradle-plugin:2.44.2'
    }
}

plugins {
  // Hilt gradle plugin
  id 'com.google.dagger.hilt.android' version '2.44.2' apply false
}

```

```Kotlin
dependencies {
    val version = "2.44.2"

    // Hilt Android
    implementation("com.google.dagger:hilt-android:$version")
    kapt("com.google.dagger:hilt-compiler:$version")

    // Hilt JVM
    implementation("com.google.dagger:hilt-core:$version")
    kapt("com.google.dagger:hilt-compiler:$version")
}

kapt {
    correctErrorTypes true
}
```
