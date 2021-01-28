# Getting ready to release
Preparing Flutter application for releasing on Google Play

## Add Launcher Icon

Assuming you already have a PNG image for your app icon.
You can create icon resources using 
[this link](https://romannurik.github.io/AndroidAssetStudio/icons-launcher.html#foreground.type=clipart&foreground.clipart=android&foreground.space.trim=1&foreground.space.pad=0.25&foreColor=rgba(96%2C%20125%2C%20139%2C%200)&backColor=rgb(68%2C%20138%2C%20255)&crop=0&backgroundShape=square&effects=none&name=ic_launcher)

Download the zipped file and unzip this to 
```
\android\app\src\main\res\
```

## Signing the app

Create keystore by running following command

```
keytool -genkey -v -keystore c:\Users\USER_NAME\key.jks -storetype JKS -keyalg RSA -keysize 2048 -validity 10000 -alias key
```
change the keystore directory as per your convinience.
#### Note
The keytool command might not be in your path—it’s part of Java, which is installed as part of Android Studio. For the concrete path, run flutter doctor -v and locate the path printed after ‘Java binary at:’. Then use that fully qualified path replacing java (at the end) with keytool. If your path includes space-separated names, such as Program Files, use platform-appropriate notation for the names. For example, on Mac/Linux use Program\ Files, and on Windows use "Program Files".

The -storetype JKS tag is only required for Java 9 or newer. As of the Java 9 release, the keystore type defaults to PKS12.

### Create key.properties
Create a file named key.properties int he following location.
```
\android\
```

save the following the key.properties file

```
storePassword=<password from previous step>
keyPassword=<password from previous step>
keyAlias=key
storeFile=<location of the key store file, such as /Users/<user name>/key.jks>
```
