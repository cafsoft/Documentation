# Setting Up CAFSoftFoundation in Android Studio

This guide will help you configure your Android Studio project to use the **CAFSoftFoundation** library.  
Depending on how your project was created, your Gradle build scripts may be written in either:

- **Groovy DSL** (`.gradle`)
- **Kotlin DSL** (`.gradle.kts`)

This guide provides instructions for both formats.

---

## 1. Add JitPack Repository

CAFSoftFoundation is hosted on [JitPack](https://jitpack.io), so you need to include it in your **project-level** `settings.gradle` or `settings.gradle.kts` file.

### Groovy DSL (`settings.gradle`)

```groovy
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        maven { url 'https://jitpack.io' }
    }
}
```

### Kotlin DSL (`settings.gradle.kts`)

```kotlin
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        maven(url = "https://jitpack.io")
    }
}
```

---

## 2. Add CAFSoftFoundation Dependency

Next, include the CAFSoftFoundation dependency in your **module-level** `build.gradle` or `build.gradle.kts` file.

> Replace `2.6.0` with the latest version available on [JitPack](https://jitpack.io/#cafsoft/CAFSoftFoundation).

### Groovy DSL (`build.gradle`)

```groovy
dependencies {
    implementation 'com.github.cafsoft:CAFSoftFoundation:2.6.0'
}
```

### Kotlin DSL (`build.gradle.kts`)

```kotlin
dependencies {
    implementation("com.github.cafsoft:CAFSoftFoundation:2.6.0")
}
```

---

## 3. Set Java Compatibility (Java 11)

CAFSoftFoundation requires Java 11. Ensure your module's Gradle file is configured accordingly.

### Groovy DSL

```groovy
android {
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_11
        targetCompatibility JavaVersion.VERSION_11
    }
}
```

### Kotlin DSL

```kotlin
android {
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_11
        targetCompatibility = JavaVersion.VERSION_11
    }
}
```

---

## 4. Add Internet Permission (if needed)

If your app performs network operations, make sure to add the following permission in your `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

---

## 5. Sync Your Project

Once you’ve made the changes above, **sync your Gradle project** to apply the configurations.

In Android Studio:  
**File > Sync Project with Gradle Files**

---

## Notes

- You can check the latest version of CAFSoftFoundation here:  
  [https://jitpack.io/#cafsoft/CAFSoftFoundation](https://jitpack.io/#cafsoft/CAFSoftFoundation)

- If you encounter any issues, make sure your Gradle and Android Gradle Plugin are up to date.
