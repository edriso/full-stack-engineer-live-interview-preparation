# React Native & Expo Basics

> A beginner-friendly guide for junior developers

---

## What is React Native?

React Native is a way to build **mobile apps** (iOS and Android) using **JavaScript** and **React**.

Think of it like this:
- **React** = Build websites
- **React Native** = Build mobile apps (using similar code!)

### Key Difference from Web React

| Web React | React Native |
|-----------|--------------|
| Uses HTML tags (`<div>`, `<span>`, `<p>`) | Uses special components (`<View>`, `<Text>`, `<Image>`) |
| Uses CSS files | Uses JavaScript objects for styles |
| Runs in browser | Runs on phone (iOS/Android) |

---

## What is Expo?

**Expo** is a tool that makes React Native easier. It handles the hard stuff for you.

### Two Ways to Use React Native:

1. **Expo (Managed Workflow)** ← What you used!
   - Easier to start
   - No need to install Xcode or Android Studio
   - Run app with QR code on your phone
   - Some limitations (can't use all native features)

2. **Bare Workflow**
   - More control
   - Harder to set up
   - Can use any native feature

**For your interview:** Know that Expo is great for quick development, but some apps need "bare workflow" for special features (like Bluetooth or background tasks).

---

## Core Components (The Building Blocks)

In React Native, you don't use HTML. You use these components:

### 1. `<View>` - Like a `<div>`

```tsx
import { View } from 'react-native';

// A container box
<View style={{ padding: 10 }}>
  {/* stuff goes here */}
</View>
```

### 2. `<Text>` - For any text

```tsx
import { Text } from 'react-native';

// You MUST wrap text in <Text>
<Text>Hello World</Text>

// ❌ WRONG - This will crash!
<View>Hello World</View>
```

**Important:** All text MUST be inside `<Text>`. This is different from web!

### 3. `<TextInput>` - Like `<input>`

```tsx
import { TextInput } from 'react-native';

<TextInput
  value={text}
  onChangeText={setText}  // Note: onChangeText, not onChange
  placeholder="Type here..."
/>
```

### 4. `<ScrollView>` - Scrollable container

```tsx
import { ScrollView } from 'react-native';

// Use when content might be longer than screen
<ScrollView>
  {/* lots of content */}
</ScrollView>
```

### 5. `<FlatList>` - For long lists (better performance)

```tsx
import { FlatList } from 'react-native';

<FlatList
  data={myArray}
  keyExtractor={(item) => item.id}
  renderItem={({ item }) => <Text>{item.name}</Text>}
/>
```

**When to use which?**
- `ScrollView` = Short lists (under 20 items)
- `FlatList` = Long lists (it only renders what's visible = faster!)

### 6. `<Pressable>` - Touchable button

```tsx
import { Pressable } from 'react-native';

<Pressable onPress={() => console.log('Pressed!')}>
  <Text>Click Me</Text>
</Pressable>
```

### 7. `<Image>` - For images

```tsx
import { Image } from 'react-native';

// Local image
<Image source={require('./cat.png')} />

// Remote image (needs width/height!)
<Image 
  source={{ uri: 'https://example.com/cat.png' }}
  style={{ width: 100, height: 100 }}
/>
```

---

## Styling in React Native

### No CSS Files!

In React Native, styles are JavaScript objects:

```tsx
import { StyleSheet, View, Text } from 'react-native';

function MyComponent() {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Hello</Text>
    </View>
  );
}

// Define styles at bottom of file
const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#fff',
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#333',
  },
});
```

### Key Style Differences from CSS

| CSS | React Native |
|-----|--------------|
| `background-color` | `backgroundColor` (camelCase) |
| `font-size: 16px` | `fontSize: 16` (no 'px') |
| `margin: 10px 20px` | `marginVertical: 10, marginHorizontal: 20` |
| `display: flex` | Flexbox is default! |
| `flex-direction: row` | Default is `column` (not row!) |

### Flexbox is Default

Everything in React Native uses Flexbox, but with different defaults:

```tsx
// In React Native, flex direction is COLUMN by default
<View>
  <Text>Item 1</Text>  {/* Top */}
  <Text>Item 2</Text>  {/* Middle */}
  <Text>Item 3</Text>  {/* Bottom */}
</View>

// To make items horizontal:
<View style={{ flexDirection: 'row' }}>
  <Text>Item 1</Text>  {/* Left */}
  <Text>Item 2</Text>  {/* Middle */}
  <Text>Item 3</Text>  {/* Right */}
</View>
```

### Inline Styles (Quick but not recommended for big apps)

```tsx
<View style={{ padding: 10, backgroundColor: 'blue' }}>
  <Text style={{ color: 'white' }}>Hello</Text>
</View>
```

### Combining Styles

```tsx
// Array of styles (later ones override earlier ones)
<Text style={[styles.text, styles.bold, { color: 'red' }]}>
  Hello
</Text>
```

---

## React Hooks You Need to Know

### 1. `useState` - Store and update data

```tsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);  // Start at 0
  
  return (
    <Pressable onPress={() => setCount(count + 1)}>
      <Text>Count: {count}</Text>
    </Pressable>
  );
}
```

**How it works:**
- `count` = the current value
- `setCount` = function to change the value
- `useState(0)` = starting value is 0

### 2. `useEffect` - Do something when component loads or data changes

```tsx
import { useEffect, useState } from 'react';

function MyComponent() {
  const [data, setData] = useState(null);
  
  // Runs ONCE when component first appears
  useEffect(() => {
    console.log('Component loaded!');
    fetchData();
  }, []);  // Empty array = run once
  
  // Runs every time 'count' changes
  useEffect(() => {
    console.log('Count changed to:', count);
  }, [count]);  // Watch 'count'
  
  return <Text>Hello</Text>;
}
```

### 3. `useRef` - Store value that doesn't cause re-render

```tsx
import { useRef } from 'react';

function Timer() {
  const timerRef = useRef(null);
  
  const startTimer = () => {
    timerRef.current = setTimeout(() => {
      console.log('Done!');
    }, 3000);
  };
  
  const stopTimer = () => {
    clearTimeout(timerRef.current);
  };
  
  return (
    <View>
      <Pressable onPress={startTimer}><Text>Start</Text></Pressable>
      <Pressable onPress={stopTimer}><Text>Stop</Text></Pressable>
    </View>
  );
}
```

---

## Expo Router (File-Based Navigation)

Your project uses **Expo Router** - it creates navigation based on file names!

### How It Works

```
app/
  _layout.tsx      → Root layout (wraps everything)
  (tabs)/
    _layout.tsx    → Tab bar layout
    index.tsx      → First tab (Home) - shows at "/"
    explore.tsx    → Second tab - shows at "/explore"
  modal.tsx        → Modal screen - shows at "/modal"
```

### The `_layout.tsx` Files

These files define HOW screens are shown (tabs, stack, etc.):

```tsx
// app/_layout.tsx - Root layout
import { Stack } from 'expo-router';

export default function RootLayout() {
  return (
    <Stack>
      <Stack.Screen name="(tabs)" options={{ headerShown: false }} />
      <Stack.Screen name="modal" options={{ presentation: 'modal' }} />
    </Stack>
  );
}
```

```tsx
// app/(tabs)/_layout.tsx - Tab layout
import { Tabs } from 'expo-router';

export default function TabLayout() {
  return (
    <Tabs>
      <Tabs.Screen name="index" options={{ title: 'Home' }} />
      <Tabs.Screen name="explore" options={{ title: 'Explore' }} />
    </Tabs>
  );
}
```

### Navigation Between Screens

```tsx
import { Link, router } from 'expo-router';

// Method 1: Link component
<Link href="/explore">Go to Explore</Link>

// Method 2: Programmatic navigation
<Pressable onPress={() => router.push('/explore')}>
  <Text>Go to Explore</Text>
</Pressable>
```

---

## Common Patterns

### Conditional Rendering

```tsx
function MyComponent() {
  const [isLoading, setIsLoading] = useState(true);
  const [items, setItems] = useState([]);
  
  // Show loading spinner
  if (isLoading) {
    return <Text>Loading...</Text>;
  }
  
  // Show empty state
  if (items.length === 0) {
    return <Text>No items yet!</Text>;
  }
  
  // Show list
  return (
    <FlatList
      data={items}
      renderItem={({ item }) => <Text>{item.name}</Text>}
    />
  );
}
```

### Handling User Input

```tsx
function AddTask() {
  const [text, setText] = useState('');
  
  const handleAdd = () => {
    if (!text.trim()) return;  // Don't add empty tasks
    
    // Do something with text
    console.log('Adding:', text);
    
    setText('');  // Clear input
  };
  
  return (
    <View>
      <TextInput
        value={text}
        onChangeText={setText}
        placeholder="Enter task..."
      />
      <Pressable onPress={handleAdd}>
        <Text>Add</Text>
      </Pressable>
    </View>
  );
}
```

---

## Platform-Specific Code

Sometimes iOS and Android need different code:

```tsx
import { Platform } from 'react-native';

// Check platform
if (Platform.OS === 'ios') {
  console.log('Running on iPhone');
} else if (Platform.OS === 'android') {
  console.log('Running on Android');
}

// Platform-specific styles
const styles = StyleSheet.create({
  container: {
    paddingTop: Platform.OS === 'ios' ? 50 : 30,
    // Or use Platform.select:
    ...Platform.select({
      ios: { shadowColor: 'black' },
      android: { elevation: 5 },
    }),
  },
});
```

---

## Running Your App

### Development Commands

```bash
# Start the development server
npx expo start

# Then press:
# 'i' - Open iOS simulator
# 'a' - Open Android emulator
# 'w' - Open in web browser
# Scan QR code - Open on your phone (needs Expo Go app)
```

### Common Issues

1. **"Text strings must be rendered within a <Text> component"**
   - You have text outside of `<Text>` tags

2. **"VirtualizedLists should never be nested"**
   - Don't put `FlatList` inside `ScrollView`

3. **Styles not working**
   - Check for typos (camelCase!)
   - Make sure you're using numbers, not strings with 'px'

---

## Quick Reference Card

```tsx
// Basic component structure
import { View, Text, StyleSheet } from 'react-native';

export default function MyScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.text}>Hello!</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
  },
  text: {
    fontSize: 18,
  },
});
```

---

## Resources for Learning More

- [React Native Docs](https://reactnative.dev/docs/getting-started)
- [Expo Docs](https://docs.expo.dev/)
- [Expo Router Docs](https://docs.expo.dev/router/introduction/)

---

**Next:** Read `02-typescript-essentials.md` to learn TypeScript basics!

