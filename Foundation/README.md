# Foundation (CAFSoft)

CAFSoftFoundation is a software development framework written in Java, designed to offer some network communication functionalities similar to those of Apple's Foundation framework.

[![](https://jitpack.io/v/cafsoft/CAFSoftFoundation.svg)](https://jitpack.io/#cesarfrancoe/CAFSoftFoundation)


[https://jitpack.io/#cafsoft/CAFSoftFoundation](https://jitpack.io/#cesarfrancoe/CAFSoftFoundation)


To install the library add: 

- Repository to settings.gradle

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

- Library to build.gradle (Module):

 
   ```gradle
   dependencies {
         implementation 'com.github.cafsoft:CAFSoftFoundation:2.5.7'
   }
   ```
## Example codes

[PRE request using default URLSession](https://github.com/cafsoft/Documentation/blob/main/Foundation/URLSession/Example_1_PRE_Request.md)
