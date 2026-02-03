# Project Maintenance Guide

> How to run, update, and improve your Do Meows app

---

## Running the Project

### First Time Setup

```bash
# 1. Navigate to project folder
cd expo-react-native-checklist

# 2. Install dependencies
npm install

# 3. Start the development server
npx expo start
```

### Running on Different Platforms

After `npx expo start`, press:

| Key | Action |
|-----|--------|
| `i` | Open iOS Simulator (Mac only) |
| `a` | Open Android Emulator |
| `w` | Open in web browser |
| `r` | Reload the app |
| `m` | Toggle menu |
| `j` | Open debugger |

### Running on Your Phone

1. Install **Expo Go** app from App Store / Play Store
2. Run `npx expo start`
3. Scan the QR code with your phone camera (iOS) or Expo Go app (Android)

---

## Project File Map

```
📁 Your Project
├── 📁 app/                    # SCREENS GO HERE
│   ├── _layout.tsx            # Root navigation setup
│   ├── 📁 (tabs)/             # Tab-based screens
│   │   ├── _layout.tsx        # Tab bar configuration
│   │   └── index.tsx          # ⭐ MAIN TASK LIST SCREEN
│   └── modal.tsx              # Modal screen (not used)
│
├── 📁 components/             # REUSABLE COMPONENTS
│   ├── themed-text.tsx        # Text with theme colors
│   ├── themed-view.tsx        # View with theme colors
│   └── 📁 ui/                 # UI components
│
├── 📁 constants/
│   └── theme.ts               # ⭐ COLORS DEFINED HERE
│
├── 📁 hooks/                  # CUSTOM HOOKS
│   ├── use-color-scheme.ts    # Detect light/dark mode
│   └── use-theme-color.ts     # Get theme colors
│
├── 📁 assets/
│   └── 📁 images/cats/        # Cat GIF feedback images
│
└── package.json               # Dependencies
```

---

## Common Updates

### Adding a New Task Feature

Let's say you want to add an "Edit Task" feature:

#### Step 1: Update the Task Type

```typescript
// In app/(tabs)/index.tsx, update TaskType if needed
export type TaskType = {
  id: string;
  text: string;
  completed: boolean;
  // Add new fields here if needed
  createdAt?: Date;
};
```

#### Step 2: Add State for Edit Mode

```typescript
// Add new state
const [editingId, setEditingId] = useState<string | null>(null);
const [editText, setEditText] = useState('');
```

#### Step 3: Add Edit Function

```typescript
const handleStartEdit = (task: TaskType) => {
  setEditingId(task.id);
  setEditText(task.text);
};

const handleSaveEdit = () => {
  if (!editText.trim()) return;
  
  setTasks(prev => prev.map(t =>
    t.id === editingId ? { ...t, text: editText.trim() } : t
  ));
  
  setEditingId(null);
  setEditText('');
};

const handleCancelEdit = () => {
  setEditingId(null);
  setEditText('');
};
```

#### Step 4: Update UI

```tsx
// In the task row, show edit mode or normal mode
{editingId === task.id ? (
  // Edit mode
  <View style={styles.editRow}>
    <TextInput
      value={editText}
      onChangeText={setEditText}
      style={styles.editInput}
    />
    <Pressable onPress={handleSaveEdit}>
      <Text>Save</Text>
    </Pressable>
    <Pressable onPress={handleCancelEdit}>
      <Text>Cancel</Text>
    </Pressable>
  </View>
) : (
  // Normal mode (existing code)
  <View style={styles.taskRow}>
    {/* ... existing task display ... */}
    <Pressable onPress={() => handleStartEdit(task)}>
      <Text>Edit</Text>
    </Pressable>
  </View>
)}
```

---

### Changing Colors/Theme

Edit `constants/theme.ts`:

```typescript
export const Colors = {
  light: {
    text: '#1a2634',           // Main text color
    background: '#e8f2fa',     // Screen background
    card: '#f5f9fd',           // Task card background
    border: '#c5d9ed',         // Card borders
    tint: '#5b9bd5',           // Accent color (buttons, etc.)
    icon: '#5b7a9a',           // Icon color
    delete: '#b85c58',         // Delete button color
  },
  dark: {
    // Same keys, different values for dark mode
    text: '#e8eef4',
    background: '#1a2332',
    // ... etc
  },
};
```

**To use a color in a component:**

```typescript
import { useThemeColor } from '@/hooks/use-theme-color';

function MyComponent() {
  const backgroundColor = useThemeColor({}, 'background');
  const textColor = useThemeColor({}, 'text');
  
  return (
    <View style={{ backgroundColor }}>
      <Text style={{ color: textColor }}>Hello</Text>
    </View>
  );
}
```

---

### Adding a New Screen

#### Step 1: Create the file

Create `app/(tabs)/settings.tsx`:

```typescript
import { View, Text, StyleSheet } from 'react-native';
import { ThemedText } from '@/components/themed-text';
import { ThemedView } from '@/components/themed-view';

export default function SettingsScreen() {
  return (
    <ThemedView style={styles.container}>
      <ThemedText type="title">Settings</ThemedText>
    </ThemedView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
  },
});
```

#### Step 2: Add to tab layout

Edit `app/(tabs)/_layout.tsx`:

```typescript
export default function TabLayout() {
  return (
    <Tabs>
      <Tabs.Screen
        name="index"
        options={{
          title: 'Tasks',
          tabBarIcon: ({ color }) => <IconSymbol name="checklist" color={color} />,
        }}
      />
      {/* Add new tab */}
      <Tabs.Screen
        name="settings"
        options={{
          title: 'Settings',
          tabBarIcon: ({ color }) => <IconSymbol name="gearshape.fill" color={color} />,
        }}
      />
    </Tabs>
  );
}
```

---

### Adding Data Persistence

Install AsyncStorage:

```bash
npx expo install @react-native-async-storage/async-storage
```

Update your main screen:

```typescript
import AsyncStorage from '@react-native-async-storage/async-storage';

const STORAGE_KEY = 'do_meows_tasks';

export default function HomeScreen() {
  const [tasks, setTasks] = useState<TaskType[]>([]);
  const [isLoading, setIsLoading] = useState(true);

  // Load tasks on app start
  useEffect(() => {
    const loadTasks = async () => {
      try {
        const saved = await AsyncStorage.getItem(STORAGE_KEY);
        if (saved) {
          setTasks(JSON.parse(saved));
        }
      } catch (error) {
        console.error('Failed to load tasks:', error);
      } finally {
        setIsLoading(false);
      }
    };
    
    loadTasks();
  }, []);

  // Save tasks whenever they change
  useEffect(() => {
    if (!isLoading) {
      AsyncStorage.setItem(STORAGE_KEY, JSON.stringify(tasks));
    }
  }, [tasks, isLoading]);

  if (isLoading) {
    return <Text>Loading...</Text>;
  }

  // ... rest of component
}
```

---

### Adding Animations

Install Reanimated:

```bash
npx expo install react-native-reanimated
```

Add to `babel.config.js`:

```javascript
module.exports = function(api) {
  api.cache(true);
  return {
    presets: ['babel-preset-expo'],
    plugins: ['react-native-reanimated/plugin'],
  };
};
```

Example: Fade in animation for tasks:

```typescript
import Animated, { FadeIn, FadeOut } from 'react-native-reanimated';

// In your task list:
{tasks.map((task) => (
  <Animated.View
    key={task.id}
    entering={FadeIn.duration(300)}
    exiting={FadeOut.duration(200)}
  >
    {/* Task content */}
  </Animated.View>
))}
```

---

### Using FlatList Instead of ScrollView

For better performance with many tasks:

```typescript
// Before (current code)
<ScrollView>
  {tasks.map((task) => (
    <TaskItem key={task.id} task={task} />
  ))}
</ScrollView>

// After (better for long lists)
<FlatList
  data={tasks}
  keyExtractor={(item) => item.id}
  renderItem={({ item }) => (
    <TaskItem task={item} />
  )}
  ListEmptyComponent={<Text>No tasks yet!</Text>}
/>
```

---

## Debugging Tips

### Common Errors

#### "Text strings must be rendered within a <Text> component"

```typescript
// ❌ Wrong
<View>Hello World</View>

// ✅ Correct
<View><Text>Hello World</Text></View>
```

#### "Each child in a list should have a unique 'key' prop"

```typescript
// ❌ Wrong
{items.map(item => <Text>{item.name}</Text>)}

// ✅ Correct
{items.map(item => <Text key={item.id}>{item.name}</Text>)}
```

#### "Cannot read property 'X' of undefined"

```typescript
// ❌ Might crash if user is null
<Text>{user.name}</Text>

// ✅ Safe with optional chaining
<Text>{user?.name}</Text>

// ✅ Or check first
{user && <Text>{user.name}</Text>}
```

### Debugging Tools

1. **Console.log** - Add anywhere to see values
   ```typescript
   console.log('Tasks:', tasks);
   ```

2. **React DevTools** - Press `j` in terminal to open debugger

3. **Shake device** - Opens developer menu on phone

---

## Useful Commands

```bash
# Start development server
npx expo start

# Start with cache cleared
npx expo start --clear

# Run linter
npm run lint

# Install a package
npx expo install <package-name>

# Check for outdated packages
npx expo-doctor

# Build for production (requires Expo account)
eas build --platform ios
eas build --platform android
```

---

## Adding New Dependencies

Always use `npx expo install` instead of `npm install` for Expo projects:

```bash
# ✅ Correct - installs compatible version
npx expo install react-native-reanimated

# ❌ Avoid - might install incompatible version
npm install react-native-reanimated
```

---

## Git Workflow

### Before Making Changes

```bash
# Create a new branch
git checkout -b feature/edit-task

# Make your changes...
```

### After Making Changes

```bash
# See what changed
git status

# Add changes
git add .

# Commit with message
git commit -m "Add edit task feature"

# Push to GitHub
git push origin feature/edit-task
```

### Good Commit Messages

```
✅ Good:
- "Add edit task feature"
- "Fix task completion toggle bug"
- "Update theme colors for dark mode"
- "Add data persistence with AsyncStorage"

❌ Bad:
- "fix"
- "update"
- "changes"
- "asdfasdf"
```

---

## Quick Reference: File Locations

| What you want to change | File to edit |
|------------------------|--------------|
| Main task list logic | `app/(tabs)/index.tsx` |
| Colors/theme | `constants/theme.ts` |
| Tab bar setup | `app/(tabs)/_layout.tsx` |
| Root navigation | `app/_layout.tsx` |
| Themed text styles | `components/themed-text.tsx` |
| Cat feedback images | `assets/images/cats/` |

---

## Next Steps for Your Project

1. **Add persistence** - Save tasks to device storage
2. **Add FlatList** - Better performance for many tasks
3. **Add edit feature** - Let users modify tasks
4. **Add categories** - Organize tasks by type
5. **Add due dates** - Schedule tasks
6. **Add notifications** - Remind users of tasks
7. **Add tests** - Ensure code works correctly

---

Good luck with your interview! 🎉

