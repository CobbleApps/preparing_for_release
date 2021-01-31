# Getting ready to release
Preparing Flutter application for releasing on Google Play

## Add Launcher Icon

Assuming you already have a PNG image for your app icon.
You can create icon resources using 
[this link](https://romannurik.github.io/AndroidAssetStudio/icons-launcher.html#foreground.type=clipart&foreground.clipart=android&foreground.space.trim=1&foreground.space.pad=0.25&foreColor=rgba(96%2C%20125%2C%20139%2C%200)&backColor=rgb(68%2C%20138%2C%20255)&crop=0&backgroundShape=square&effects=none&name=ic_launcher)

Download the zipped file and unzip this to 
```
cd \android\app\src\main\res\
start .
```
## Adding Launch Screen
Make a file named color.xml in the following folder
```
cd \android\app\src\main\res\values
start .
```
Add the following code(Updating your backgrounf color)
```
<?xml version="1.0" encoding="utf-8"?>

<resource>
    <color name="launch_background_color">
        #303030
    </color>
</resource>
```
Save your app logo in the following file
```
cd \android\app\src\main\res\drawable
start .
```
Update the drawable with the following code
```
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
Create a file named key.properties in the following location.
```
\android\
```

save the following to the key.properties file

```
storePassword=<password from previous step>
keyPassword=<password from previous step>
keyAlias=key
storeFile=<location of the key store file, such as /Users/<user name>/key.jks>
```

## Configure App level build.gradle
Add code before android block in the file:
```
\android\app\build.gradle
```
With the keystore information from your properties file

```
   def keystoreProperties = new Properties()
   def keystorePropertiesFile = rootProject.file('key.properties')
   if (keystorePropertiesFile.exists()) {
       keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
   }

   android {
         ...
   }
```
Add code before buildTypes block:
```
   signingConfigs {
       release {
           keyAlias keystoreProperties['keyAlias']
           keyPassword keystoreProperties['keyPassword']
           storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
           storePassword keystoreProperties['storePassword']
       }
   }
   buildTypes {
       release {
           signingConfig signingConfigs.release
       }
   }
```

## Review AndroidManifest.xml

location:
```
cd \android\app\src\main\ 
start . 
```
Thing to review :  

1.android:label="app_name_to_be_displayed"

2.android:icon="@mipmap/ic_launcher">

3.package="com.cobble.package_name">


Add required permissions:

```
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.ACCESS_MEDIA_LOCATION"/>
```

## Building the app

First buidinf run following commands
```
flutter pub get
flutter clean
```
Build apk using 
```
flutter build apk --split-per-abi
```
Build app bundle (Recommended for Play Store)
```
flutter build appbundle
```
Once built files are saved at :
```
cd build\app\outputs\bundle\release\ 
start .
```
## When adding update 
When uploading a new updated version of the application change the pubspec.yaml file version number.

And increment the count as follows.

```
version: 1.0.1+2
#Earlier it was 1.0.0+1
```
