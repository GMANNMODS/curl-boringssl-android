# openssl-curl-android

Compile openssl and curl for Android

## Prerequisites

Make sure you have `Android NDK` installed.

And also necessary `autoconf` and `libtool` toolchains.

## Download

If you do not want to compile them yourself, you can download pre-compiled static libraries from [releases](https://github.com/Zackptg5/openssl-curl-android/releases). They are in `build.tar.gz`.

Doing your own compilation is recommended, since the pre-compiled binary can become outdated soon.

Update git submodules to compile newer versions of the libraries.

## Usage

```bash
git clone https://github.com/Zackptg5/openssl-curl-android.git
git submodule update --init --recursive

NDK=android_ndk_version_you_want_to_use
export HOST_TAG=see_this_table_for_info # https://developer.android.com/ndk/guides/other_build_systems#overview
export MIN_SDK_VERSION=21 # or any version you want

chmod +x ./build.sh
./build.sh
```

All compiled libs are located in `build/openssl` and `build/curl` directory.

Use NDK to link those libs, part of `Android.mk` example:

```makefile
include $(CLEAR_VARS)
LOCAL_MODULE := curl
LOCAL_SRC_FILES := build/curl/$(TARGET_ARCH_ABI)/libcurl.a
include $(PREBUILT_STATIC_LIBRARY)
```

## Options

Change scripts' configure arguments to meet your requirements.

For now, using tls (https) in Android would throw `peer verification failed`.

Please explicitly set `curl_easy_setopt(curl, CURLOPT_CAINFO, CA_BUNDLE_PATH);` where `CA_BUNDLE_PATH` is your ca-bundle in the devide storage.

You can download and copy [cacert.pem](https://curl.haxx.se/docs/caextract.html) to the internal storage to get tls working for libcurl.

## Working Example

Checkout this [repo](https://github.com/robertying/CampusNet-Android/blob/master/app/src/main/cpp/jni) to see how to integrate compiled static libraries into an existing Android project, including `Android.mk` setup and `JNI` configurations.

## Credits of Originality:

This is a fork of the original here: https://github.com/robertying/openssl-curl-android
