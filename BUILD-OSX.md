# Building on Mac OS X (10.11)

This is walkthrough for setting up a build environment, getting the source, and actually compiling
the xdAuto ROM for the NU/NR series of Carpad headunits. The steps here are based on one
developer's experience. YMMV

My environment as of starting this process:

* Mac OS X: 10.11.5
* Xcode: 7.3.1
* Java: JDK 1.8

## Prerequisites

First take a look at https://source.android.com/source/requirements.html#operating-system
and make sure you have all the required software (keep in mind that this platform is building 4.4
(KitKat):

> Android 4.1.x-4.3.x (Jelly Bean) - Android 4.4.x (KitKat): Mac OS v10.6 (Snow Leopard) or Mac OS X
v10.7 (Lion) and Xcode 4.2 (Apple's Developer Tools)

> Android 2.3.x (Gingerbread) - Android 4.4.x (KitKat): Ubuntu - Java JDK 6, Mac OS - Java JDK 6

What we _can_ "ignore":

1. Mac OS X 10.6 or 10.7 -- I've tested this with 10.11, and it does work, with some tweaks.
2. Xcode 4.2 -- You will not be able to install (and use) such an old version of Xcode on 10.11,
    but the important part is you will be able to have the app package itself, and use
    `xcode-select` to switch to it. I've tested this with Xcode 5.0.2, and it does appear to work.

What we _cannot_ ignore:

1. Xcode must be _older_ than 5.1.x. So Google suggests 4.2, and I've had some success with 5.0.2.
    To download the proper version of Xcode, first sign into Apple's developer portal with a valid
    Apple account: https://developer.apple.com/download/more/

    From there, search for "5.0" and it should list two versions: 5.0.1 and 5.0.2. I used 5.0.2.

    After it downloads, you **will not** be able to run that version of Xcode directly, because it
    does not support modern versions of OS X. However, if you copy the Xcode.app package to a new
    name, you will be able to keep your current (probably modern) version of Xcode, alongside the
    now ancient 5.0.x version.  Just copy the app to something like /Applications/Xcode-5.0.2.app.
    If you're not sure how to do that, the easiest way is to **ignore** the dmg instructions
    (which will tell you to simple drag from Xcode.app -> Applications), and instead drag Xcode.app
    to your desktop. Once it's copied over, you can rename it to "Xcode-5.0.2.app", and then you
    can drag that renamed version into the Applications folder.
2. JDK 6 is required -- it will not build with JDK 7 or JDK 8. To make matters worse, it's somewhat
    difficult to actually get the proper JDK 6 package for modern OS X versions. At time of
    writing, the JDK 6 can be downloaded from http://support.apple.com/downloads/DL1572/en_US/javaforosx.dmg

## Preparing a build environment

Next, read over http://source.android.com/source/initializing.html#setting-up-a-mac-os-x-build-environment
to make sure that your system is set up for building. One important note from this document that
you really do need to follow is creating the `android.dmg` virtual hard drive, in order to support
a case-sensitive filesystem. Downgrading gmake was not necessary for me, and I did not enable
ccache. It would probably work just as well if you follow Google's instructions, so enable it if
you want to.

## Downloading the source

The next step is [Downloading the Source](http://source.android.com/source/downloading.html).
You do need to install the `repo` tool, but you **should not** run the `repo init` command using
the URL mentioned on that page. Instead, you need to use the xdAuto repo manifest, as mentioned
in [platform_manifest/README](https://github.com/Nu3001/platform_manifest/blob/master/README.md):

```
$ mkdir /Volumes/android/xdauto
$ cd /Volumes/android/xdauto
$ repo init -u https://github.com/Nu3001/platform_manifest.git -b master
$ repo sync
```
This assumes that your `android.img` virtual disk is mounted at `/Volumes/android`, and that you
want the source code to live `/Volumes/android/xdauto`.

Sit back and relax, go get a coffee, or something like that, because this will take a _long_ time
for most people to download (it's approximately **23 GB**)

## Preparing to build

As mentioned above, you do need to have Xcode 5.0.x or earlier, and JDK 6. If you have already
installed those components, now you also need to switch to them so that the build environment can
build using the proper versions:

1. Switch to JDK 6 -- This is simple, as it's just an environment variable:

    ```
    $ export JAVA_HOME=$(/usr/libexec/java_home -v 1.6)
    ```
2. Switch to Xcode 5.0.2 -- If you followed the instructions above, you should have 5.0.2 now
    "installed" at `/Applications/Xcode-5.0.2.app/`. So now you need to indicate that you want to
    use that version:

    ```
    $ sudo xcode-select --switch /Applications/Xcode-5.0.2.app/Contents/Developer
    ```

## Building

After that, you should (hopefully) have everything you need to start building, and now you can
once again resume building by following the [platform_manifest/README](https://github.com/Nu3001/platform_manifest/blob/master/README.md)
document, starting first with building the kernel, and then building android, and so on.
