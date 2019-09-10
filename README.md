# LifeScope Universal App for iOS/Android/Windows/Mac


LifeScope Universal app for iOS/Android/Windows/Mac using Cordova.

Learn more
https://lifescopelabs.github.io/app

https://cordova.apache.org/

## Build process

You first need to add the platforms that you want to build for. Run

```cordova platform add android ios osx windows```

to initialize directories in the 'platforms' directory.
If you don't want to build the files for some of those platforms, just leave it out of that command.

Next, run 

```cordova build```

to build the app for the instantiated platforms. 

## Building Android for release
```cordova platform remove android```

```cordova platform add android```

```cordova build android --release --buildConfig```

### Signing Android package

The Android package should be located in lifescope-cordova/platforms/android/app/build/outputs/apk/release,
and it should be called app-release-unsigned.apk.
You'll need to sign this package before it can be uploaded to the Play Store.

You'll need the LifeScope keystore, called ```lifescope-release-keys.keystore```.
You'll also need  the LSAdmin.kdbx KeePass database, which contains passwords necessary to use the keystore.

From the top level of this library run

```jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore ~/Development/Keys/lifescope-release-keys.keystore platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk lifescope-android-key```

You will be asked for the Passphrase for the keystore, which is called 'LifeScope Keystore' in the KeePass db,
and then for the key password for lifescope-android-key, which is called 'LifeScope Android Key' in KeePass.

Once the signing process is finished, you'll have to run zipalign to produce the final package; install
'zipalign' on you machine if it isn't already (sudo apt-get install zipalign, or whatever package manager you machine uses).
Then, run 

```zipalign -v 4 platforms/android/app/build/outputs/apk/release/app-release-unsigned.apk platforms/android/app/build/outputs/apk/release/app-release.apk```

'app-release.apk' is what you'll need to upload to the Play Store.