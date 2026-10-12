# A link that relaunches a reclaimed Android app never reaches JavaScript

React Native 0.87.1, from the [React Native reproducer template](https://github.com/react-native-community/reproducer-react-native). `App.tsx` shows `Linking.getInitialURL()` and the last `url` event, and the template's `singleTask` `MainActivity` takes `reproducer://` links.

## Steps

1. Build and install the release APK: `cd ReproducerApp/android && ./gradlew assembleRelease && adb install -r app/build/outputs/apk/release/app-release.apk`.
2. Open the first link: `adb shell am start -a android.intent.action.VIEW -d reproducer://first com.reproducerapp`.
3. Background the app and let Android end its process while the task stays in Recents: `adb shell input keyevent KEYCODE_HOME`, `adb shell am start -a android.settings.SETTINGS`, `adb shell input keyevent KEYCODE_HOME`, then `adb shell am kill com.reproducerapp` until `adb shell pidof com.reproducerapp` prints nothing.
4. Open the second link: `adb shell am start -a android.intent.action.VIEW -d reproducer://second com.reproducerapp`.

On `main` the screen shows `getInitialURL(): reproducer://first` and `last url event: null`. The `verify/set-intent` branch adds `setIntent(intent)` to `MainActivity.onNewIntent`, and the screen shows `getInitialURL(): reproducer://second`.
