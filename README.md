# Unreal Engine 5.0 | Oculus Quest 2 | MacOS

This repository contains a barebones starter template for an Unreal Engine 5.0 VR project. This branch is specifically targeted and tested to run on Oculus Quest 2 via an Unreal Engine 5 build on MacOS.

## Goal

The goal of this repository is to get a VR project successfully built and running on your head mounted display. It should provide a known working starting point for new VR projects. It is not focused on providing game logic or starter content.

The configuration documented below will help you launch Unreal Engine's supplied _VRTemplate_ project, but we felt that project contained too many features, shaders, plugins, variables, etc. that could create hard-to-trace problems in later development and build stages.

Our template is geared with a "bring your own" mentality aimed primarily at reducing the complexity of the build process.

## System Recommendations

These are the system settings and packages that we know to successfully compile and launch this template. As a disclaimer, there may be some flexibility in the versions and packages you choose, but the ones listed below provided the most consistent results for us. There are notes below that detail definite conflicts and problems areas when known as well as tips for installing.

- Unreal Engine
  - Version: `5.0.2`
- Android Studio
  - Version: `4.0`, `4.2`
  - SDK Platform: Android `10.0 (Q)`
    - Android SDK Platform `29`
    - Sources for Android `29`
  - SDK Tools:
    - Android SDK Build-Tools: `29.0.2`
    - NDK (Side by side): `21.1.6352462`
    - Android SDK Command-line Tools: `7.0 (latest)`
    - CMake: `3.18.1`
    - Android SDK Platform-Tools: `33.0.2`
    - Intel x86 Emulator Accelerator (HAXM installer): `7.6.5`
- Java
  - openjdk version: `1.8.0_242-release`
  - OpenJDK Runtime Environment (build 1.8.0_242-release-1644-b3-6222593)
  - OpenJDK 64-Bit Server VM (build 25.242-b3-6222593, mixed mode)
- MacOS
  - Version: `12.4 (Monterey)`
  - Processor:
    - Apple Chip M1 Max
    - Intel i5
- Oculus Developer Hub
  - Version: `2.5.0`

### Notes

## Installation and Configuration

### Android Studio

#### Install

From the download archives at [https://developer.android.com/studio/archive](https://developer.android.com/studio/archive), download and install [Android Studio 4.0](https://redirector.gvt1.com/edgedl/android/studio/install/4.0.0.16/android-studio-ide-193.6514223-mac.dmg)

It's often recommended to have Unreal Engine and Epic Games Launcher closed while installing and configuring Android Studio

Version 4.0 is the one we've seen most often recommended, but we have also had success with version 4.2.2 and suspect any of the 4.x.x versions would be compatible. Our configuration seems to work on both fresh and existing installs.

The latest version of Android Studio (Chipmunk 2021.2.1, at this writing) will not work. From our tests, it seems to expect different requirements for the gradle project than the ones that Unreal Engine 5.0 automatically generates during the build process. We did not experiment with changing this default gradle setup.

#### Configure

The 4.x versions include an SDK Manager that will handle installing the minimum required packages. We recommend using it as opposed to installing separately from other sources.

From the "Welcome to Android Studio" splash screen, select _Configure > SDK Manager_ (or from within an Android project, select _Tools > SDK Manager_).

Under _Appearance & Behavior > System Settings > Android SDK_, select the _SDK Platforms_ tab. Check the box for "Show Package Details," then find "Android 10.0 (Q)."

- Check the box for **Android SDK Platform 29**
- Check the box for **Sources for Android 29.**

> Note: We have preferred to uncheck all other packages in order to keep our installation light, but having them does not appear to cause conflict with the Unreal Engine build process.

Next, select the _SDK Tools_ tab and check "Show Package Details."

- Under "Android SDK Build-Tools 33," check the box for **29.0.2**.
- Under "NDK (Side by side)," check the box for **21.1.6352464**.
- Check the box for **Android SDK Command-line Tools (latest)**, it should be version **7.0**
- Under "CMake," check the box for **3.18.1**
- Check the box for **Android SDK Platform-Tools** (version 33.0.2)

It may be necessary to add the Intel HAXM tool, but we're not sure how necessary it is. It may depend on your system's processer (i.e. Apple chip vs. Intel chip). We know it is not necessary for M1 Apple chip Macs, but also doesn't hurt.

- Check the box for **Intel x86 Emulator Accelerator (HAXM installer)** (version 7.6.5)

Click "Apply" and follow any prompts to continue with the install/uninstall of the packages.

#### Tips

You should not ever need to run the full Android Studio during the build process. It is really only used to manage related tools. In fact, you do not even need to open the full IDE. You can configure everything from the "welcome" splash screen.

Save some time by setting Android Studio to prefer the splash screen by unchecking the box for **Reopen last project on startup** in _Preferences > Appearance and Behavior > System Settings_, under the "Startup/Shutdown" section.

You can get a list of the installed packages and their locations in terminal with the `sdkmanager` command line tool.

```bash
$ cd ~/Library/Android/sdk/cmdline-tools/latest/bin/
$ ./sdkmanager --list_installed
```

### Java

#### Install

Android Studio 4.0 comes with a pre-installed Java version `1.8` ([or 8](https://www.oracle.com/java/technologies/javase/jdk8-naming.html)). We use this install for our Unreal Engine builds. It is located in the Application directory, such as _/Applications/Android Studio.app/Contents/jre/jdk/Contents/Home_.

> Beware: the white-space character in the app name can be an issue for Unreal Engine settings

Unreal Engine 5 will automatically install `gradle v6.1.1` during the build/packaging process. This seems most compatible with `java 1.8`, but we have also seen `java 11.x` work. Proceed at you own risk.

#### Configure

You should ensure that the `JAVA_HOME` environment variable is set in you shell, and that it matches the same version and path as the one you reference in any Unreal Engine Project Settings.

We use `jenv` to handle this, see Tips section below.

#### Tips

##### Environment Management

We use `jenv` to reduce the headaches involved with Java versioning and ensuring the `JAVA_HOME` environment variable is set and available when Unreal Engine needs it.

Refer to the `jenv` [website](https://www.jenv.be/) and Github [documentation](https://github.com/jenv/jenv) for details about installing and configuring. We did it through [`homebrew`](https://brew.sh/) and ensured the shell was configured to `export` the `JAVA_HOME` variable.

##### Information

To get information about your current Java install, here are some helpful terminal commands:

```bash
# see the current global version
$ java -version

# if using jenv, find the install path
readlink -f `jenv javahome`
# or for more verbose output
ls -l `jenv javahome`

# or if not using jenv
which java
```

### Gradle

Gradle is the command line tool used under the hood to package the actual Android APK file for your VR application. Unreal Engine 5 automatically installs gradle locally to the project during the build process.

This means you don't need to install this separately, but we wanted to make a note of it since it is a critical part of the build process and may be the source of errors.

#### Tips

You can find build information and perform useful functions by locating the install path and running `gradlew` tasks. Note the `w` at the end of the command name! Also know that this directory and install will not exist until you attempt your first build.

```bash
$ cd $YOUR_PROJECT_DIRECTORY/Intermediate/Android/arm64/gradle

# to list version information
$ ./gradlew --version

# to get a list of other tasks you can run (we only do these if debugging an issue)
$ ./gradlew tasks
```

If your build runs into errors that seem related to Gradle or APK packaging, we have found it helpful to sometimes manually clear the build directory and caches with a `gradlew` task. For instance:

```bash
$ cd $YOUR_PROJECT_DIRECTORY/Intermediate/Android/arm64/gradle
$ ./gradlew clean
$ ./gradlew cleanBuildCache
```

### Unreal Engine

#### Install

We are using the latest stable release of Unreal Engine `5.0.2`, as installed through the Epic Games Launcher.

#### Configure

##### Config INI Files

The _Config/_ directory in this repository should contain everything you need to build the project for an Oculus Quest 2 with regard to basic VR rendering. We've made efforts to reduce or remove settings that do not specifically target or address VR environments.

##### Android SDK

There are some additional things you should set in the Editor UI to ensure success. Open up the _Project Settings_ window.

Navigate to _Platforms > Android SDK_ and configure all the fields here. Unreal Engine's tooltips for these fields suggest that they will default to your system's environment variables if not set, but we have noticed less issues when explicity setting them. Below is an example:

| SDKConfig               |                                                                                       |
| ----------------------- | ------------------------------------------------------------------------------------- |
| Location of Android SDK | `~/Library/Android/sdk`                                                               |
| Location of Android NDK | `~/Library/Android/sdk/ndk/21.1.6352462`                                              |
| Location of JAVA        | `/Applications/Android Studio.app/Contents/jre/jdk/Contents/Home`                     |
|                         | or `~/.jenv/versions/1.8` (if you set up `jenv` to point to the Android install path) |
| SDK API Level           | `matchndk`                                                                            |
| NDK API Level           | `latest`                                                                              |

> Note: the example uses `~` to indicate your user's home path, but you should probably replace this with an absolute path like `/Users/myname`

##### APK Packaging

Open up the _Project Settings_ window.

Under _Platforms > Android > APK Packaging_, change the Android Package name to use your own company or personal ID.

Then scroll down to the bottom of the _APK Packaging_ section and ensure that the "Platform files are writable" by clicking the "Configure Now" button (if applicable). Be sure to click "Accept SDK License" as well.

> Note: You should "Configure Now" only after you've applied the Android SDK settings documented above.

#### Tips

##### Config Merging

We have been able to turn unsuccessful project builds into succesful ones by merging the _Config/\*.ini_ files from this repository into the unsuccessful project's repository. This is an advanced option we would only recommend for users who are comfortable working with a text editor and source control as you will have to make some manual decisions about some settings.

> Our _Config/\*.ini_ files have had their settings alphabetically sorted and we'd recommend do the same to yours. This greatly reduces the complexity of diffing a merge!

##### Other Config Locations

Unreal Engine saves project settings in various places throughout the filesystem. Only one of them is generally checked into source control, as others contain generated and per-system options that are otherwise "hidden" from the end-user. These files contain a lot of useful information for comparing what works and what doesn’t for a successful VR build.

If you run into problems with your build and suspect project settings are to blame, here are some places to look:

```bash
# usually checked into source control
$YOUR_PROJECT_DIRECTORY/Config/**

# generated by Unreal, contains things like per-system SDKPath variable
$YOUR_PROJECT_DIRECTORY/Intermediate/Config/CoalescedSourceConfigs/**

# per system settings, may be stored elsewhere depending on system
# note the name of the Editor path includes you project's name, like SuperBlankEditor
~/Library/Preferences/Unreal\ Engine/$YOUR_PROJECT_NAMEEditor/**
```
