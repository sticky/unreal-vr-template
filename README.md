# Unreal Engine 5.0 | Oculus Quest 2 | MacOS

This repository contains a barebones starter template for an Unreal Engine 5.0 VR project. This branch is specifically targeted and tested to package a working development-mode APK for Oculus Quest 2 via Unreal Engine 5.0 on MacOS.

## Goal

The goal of this repository is to get a VR project successfully packaged and running on your head mounted display. It should provide a known working starting point for new VR projects. It is not focused on providing game logic or starter content.

The steps documentated below will help you launch Unreal Engine's supplied _Virtual Reality_ project, but we felt that project contained too many features, shaders, plugins, variables, and unknowns that could create hard-to-trace problems in later stages of development and packaging.

Our template is geared with a "bring your own" mentality aimed primarily at reducing the complexity and mystery of building and packaging for devices. We hope it provides a unified source of knowledge for getting started in Unreal / VR development.

## What to Expect

![A preview of the app depicting blue floor, sphere and Oculus Quest 2 controllers](./docs/img/preview.jpg)

After successfully building and packaging the project, you will be able to put on your Oculus Quest 2 headset and see a starkly lit blue floor. There is a sphere, cube and cone in three of the four corners. You will be able to look around and see your hand controllers.

There is no visual feedback for button clicking and the only functionality is the "snap turn" motion on the left joystick.

> Note: The Oculus Quest 1 headset will also work, but we have hardcoded a reference to the Quest 2 hand controller meshes as provided by the OpenXR plugin. You can change this in the `VRPawn` Blueprint.

We used the `VRPawn` Blueprint from Unreal Engine's _Virtual Reality_ template as a starting point for our template, but we have cleared out its original functionality, leaving only the basic nodes for responding to input. Bring your own.

## System Recommendations

These are the system settings and packages that we know to successfully compile and launch this template. As a disclaimer, there may be some flexibility in the versions and packages you choose, but the ones listed below provided the most consistent results for us.

See the [Setting Up Your Environment](#setting-up-your-environment) section of this document for tips on preparing your environment and notes on potential issues you may experience in the process.

- Unreal Engine
  - Version: `5.0.2`
- Android Studio
  - Version: `4.0`
  - SDK Platform: Android `10.0 (Q)`
    - Android SDK Platform `29`
    - Sources for Android `29`
  - SDK Tools:
    - Android SDK Build-Tools: `29.0.2`
    - NDK (Side by side): `21.1.6352462`
    - Android SDK Command-line Tools: `7.0 (latest)`
    - CMake: `3.18.1`
    - Android SDK Platform-Tools: `33.0.2`
- Java
  - openjdk version: `1.8.0_242-release`
- MacOS
  - Version: `12.4 (Monterey)`
  - Processor:
    - Apple Chip M1 Max
    - Intel i5
- Oculus Developer Hub
  - Version: `2.5.0`

## Setting Up Your Environment

### Android Studio

Oculus headsets are Android devices under the hood. When creating an Oculus-targeted VR project in Unreal Engine, your final output will be an Android app. Android Studio provides a complete set of tools for developing Android apps and so it is the obvious choice for packaging a VR application from Unreal.

#### Install

From the download archives at [https://developer.android.com/studio/archive](https://developer.android.com/studio/archive), download and install [Android Studio `4.0`](https://redirector.gvt1.com/edgedl/android/studio/install/4.0.0.16/android-studio-ide-193.6514223-mac.dmg)

> Note: We've often seen it recommended to have Unreal Engine and Epic Games Launcher closed while installing and configuring Android Studio. We suspect this has something to do with Unreal being able to source the newly configured environment. At the very least, you should restart your Editor after installing everything.

Android Studio `4.0` is the one we've seen most often recommended, but we have also had success with version `4.2.2` and suspect any of the `4.x.x` versions would be compatible. Our configuration seems to work on both fresh and existing installs. It doesn't really matter if you choose a "standard" or "custom" installation as you will have to configure additional components anyway.

> Beware: The latest version of Android Studio (`Chipmunk 2021.2.1`, at this writing) will _not_ work. From our tests, it seems to expect different requirements for the Gradle project than the ones that Unreal Engine 5.0 automatically generates during the build process. We did not experiment with changing this default Gradle setup. See the [Troubleshooting > Gradle](#gradle) for more.

#### Configure

The `4.x.x` versions include an SDK Manager that will handle installing the minimum required packages. We recommend using it as opposed to installing separately from other sources.

From the "Welcome to Android Studio" splash screen, select _Configure > SDK Manager_ (or from within an Android project, select _Tools > SDK Manager_).

Under _Appearance & Behavior > System Settings > Android SDK_, select the _SDK Platforms_ tab. Check the box for "Show Package Details," then find "Android 10.0 (Q)" and check the following:

- [x] **Android SDK Platform 29**
- [x] **Sources for Android 29.**

Next, select the _SDK Tools_ tab and check "Show Package Details."

- [x] Android SDK Build-Tools 33
  - [x] **29.0.2**
- [x] NDK (Side by side)
  - [x] **21.1.6352464**
- [x] Android SDK Command-line Tools (latest)
  - [x] **Android SDK Command-line Tools (latest)** (version **7.0**)
- [x] CMake
  - [x] **3.18.1**
- [x] **Android SDK Platform-Tools** (version 33.0.2)

Click "Apply" and follow any prompts to continue with the install/uninstall of the packages.

> Note: We prefer to uncheck all other packages in our SDK Mananger settings. It keeps the installation light and helps prevent possible overwrites/collisions in Unreal Engine. See more about this later in [Unreal Engine > Configure > Android SDK](#android-sdk)

#### Tips

##### Quick Launch

You should not ever need to run the full Android Studio during the build process. It is really only used to manage related tools. In fact, you do not even need to open the full IDE. You can configure everything from the "welcome" splash screen.

Save some time by setting Android Studio to prefer the splash screen instead of the last open project from _Preferences > Appearance and Behavior > System Settings_, under the "Startup/Shutdown" section, uncheck:

- [ ] **Reopen last project on startup**

##### CLI

You can get a list of the installed packages and their locations in terminal with the `sdkmanager` command line tool.

```bash
$ cd ~/Library/Android/sdk/cmdline-tools/latest/bin/
$ ./sdkmanager --list_installed
```

### Java

#### Install

Android Studio `4.0` comes with a pre-installed Java version `1.8` ([or 8](https://www.oracle.com/java/technologies/javase/jdk8-naming.html)). We use this install for our Unreal Engine builds. It is located wherever your app is installed, such as _/Applications/Android Studio.app/Contents/jre/jdk/Contents/Home_.

> Beware: the white-space character in the app name can be an issue for Unreal Engine settings and terminal commands

Unreal Engine `5.0` will automatically install Gradle `6.1.1` in order to handle the packaging process. This seems most compatible with Java `1.8`, but we have also seen Java `11.x` work. Proceed at you own risk.

#### Configure

You should ensure that the `JAVA_HOME` environment variable is set in your shell and that it matches the same version and path as the one you reference in any Unreal Engine Project Settings.

We use `jenv` to handle this, see [Tips > Environment Management](#environment-management) section below.

#### Tips

##### Environment Management

We use `jenv` to reduce the headaches involved with Java versioning and ensuring the `JAVA_HOME` environment variable is set and available when Unreal Engine needs it.

Refer to the `jenv` [website](https://www.jenv.be/) and Github [documentation](https://github.com/jenv/jenv) for details about installing and configuring. We did it through [`homebrew`](https://brew.sh/) and ensured the shell was configured to `export` the `JAVA_HOME` variable.

##### Information

To get information about your current Java install, here are some helpful terminal commands:

```bash
# see the current global version
$ java -version

# to see if the JAVA_HOME variable is set
$ echo $JAVA_HOME

# if using jenv, find the install path
readlink -f `jenv javahome`
# or for more verbose output
ls -l `jenv javahome`

# or if not using jenv
which java
```

### Unreal Engine

#### Install

We are using the latest stable release of Unreal Engine `5.0.2`, as installed through the Epic Games Launcher.

#### Configure

##### Config INI Files

The _Config/_ directory in this repository should contain everything you need to build the project for an Oculus Quest 2 with regard to basic VR rendering. We've made efforts to remove settings that do not specifically address VR environments.

##### Android SDK

There are some additional things you should set in the Editor UI to ensure success. Open up the _Project Settings_ window.

Navigate to _Platforms > Android SDK_ and configure all the fields here. Unreal Engine's tooltips for these fields suggest that they will default to your system's environment variables if not set, but we have noticed less issues when explicity setting them. Below is an example:

| SDKConfig               |                                                                                       |
| ----------------------- | ------------------------------------------------------------------------------------- |
| Location of Android SDK | _~/Library/Android/sdk_                                                               |
| Location of Android NDK | _~/Library/Android/sdk/ndk/21.1.6352462_                                              |
| Location of JAVA        | _/Applications/Android Studio.app/Contents/jre/jdk/Contents/Home_                     |
|                         | or _~/.jenv/versions/1.8_ (if you set up `jenv` to point to the Android install path) |
| SDK API Level           | `matchndk` (or `android-29`, see notes)                                               |
| NDK API Level           | `latest` (or `android-21`, see notes)                                                 |

> Note: the example uses `~` to indicate your user's home path, but you should probably replace this with an absolute path like `/Users/myname`

We use `matchndk` and `latest` in our API level settings, but this assumes that there are no other tools installed in the Android Studio SDK Manager. If you need others installed simultaneously, you should reference specific versions such as `android-29` and `android-21`, but beware that Unreal Engine may attempt to install its preferred minor/patch version in this scenario.

##### APK Packaging

Open up the _Project Settings_ window.

Under _Platforms > Android > APK Packaging_, change the Android Package name to use your own company or personal ID.

Then scroll down to the bottom of the _APK Packaging_ section and ensure you see a big green banner with "Platform files are writable" by clicking the "Configure Now" button (if applicable). Be sure to click "Accept SDK License" as well.

> Note: You should "Configure Now" only after you've applied the Android SDK settings documented above.

##### Plugins

One of the most important breakthroughs we had while troubleshooting the default _Virtual Reality_ template was _removing_ the OculusVR Plugin! As of Unreal Engine 5.0, it is [officially recommended](https://discord.com/channels/187217643009212416/221799478364078081/934127474424578141) that you only need the OpenXR plugin. _This is already configured in our template_, but we mention it here because it is such a critical piece of the puzzle (re: not launching black screens in the headset).

To our knowledge, this change has something to do with the way that Oculus now interfaces with OpenXR to handle all of those original OculusVR plugin features. But all we really know is:

- [x] OpenXR :star2:
- [ ] Oculus VR :japanese_ogre:
- [ ] SteamVR :confused:

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

## Packaging the Application

After successfully [Setting Up Your Environment](#setting-up-your-environment), you are ready to package the application and run it on your device.

The first step of this process is to ensure that your Oculus device is configured for development and attached to your computer. You should follow [Meta's guidelines](https://developer.oculus.com/) regarding these steps. We use the [Oculus Developer Hub `2.5.0`](https://developer.oculus.com/documentation/unity/ts-odh/)

### Before Launching

Be sure to build the light and levels, otherwise you may see some warnings in the headset. Select _Build > Build All Levels_ from the Unreal Engine toolbar.

### From the Project Launcher Window

After confirming your device is ready and connected to your computer, you should see it in the list from the "Platforms" dropdown in the Unreal Engine editor. For the first run, we recommend packaging from the _Tools > Project Launcher_ window.

From the window, click the button for "Show Advanced." The find your Android Quest device in the list and configure the following dropdown settings:

| Setting    | Value          |
| ---------- | -------------- |
| Variant    | `Android_ASTC` |
| Config     | `Development`  |
| Data build | `By the book`  |

Once set, click the little screen/controller icon (Launch this profile) to begin the packaging process. If the project is successfully packaged, it will attempt to launch the app on your device. When you are finished, you can click "Cancel" or "Done" to quit the preview.

#### Notes

We have only had consistent results with the `Android_ASTC` variant.

At the time of this writing, we have not tested a `Shipping` config and expect it to involve an additional amount of work related to Android/Oculus app accounts and key-signing.

We start off with a `By the book` data build because even our slim template needs to cook some assets on the first run. If you are not changing any meshes, materials or lighting and are just testing the packaging process, you can probably switch to `Do not cook` to save yourself some time.

### From the Quick Launch Dropdown

If you've followed the steps in [From the Project Launcher Window](#from-the-project-launcher-window), you should be able to select you device from the _Platforms > Quick Launch_ dropdown and package/run the app with the same settings.

### From the Content/Device/SDK Management Dropdown

You can generate and save an APK file on your machine by selecting _Platforms > Content/Device/SDK Management > Android > Package Project_. Configure the settings as described in [From the Project Launcher Window](#from-the-project-launcher-window).

> Note: you will likely get an annoying popup when using this option that suggests "SDK Not Setup." As best we can tell, this is a bug in Unreal Engine for Mac because if you followed our [Setting Up Your Environment](#setting-up-your-environment) steps, the SDK is definitely setup. Hit "Continue" and proceed on your merry way.

You will prompted to specify a directory for the packaged files to be save on you machine. After the process is complete, you can manually drag and drop the APK file into any Oculus Developer Hub to install on a selected device.

## Troubleshooting

### Gradle

Gradle is a build tool used under the hood to package the actual Android APK file for your VR application. Unreal Engine `5.0` automatically installs Gradle locally to the project during the build process.

This means you don't need to install this separately, but we wanted to make a note of it since it is a critical part of the build process and may be the source of errors. We have not tested what happens if there is an existing version of Gradle already on the system.

#### Tips

You can find build information and perform useful functions by locating the install path and running `gradlew` tasks (note the `w` at the end of the command name). Also know that this directory and install will not exist until you attempt to create your first packaged app.

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

## Contributions

This is an open source project and document! Despite there being a vast and active Unreal Engine community publishing tutorials and answering forum questions, we found it extremely difficult to get a clear, unified answer on everything related to VR development. The topic itself is huge and involves many moving pieces.

We created this project to answer these questions for our own needs. Ideally, they will answer your questions as well. There is still a ton that we don't fully understand about the VR development and packaging process and welcome any and all expertise.

If you would like to contribute, please create an issue or search to see if one already exists in this repository. As a reminder, this project is focused on knowledge and guidance related to VR packaging. We are not interested in provided game content or logic.

Some things we would like to know include:

- [ ] Does this work on other non-Oculus devices?
- [ ] Do other versions of the Android SDK/APIs work?
- [ ] Are there things we do/don't need to set/install/configure?
- [ ] Are there better/more performant/more VR-focused Project Settings we can use?
- [ ] What needs to be configured for a mininum, working Windows environment?
  - We've done some testing in this area, but have been unable to produce consistent results
  - We would likely create a separate Windows-focused branch for this in our repository
- [ ] What should we know about the difference between `ASTC`, `ETC2` and `DXT`?
- [ ] What do need to do to create a `Shipping` package that's ready for app stores?
