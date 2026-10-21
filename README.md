# KiroOS manifest

How to build **KiroOS 1.0 Developer Beta 5** for the Samsung Galaxy S6 (`zerofltexx`), S6 edge (`zeroltexx`) and S6 edge+ (`zenltexx`).

KiroOS is built on LineageOS 14.1 (Android 7.1.2). The projects KiroOS changes live in [KiroOS-Project](https://github.com/KiroOS-Project), on the `kiroos-5.0` branch; everything else comes from LineageOS.

## Requirements

- A 64-bit Linux (Ubuntu 20.04 works, also in WSL2), 16 GB of RAM or more, about 300 GB of disk
- OpenJDK 8 and the usual LineageOS 14.1 build packages ([LineageOS build guide](https://wiki.lineageos.org/devices/zerofltexx/build))

## Get the source

```bash
mkdir kiroos && cd kiroos
repo init -u https://github.com/LineageOS/android.git -b cm-14.1 --depth=1
mkdir -p .repo/local_manifests
curl -o .repo/local_manifests/kiroos.xml https://raw.githubusercontent.com/KiroOS-Project/manifest/main/kiroos.xml
repo sync -c -j8 --no-tags
```

## Files that are not in git

Some parts are too big for git. Download them from the [KiroOS release](https://github.com/KiroOS-Project/KiroOS/releases) and put them in the source tree:

- `vendor_kiroos_kai-5.0.zip`, kAI's offline speech model (Whisper) and its two voices (Piper Paola and Kristin, with the espeak-ng data), with their licenses: `unzip vendor_kiroos_kai-5.0.zip -d vendor/kiroos`
- `KOptimizeWebView-1.1.apk`, KOptimize WebView built from Chromium 119: copy it to `vendor/kiroos/webview/KOptimizeWebView.apk`

Without them the build still works: kAI then works without offline speech and with the system voice, and the system uses the stock WebView.

## Build

```bash
source build/envsetup.sh
export KIRO_RELEASE=5.0 KIRO_RELEASE_CHANNEL="Developer Beta 5"
lunch kiro_zerofltexx-userdebug      # kiro_zerolte-userdebug for the S6 edge, kiro_zenlte-userdebug for the S6 edge+
make -j$(nproc) bacon
```

The package is `out/target/product/<device>/KiroOS-1.0-5.0-<date>-<device>.zip`.

## License

The KiroOS changes are released under the licenses of the projects they belong to (Apache 2.0 for Android and LineageOS, GPL v2 for the Linux kernel). The KiroOS apps (`kiroos_packages_apps_KiroOS`) are Apache 2.0. Samsung's proprietary files (`kiroos_vendor_samsung`) remain Samsung's.
