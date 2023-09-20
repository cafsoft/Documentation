# Foundation (CAFSoft)

CAFSoftFoundation is a software development framework written in Java, designed to offer some network communication functionalities similar to those of Apple's Foundation framework.

[![](https://jitpack.io/v/cafsoft/CAFSoftFoundation.svg)](https://jitpack.io/#cesarfrancoe/CAFSoftFoundation)


[https://jitpack.io/#cafsoft/CAFSoftFoundation](https://jitpack.io/#cesarfrancoe/CAFSoftFoundation)


To install the library add: 

- Add https://jitpack.io repository in settings.gradle

```gradle
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
        maven { url 'https://jitpack.io' }
    }
}
```

- Add CAFSoftFoundation library in build.gradle (Module)

 
   ```gradle
   dependencies {
         implementation 'com.github.cafsoft:CAFSoftFoundation:2.5.9'
   }
   ```

- Add Java 11 Compatibility in build.gradle (Module)
  ```
  compileOptions {
        sourceCompatibility JavaVersion.VERSION_11
        targetCompatibility JavaVersion.VERSION_11
  }
   ```
Note: Remember to save and sync after making the above settings.

  
## Example codes

- [PRE request using default URLSession](https://github.com/cafsoft/Documentation/blob/main/Foundation/URLSession/Example_1_PRE_Request.md)
- [POST request using default URLSession](https://github.com/cafsoft/Documentation/blob/main/Foundation/URLSession/Example_1_POST_Request.md)
