---
title: "Mobile Security Cheatsheet"
author: "Hazy"
pubDatetime: 2025-10-21T00:00:00.000Z
featured: false
draft: false
tags:
  - mobsec
  - appsec
description: "quick-reference notes I use during mobile security tests"
---

TL;DR: I made this cheatsheet in my Obsidian notes to stop Googling the same stuff every time. Just quick copy-paste commands to make mobile pentesting faster and way less painful.

## 🛡️ My Environment Setup

### Environment 
Here's my basic testing setup, nothing too fancy, just what actually works day-to-day:

- **Android devices**: Google Pixel 4 (rooted with Magisk)
- **Emulators**: Android AVD (used to be Genymotion)
- **iOS**: iPhone X
- **Dynamic tools**: Frida + Objection
- **Proxy / interception**: Burp Suite & command-line tools

## 📱 Android Security Testing

I use a Google Pixel 4 rooted with Magisk, I won’t go into the rooting steps here, but it works perfectly with [TrustUserCertificates](https://github.com/lupohan44/TrustUserCertificates) module for Burp’s certificate setup. Once you get that working, HTTPS interception becomes a breeze.

Previously, I relied on Genymotion (great emulator, by the way), but the free version has limits, like missing biometric support, which became a headache when testing banking app flows. So I eventually switched to Android Virtual Device (AVD).

The downside? Installing AVD through Android Studio is a full-on CPU sauna, it feels like baking a cake on laptop. 🍰 So I dug into how to install and run AVD without Android Studio, which saved both disk space and sanity.

1. Download Command line tools only from the [official Android page](https://developer.android.com/studio#command-line-tools-only).
2. Unzip and move cmdline-tools contents so the structure looks like
```bash
/Users/name/Library/Android/sdk/cmdline-tools/latest/bin
```
Notes: Create a folder name latest inside cmdline-tools.

3. Add environment vars to .zshrc
```bash
export ANDROID_SDK_ROOT="$HOME/Library/Android/sdk"
export ANDROID_HOME="$ANDROID_SDK_ROOT"
export PATH="$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/platform-tools:$PATH"
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools/bin
```
then reload 
```bash
source ~/.zshrc
```
4. Install platform-tools and emulator
```
sdkmanager --list
sdkmanager platform-tools emulator
sdkmanager "system-images;android-34;google_apis_playstore;arm64-v8a" "platforms;android-34"

# using `android-34` here which is SDK for Android 14 
avdmanager create avd --name 'pixel' --package "system-images;android-34;google_apis_playstore;arm64-v8a" -d pixel
emulator -avd pixel -no-snapshot-load
```
If you get an error, start with the full path
```bash
$ANDROID_HOME/emulator/emulator -avd pixel -no-snapshot-load
```

5. Root AVD
- Use [rootAVD script](https://gitlab.com/newbit/rootAVD)
```bash
./rootAVD.sh system-images/android-34/google_apis_playstore/arm64-v8a/ramdisk.img
```


## 🍎 iOS Security Testing

I used Palera1n to jailbreak my iPhone X running iOS 16.7.2, check [this](/posts/ios-jailbreak-guide) out.

```bash
palera1n -l
```

## 💡 Frida

Make sure the host Frida CLI and the device’s frida-server are the same version. I stick with v16.6.6 (arm64 / aarch64). 

```bash
adb shell getprop ro.product.cpu.abi
# or
adb shell uname -m
adb push frida-server /data/local/tmp/

# make it executable
adb shell "chmod 755 /data/local/tmp/frida-server"

# run it in background (simple)
adb shell "/data/local/tmp/frida-server &"

# Kill Frida process
ps -e | grep frida-server
kill -9 pid

#Start Frida process
/data/local/tmp/frida-server & 

# To hook a process like com.example.dev

frida -U -f com.example.dev -l script.js
```
The scripts I used:
- Flutter SSL pinning using frida-flutterproxy

Repo: https://github.com/hackcatml/frida-flutterproxy
```bash
# change IP address 
frida -U -f com.example.dev -l ssl-bypass.js
```
- Bypass talsec / RASP & root detection

https://codeshare.frida.re/@muhammadhikmahhusnuzon/bypass-talsec-rasp-and-root-detection/
```bash
frida -U -f com.example.dev -l talsec-root.js
```
- Bypass Universal biometric bypass

```bash
frida -U -f com.example.dev -l universal-bio-bypass.js
```
---

*Use this cheatsheet as a starting point for mobile security testing.*

*https://github.com/floudeciel/chezmoi/tree/main/mobsec*
