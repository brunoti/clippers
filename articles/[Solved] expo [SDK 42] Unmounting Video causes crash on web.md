# [Solved] expo [SDK 42] Unmounting <Video /> causes crash on web
### Summary

As per the title, unmounting a `Video` component crashes the app and the following error message is shown:

`TypeError: Cannot use 'in' operator to search for 'requestFullscreen' in null`

Downgrading `expo-av` from 9.2.3 to 9.1.2 fixes the crash.

### Managed or bare workflow? If you have `ios/` or `android/` directories in your project, the answer is bare!

managed

### What platform(s) does this occur on?

Web

### SDK Version (managed workflow only)

42

### Environment

      Expo CLI 4.7.2 environment info:
        System:
          OS: macOS 11.4
          Shell: 5.8 \- /bin/zsh
        Binaries:
          Node: 14.4.0 \- /usr/local/bin/node
          Yarn: 1.22.4 \- /usr/local/bin/yarn
          npm: 6.14.4 \- /usr/local/bin/npm
          Watchman: 4.9.0 \- /usr/local/bin/watchman
        Managers:
          CocoaPods: 1.10.1 \- /usr/local/bin/pod
        SDKs:
          iOS SDK:
            Platforms: iOS 14.5, DriverKit 20.4, macOS 11.3, tvOS 14.5, watchOS 7.4
          Android SDK:
            API Levels: 28, 29, 30
            Build Tools: 28.0.3, 30.0.3
            System Images: android\-30 | Google APIs Intel x86 Atom
        IDEs:
          Android Studio: 4.2 AI\-202.7660.26.42.7351085
          Xcode: 12.5/12E262 \- /usr/bin/xcodebuild
        npmPackages:
          expo: ~42.0.0 \=> 42.0.0 
          react: 16.13.1 \=> 16.13.1 
          react\-dom: 16.13.1 \=> 16.13.1 
          react\-native: https://github.com/expo/react-native/archive/sdk-42.0.0.tar.gz => 0.63.2 
          react\-native\-web: ~0.13.12 \=> 0.13.18 
        npmGlobalPackages:
          expo\-cli: 3.21.12
        Expo Workflow: managed

### Reproducible demo or steps to reproduce from a blank project

[https://snack.expo.io/@junminlee/github.com-junminleeks-expo42web-video](https://snack.expo.io/@junminlee/github.com-junminleeks-expo42web-video)  
Use the `SHOW` button to mount the video and then use the `HIDE` button to unmount the video. The crash will occur while trying to unmount the video.

> Downgrading `expo-av` from 9.2.3 to 9.1.2 fixes the crash.

That fixes web, but with 9.1.2, I'm getting the following error when I run pod install (bare workflow):

    \[!\] Unable to find a specification for \`UMFileSystemInterface\` depended upon by \`EXAV\`

    You have either:
     \* out\-of\-date source repos which you can update with \`pod repo update\` or with \`pod install --repo-update\`.
     \* mistyped the name or version.
     \* not added the source repo that hosts the Podspec to your Podfile.

Just upgraded from Expo 39 to 42 and am now seeing this on `expo start --web` too.

Also seeing this after upgrading from Expo v41 to v42

## Related Issues:

[expo 2FA / MFA](https://lifesaver.codes/answer/2fa-mfa-160)

Hi everyone! Thanks to lots of hard work from @fxfactorial we have preliminary support for Apple 2FA...

[expo Google sign in Error](https://lifesaver.codes/answer/google-sign-in-error-3540)

Even though it looks like an issue with Expo I've found a temporary solution: Solution Keep using Ex... 
 [https://lifesaver.codes/answer/sdk-42-unmounting-video-causes-crash-on-web-13553](https://lifesaver.codes/answer/sdk-42-unmounting-video-causes-crash-on-web-13553)
