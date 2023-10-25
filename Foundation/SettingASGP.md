# Settings in an Android Studio Gradle project

## Add repository
Add jitpack.io in settings.gradle

```gradle
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        mavenCentral()
        maven { url 'https://jitpack.io' }
    }
}
```

## Add dependency
Add CAFSoftFoundation dependency in build.gradle (Module)

 
```gradle
dependencies {
    implementation 'com.github.cafsoft:CAFSoftFoundation:2.6.0'
}
```

## Add java version compatibility
Add Java 11 Compatibility in build.gradle (Module)
```gradle
compileOptions {
    sourceCompatibility JavaVersion.VERSION_11
    targetCompatibility JavaVersion.VERSION_11
}
   ```

## Add Internet permission
Add Internet perssion in AndroidManifiest.xml
```xml
<manifest ... >

    <uses-permission android:name="android.permission.INTERNET" />

    <application ... >
        
    </application>
</manifest>
```


**Remember to save and sync**
