# All credit's go to Tahlreth.

AetherSX2 Build Instructions

Please note that this is *not* a full source release for the library, only the LGPL sources, and parts of which that have been modified.
Releasing full source code is *not* required by the LGPL, only that the closed source components can be re-linked/combined with
the LGPL components and build instructions included as per section 4/5 of the license.

An apk suitable for injecting the recompiled library is provided as a convenience, as the library cannot be used outside
of the Android app without modification.

You will need:
 - Android SDK, Build Tools and NDK.
 - A Linux machine.
 - CMake

Build steps:

1. Clone the repo and change into that directory: cd aethersx2
2. Create a build directory for the native library and change into it: mkdir build-android; cd build-android
3. Configure the build system. Change PATH_TO_NDK to whereever the NDK is installed: cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_TOOLCHAIN_FILE=/PATH_TO_NDK/build/cmake/android.toolchain.cmake -DANDROID_PLATFORM=android-26 -DANDROID_ABI=arm64-v8a ..
4. Compile the native library: make -j16 (or whatever CPU count you have)
5. Create a directory for packaging the APK: mkdir apk; cd apk
6. Copy the skeleton APK to this directory: cp PATH_TO_app-release-unsigned.apk aethersx2.apk
7. Copy the native library into the correct location: mkdir -p lib/arm64-v8a; cp ../pcsx2/libemucore.so lib/arm64-v8a
8. Add the native library to the APK. zip is used instead of aapt because aapt will compress it: zip -0 aethersx2.apk lib/arm64-v8a/libemucore.so
9. Ensure the native library is aligned to a 4-byte boundary: zipalign -p 4 aethersx2.apk aethersx2-aligned.apk
10. Create a signing key for your build. Mark down the keystore password. keytool -genkey -v -keystore keyname.keystore -alias keyname -keyalg RSA -keysize 2048 -validity 10000
11. Sign the APK, replacing PASSWORD_TO_KEYSTORE with above: apksigner sign --ks keyname.keystore --ks-pass "pass:PASSWORD_TO_KEYSTORE" --ks-key-alias keyname --out aethersx2-signed.apk --verbose aethersx2-aligned.apk

This will produce aethersx2-signed.apk, which can be installed on your device.

-----

Proceed at your own risk.

Original source code was uploaded to archive.org by juliussan January 16th 2023
https://archive.org/details/aethersx2-libemucore.tar
