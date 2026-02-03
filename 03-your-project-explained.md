# Your Project Explained: Do Meows

> Everything you built and why - be ready to explain this in your interview!

---

## What You Built

**Do Meows** - A cat-themed task manager app built with React Native and Expo.

### The Requirements (From Chapter One)

You were asked to build a Task Manager with:
- ✅ **Add Task** — User types and adds to list
- ✅ **Mark Complete** — Toggle tasks as done (with visual change)
- ✅ **Delete Task** — Remove tasks from list
- ✅ **Task List** — Show all tasks
- ✅ **Feedback** — Visual feedback when actions happen

**Rules you followed:**
- Local state only (`useState`) - no Redux or database
- Default React Native components (no extra UI libraries)

---

## Project Structure

```
expo-react-native-checklist/
├── app/                          # Screens (Expo Router)
│   ├── _layout.tsx               # Root layout (theme, navigation)
│   ├── (tabs)/                   # Tab navigation group
│   │   ├── _layout.tsx           # Tab bar setup
│   │   └── index.tsx             # Main screen (your task list!)
│   └── modal.tsx                 # Modal screen (not used)
├── components/                   # Reusable components
│   ├── themed-text.tsx           # Text with theme colors
│   ├── themed-view.tsx           # View with theme colors
│   └── ui/                       # UI components
├── constants/
│   └── theme.ts                  # Color definitions
├── hooks/                        # Custom React hooks
│   ├── use-color-scheme.ts       # Detect light/dark mode
│   └── use-theme-color.ts        # Get theme colors
├── assets/
│   └── images/cats/              # Cat GIF feedback images
└── package.json                  # Dependencies
```

---

## The Main Screen: `app/(tabs)/index.tsx`

This is where all the magic happens! Let's break it down:

### 1. Imports and Types

```typescript
import { useRef, useState } from 'react';
import { nanoid } from 'nanoid/non-secure';
import { Image } from 'expo-image';
import { Pressable, ScrollView, StyleSheet, TextInput, View } from 'react-native';

// Type for a single task
export type TaskType = {
  id: string;       // Unique identifier
  text: string;     // Task description
  completed: boolean; // Is it done?
};

// Type for feedback animations
type FeedbackType = 'completed' | 'deleted' | 'added' | 'uncompleted';
```

**Why these choices?**
- `nanoid` - Creates unique IDs for each task (needed for React's key prop)
- `expo-image` - Better than React Native's Image for GIFs
- Union type for `FeedbackType` - Only allows these 4 specific values

### 2. State Management

```typescript
const [tasks, setTasks] = useState<TaskType[]>([]);           // All tasks
const [inputText, setInputText] = useState('');               // Input field value
const [feedbackType, setFeedbackType] = useState<FeedbackType | null>(null);  // Current feedback
const [feedbackImageError, setFeedbackImageError] = useState(false);  // If GIF fails to load
const feedbackTimeoutRef = useRef<ReturnType<typeof setTimeout> | null>(null);  // Timer reference
```

**Why `useRef` for timeout?**
- We need to clear the timeout if user does another action quickly
- `useRef` doesn't cause re-renders when changed
- Persists between renders

### 3. The Feedback System

```typescript
const FEEDBACK_DURATION_MS = 3500;  // Show for 3.5 seconds

const CAT_IMAGES: Record<FeedbackType, number> = {
  completed: require('@/assets/images/cats/cat-yay.gif'),
  deleted: require('@/assets/images/cats/cat-sus.gif'),
  added: require('@/assets/images/cats/cat-watching.gif'),
  uncompleted: require('@/assets/images/cats/cat-close.gif'),
};

const showFeedback = (type: FeedbackType) => {
  // Clear any existing timeout
  if (feedbackTimeoutRef.current !== null) {
    clearTimeout(feedbackTimeoutRef.current);
  }
  
  // Show the feedback
  setFeedbackImageError(false);
  setFeedbackType(type);
  
  // Hide after duration
  feedbackTimeoutRef.current = setTimeout(() => {
    setFeedbackType(null);
    setFeedbackImageError(false);
  }, FEEDBACK_DURATION_MS);
};
```

**What this does:**
1. When user does an action, show a cat GIF
2. After 3.5 seconds, hide it
3. If user does another action before timeout, reset the timer

### 4. Core Functions

#### Adding a Task

```typescript
const handleAddTask = () => {
  if (!inputText.trim()) return;  // Don't add empty tasks
  
  setTasks((prev) => [
    ...prev,
    { id: nanoid(), text: inputText.trim(), completed: false },
  ]);
  
  setInputText('');  // Clear input
  showFeedback('added');
};
```

**Key points:**
- `trim()` removes whitespace
- `nanoid()` creates unique ID
- Spread operator `...prev` keeps existing tasks
- New task added at end of array

#### Toggling Complete

```typescript
const handleToggleComplete = (id: string) => {
  setTasks((prev) => {
    const next = prev.map((t) =>
      t.id === id ? { ...t, completed: !t.completed } : t
    );
    
    // Show appropriate feedback
    const task = next.find((t) => t.id === id);
    if (task?.completed) showFeedback('completed');
    else showFeedback('uncompleted');
    
    return next;
  });
};
```

**Key points:**
- `map()` creates new array with one task changed
- `!t.completed` flips the boolean
- Different feedback for complete vs uncomplete

#### Deleting a Task

```typescript
const handleDelete = (id: string) => {
  setTasks((prev) => prev.filter((t) => t.id !== id));
  showFeedback('deleted');
};
```

**Key points:**
- `filter()` keeps all tasks EXCEPT the one with matching id
- Simple and clean!

### 5. The UI Structure

```tsx
return (
  <View style={[styles.screen, { backgroundColor: background }]}>
    <ScrollView
      style={[styles.scrollView, { backgroundColor: background }]}
      contentContainerStyle={styles.screenContent}
      keyboardShouldPersistTaps="handled"  // Important for input!
    >
      {/* Title */}
      <View style={styles.titleBlock}>
        <ThemedText type="title">Do Meows</ThemedText>
        <ThemedText>~ your daily scratch list ~</ThemedText>
      </View>
      
      {/* Add Task Input */}
      <ThemedView style={styles.addTaskContainer}>
        <TextInput
          value={inputText}
          onChangeText={setInputText}
          placeholder="New task..."
        />
        <Pressable onPress={handleAddTask}>
          <ThemedText>Add</ThemedText>
        </Pressable>
      </ThemedView>
      
      {/* Task List */}
      <ThemedView style={styles.taskList}>
        {tasks.map((task) => (
          <View key={task.id} style={styles.taskRow}>
            {/* Checkbox */}
            {/* Task text */}
            {/* Complete/Delete buttons */}
          </View>
        ))}
      </ThemedView>
    </ScrollView>
    
    {/* Feedback Overlay */}
    {feedbackType !== null && (
      <View style={styles.feedbackOverlay}>
        <Image source={CAT_IMAGES[feedbackType]} />
      </View>
    )}
  </View>
);
```

**Important prop:** `keyboardShouldPersistTaps="handled"`
- Without this, tapping "Add" would dismiss keyboard instead of adding task!

---

## Theme System

### `constants/theme.ts`

```typescript
export const Colors = {
  light: {
    text: '#1a2634',
    background: '#e8f2fa',
    card: '#f5f9fd',
    border: '#c5d9ed',
    tint: '#5b9bd5',      // Main accent color (soft blue)
    delete: '#b85c58',    // Red for delete
  },
  dark: {
    text: '#e8eef4',
    background: '#1a2332',
    card: '#243447',
    border: '#3a4d66',
    tint: '#7eb8f0',
    delete: '#e09898',
  },
};
```

**Design choice:** Soft blue theme instead of default colors. Shows attention to design!

### `hooks/use-theme-color.ts`

```typescript
export function useThemeColor(
  props: { light?: string; dark?: string },
  colorName: keyof typeof Colors.light & keyof typeof Colors.dark
) {
  const theme = useColorScheme() ?? 'light';
  const colorFromProps = props[theme];

  if (colorFromProps) {
    return colorFromProps;
  } else {
    return Colors[theme][colorName];
  }
}
```

**What this does:**
- Detects if phone is in light or dark mode
- Returns the right color automatically
- Can override with custom colors if needed

---

## Reusable Components

### `ThemedText`

```typescript
export function ThemedText({
  style,
  lightColor,
  darkColor,
  type = 'default',
  ...rest
}: ThemedTextProps) {
  const color = useThemeColor({ light: lightColor, dark: darkColor }, 'text');

  return (
    <Text
      style={[
        { color },
        type === 'title' ? styles.title : undefined,
        // ... other type styles
        style,
      ]}
      {...rest}
    />
  );
}
```

**Why this is good:**
- Automatically uses theme colors
- Has preset styles (title, subtitle, etc.)
- Can still override with custom styles

---

## Styling Patterns Used

### 1. Dynamic Styles with Theme

```typescript
<View style={[styles.screen, { backgroundColor: background }]}>
```

Combines static styles with dynamic theme color.

### 2. Pressed State Feedback

```typescript
<Pressable
  style={({ pressed }) => [
    styles.addButton,
    pressed && styles.pressedOpacity,
  ]}
>
```

Button becomes slightly transparent when pressed.

### 3. Minimum Touch Target

```typescript
const MIN_TOUCH_TARGET = 44;  // Apple's recommended minimum

const styles = StyleSheet.create({
  addButton: {
    minHeight: MIN_TOUCH_TARGET,
    // ...
  },
});
```

Ensures buttons are easy to tap on mobile!

### 4. Conditional Styles

```typescript
<ThemedText
  style={[styles.taskText, task.completed && styles.taskTextCompleted]}
>
```

Adds strikethrough style only when task is completed.

---

## What Makes This Project Good

### 1. Clean Code Organization
- Types defined at top
- Constants separated
- Functions are small and focused

### 2. Good UX Decisions
- `keyboardShouldPersistTaps` for better input handling
- Visual feedback for every action
- Fallback text if GIF fails to load
- Minimum touch targets for accessibility

### 3. TypeScript Usage
- All state is typed
- Props are typed
- Union types prevent invalid values

### 4. Theme Support
- Works in light and dark mode
- Colors are centralized
- Easy to change theme

### 5. Performance Considerations
- Using `useRef` for timeout (no unnecessary re-renders)
- Clearing timeouts to prevent memory leaks
- Using `expo-image` for better GIF performance

---

## Things You Could Improve (Good to mention in interview!)

### 1. Persistence
> "Currently tasks disappear when you close the app. I could add AsyncStorage to save tasks locally."

### 2. Better List Performance
> "For many tasks, I'd use FlatList instead of ScrollView with map() for better performance."

### 3. Animations
> "I could add smooth animations when tasks are added/removed using react-native-reanimated."

### 4. Edit Task Feature
> "Users can't edit tasks after creating them. I'd add an edit mode."

### 5. Categories/Filters
> "Could add categories or filter to show only completed/incomplete tasks."

### 6. Testing
> "I'd add unit tests for the task functions and component tests."

---

## How to Explain Your Project (30-second version)

> "I built a task manager app called 'Do Meows' using React Native and Expo. It has all the core features - add, complete, and delete tasks. I used TypeScript for type safety, created a custom theme system that supports light and dark mode, and added cat GIF animations for user feedback. The state is managed with useState hooks, and I used useRef for the feedback timer to avoid memory leaks. If I had more time, I'd add data persistence with AsyncStorage and use FlatList for better performance with large lists."

---

**Next:** Read `04-live-coding-tips.md` for interview strategies!

