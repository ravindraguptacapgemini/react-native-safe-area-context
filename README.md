# react-native-safe-area-context

[![npm](https://img.shields.io/npm/v/react-native-safe-area-context)](https://www.npmjs.com/package/react-native-safe-area-context) [![CI](https://github.com/th3rdwave/react-native-safe-area-context/workflows/CI/badge.svg)](https://github.com/th3rdwave/react-native-safe-area-context/actions?query=workflow%3ACI) ![Supports Android, iOS and web](https://img.shields.io/badge/platforms-android%20%7C%20ios%20%7C%20web-lightgrey.svg) ![MIT License](https://img.shields.io/npm/l/react-native-safe-area-context.svg)

A flexible way to handle safe area, also works on Android and Web!

## Getting started

Install the library using either Yarn:

```
yarn add react-native-safe-area-context
```

or npm:

```
npm install --save react-native-safe-area-context
```

You then need to link the native parts of the library for the platforms you are using. The easiest way to link the library is using the CLI tool by running this command from the root of your project:

```
react-native link react-native-safe-area-context
```

If you can't or don't want to use the CLI tool, you can also manually link the library using the instructions below (click on the arrow to show them):

<details>
<summary>Manually link the library on iOS</summary>

Either follow the [instructions in the React Native documentation](https://facebook.github.io/react-native/docs/linking-libraries-ios#manual-linking) to manually link the framework or link using [Cocoapods](https://cocoapods.org) by adding this to your `Podfile`:

```ruby
pod 'react-native-safe-area-context', :path => '../node_modules/react-native-safe-area-context'
```

</details>

<details>
<summary>Manually link the library on Android</summary>

Make the following changes:

#### `android/settings.gradle`

```groovy
include ':react-native-safe-area-context'
project(':react-native-safe-area-context').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-safe-area-context/android')
```

#### `android/app/build.gradle`

```groovy
dependencies {
   ...
   implementation project(':react-native-safe-area-context')
}
```

#### `android/app/src/main/.../MainApplication.java`

On top, where imports are:

```java
import com.th3rdwave.safeareacontext.SafeAreaContextPackage;
```

Add the `SafeAreaContextPackage` class to your list of exported packages.

```java
@Override
protected List<ReactPackage> getPackages() {
    return Arrays.asList(
            new MainReactPackage(),
            ...
            new SafeAreaContextPackage()
    );
}
```

</details>

## Usage

Add `SafeAreaProvider` in your app root component:

```js
import { SafeAreaProvider } from 'react-native-safe-area-context';

function App() {
  return <SafeAreaProvider>...</SafeAreaProvider>;
}
```

Usage with hooks api:

```js
import { useSafeAreaInsets } from 'react-native-safe-area-context';

function HookComponent() {
  const insets = useSafeAreaInsets();

  return <View style={{ paddingTop: insets.top }} />;
}
```

Usage with consumer api:

```js
import { SafeAreaInsetsContext } from 'react-native-safe-area-context';

class ClassComponent extends React.Component {
  render() {
    return (
      <SafeAreaInsetsContext.Consumer>
        {insets => <View style={{ paddingTop: insets.top }} />}
      </SafeAreaInsetsContext.Consumer>
    );
  }
}
```

Usage with `SafeAreaView`:

```js
import { SafeAreaView } from 'react-native-safe-area-context';

function SomeComponent() {
  return (
    <SafeAreaView>
      <View />
    </SafeAreaView>
  );
}
```

### Web SSR

If you are doing server side rendering on the web you can use `initialMetrics` to inject insets and frame value based on the device the user has, or simply pass zero values. Since insets measurement is async it will break rendering your page content otherwise.

### Optimization

To speed up the initial render, you can import `initialWindowMetrics` from this package and set as the `initialMetrics` prop on the provider as described in Web SSR. You cannot do this if your provider remounts, or you are using `react-native-navigation`.

```js
import {
  SafeAreaProvider,
  initialWindowMetrics
} from 'react-native-safe-area-context';

function App() {
  return (
    <SafeAreaProvider initialMetrics={initialWindowMetrics}>
      ...
    </SafeAreaProvider>
  );
}
```

## Resources

- Great article about how this library can be used: https://dev.to/brunolemos/adding-notch-support-to-your-react-native-android-app-3ci3
