# Hilt

## 官方地址 <https://github.com/google/dagger>

## Gradle引用

```Kotlin
/* Apply hilt gradle plugin */
// Project build.gradle.kts
buildscript {
    dependencies {
        classpath("com.google.dagger:hilt-android-gradle-plugin:2.44.2")
    }
}

// Module build.gradle.kts
apply("com.google.dagger.hilt.android")

/* Apply hilt gradle plugin with plugins DSL */ 
// Project build.gradle.kts
plugins {
    id("com.google.dagger.hilt.android") version "2.44.2" apply false
}

// Module build.gradle.kts
plugins {
    id("com.google.dagger.hilt.android")
}

```

```Kotlin
// Module build.gradle.kts
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
    correctErrorTypes = true
}
```
