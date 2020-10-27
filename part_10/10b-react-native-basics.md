# 10b. React Native Basics

[Back to index](../README.md)

## Core Components

- A set of components provided by React Native
  - Behind the scenes, they utilize the platform's native components
- Some core components:
  - `Text`
    - Similar to for example `<strong>`, `<h1>`
    - The only React Native component that can have textual children
  - `View`: similar to `<div>`
  - `TextInput`: similar to `<input>`
  - `TouchableWithoutFeedback` (and other `Touchable*` components)
    - Similar to for example `<button>`
    - For capturing different press events

Note: for event handlers, read the [API documentation](https://reactnative.dev/docs/components-and-apis)

## Styles

Example:

```js
import { StyleSheet } from "react-native";

const styles = StyleSheet.create({
  container: {
    padding: 20,
  },
  text: {
    color: "blue",
    fontSize: 24,
  },
});
```

### Conditional Styles

Example:

```js
import { Text } from "react-native";

const MyText = ({ isBlue, children }) => {
  const textStyles = [styles.text, isBlue && styles.blueText];

  return <Text style={textStyles}>{children}</Text>;
};
```

### Flexbox

Consisting two components:

- A flex container
- A set of flex items inside

Example:

```js
import React from "react";
import { View, StyleSheet } from "react-native";

const styles = StyleSheet.create({
  flexContainer: {
    flexDirection: "row",
  },
});

const FlexboxExample = () => {
  return <View style={styles.flexContainer}>{/* ... */}</View>;
};
```

### Important Properties of Flex Container

- `flexDirection`
  - Defines the main axis
  - Values (default: `column`)
    - `row`: from left to right
    - `row-reverse`
    - `column`: from top to bottom
    - `column-reverse`
- `justifyContent`
  - Controls the alignment of flex items along the main axis
  - Default value: `flex-start`
- `alignItems`
  - Controls the alignment of flex items along the cross axis
  - Default value: `stretch`
