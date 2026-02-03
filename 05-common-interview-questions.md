# Common Interview Questions

> Questions they might ask and how to answer them

---

## About Your Project

### "Walk me through your task manager app"

```
"I built 'Do Meows', a task manager using React Native and Expo. 

The main features are:
- Add tasks with a text input
- Mark tasks as complete (with visual strikethrough)
- Delete tasks
- Cat GIF animations for feedback

I used useState for state management, TypeScript for type safety, 
and created a theme system that supports light and dark mode.

The feedback system uses useRef for the timeout to avoid memory leaks,
and I used expo-image for better GIF performance."
```

### "Why did you use useState instead of Redux?"

```
"The requirements specified local component state only. 

For a simple app like this, useState is perfect - it's simpler, 
has less boilerplate, and the state doesn't need to be shared 
across many components.

If this app grew to have multiple screens that needed to share 
task data, I'd consider Context API first, then Redux or Zustand 
for more complex state."
```

### "How would you add data persistence?"

```
"I'd use AsyncStorage from @react-native-async-storage/async-storage.

When tasks change, I'd save to storage:
  useEffect(() => {
    AsyncStorage.setItem('tasks', JSON.stringify(tasks));
  }, [tasks]);

On app load, I'd read from storage:
  useEffect(() => {
    const loadTasks = async () => {
      const saved = await AsyncStorage.getItem('tasks');
      if (saved) setTasks(JSON.parse(saved));
    };
    loadTasks();
  }, []);

For a production app, I might use a database like SQLite or 
sync with a backend API."
```

### "What would you improve about your code?"

```
"A few things:

1. Use FlatList instead of ScrollView + map() for better 
   performance with many tasks

2. Add animations with react-native-reanimated for smoother 
   task additions/deletions

3. Add data persistence with AsyncStorage

4. Add an edit feature for tasks

5. Add unit tests for the task functions

6. Extract the task item into its own component for better 
   code organization"
```

---

## React Native Questions

### "What's the difference between React and React Native?"

```
"React is for building web apps, React Native is for mobile apps.

Key differences:
- React uses HTML elements (div, span, p)
- React Native uses native components (View, Text, Image)

- React uses CSS for styling
- React Native uses JavaScript objects with StyleSheet

- React runs in a browser
- React Native compiles to native iOS/Android code

The component logic and hooks (useState, useEffect) work the same way."
```

### "What's the difference between ScrollView and FlatList?"

```
"ScrollView renders ALL items at once. Good for short lists.

FlatList only renders items that are visible on screen. 
Much better for long lists because:
- Uses less memory
- Faster initial render
- Smoother scrolling

Rule of thumb: 
- Under 20 items → ScrollView is fine
- Over 20 items → Use FlatList"
```

### "How do you handle different screen sizes?"

```
"Several approaches:

1. Flexbox - Use flex: 1 and percentages instead of fixed sizes

2. Dimensions API - Get screen width/height:
   const { width, height } = Dimensions.get('window');

3. useWindowDimensions hook - Reactive to orientation changes:
   const { width, height } = useWindowDimensions();

4. Platform-specific code:
   Platform.OS === 'ios' ? iosStyle : androidStyle

5. Responsive libraries like react-native-responsive-screen"
```

### "What is Expo? Why use it?"

```
"Expo is a framework built on top of React Native that makes 
development easier.

Benefits:
- Quick setup (no Xcode/Android Studio needed to start)
- Easy testing with Expo Go app
- Built-in libraries (camera, location, etc.)
- Over-the-air updates
- Easier build process

Limitations:
- Can't use all native modules
- Larger app size
- Less control over native code

For most apps, Expo is great. For apps needing deep native 
integration (Bluetooth, background processing), you might 
need bare workflow or eject."
```

### "How do you debug React Native apps?"

```
"Several tools:

1. Console.log - Simple but effective

2. React Native Debugger - Full debugging with breakpoints

3. Flipper - Facebook's debugging tool, shows network requests, 
   layout, etc.

4. React DevTools - Inspect component tree and state

5. Expo's error overlay - Shows errors with stack trace

For performance issues, I use the Performance Monitor 
(shake device → Show Perf Monitor) to check FPS and memory."
```

---

## TypeScript Questions

### "Why use TypeScript?"

```
"TypeScript catches errors before runtime.

Example: If I type 'complted' instead of 'completed', 
TypeScript shows an error immediately. In JavaScript, 
this bug might only appear when a user triggers that code.

Other benefits:
- Better autocomplete in IDE
- Self-documenting code
- Easier refactoring
- Catches null/undefined errors"
```

### "What's the difference between interface and type?"

```
"Both define object shapes, but:

Interface:
- Can be extended (interface B extends A)
- Can be merged (declare same interface twice)
- Better for objects and classes

Type:
- Can define unions (type Status = 'a' | 'b')
- Can define primitives (type ID = string)
- More flexible

My rule: Use interface for objects, type for everything else."
```

### "What are generics?"

```
"Generics let you write reusable code that works with any type.

Example without generics - need separate functions:
  function firstString(arr: string[]): string { return arr[0]; }
  function firstNumber(arr: number[]): number { return arr[0]; }

With generics - one function works for all:
  function first<T>(arr: T[]): T { return arr[0]; }
  
  first<string>(['a', 'b']);  // returns 'a'
  first<number>([1, 2]);      // returns 1

Common use: useState<Task[]>([]) tells TypeScript the state 
is an array of Task objects."
```

---

## JavaScript Questions

### "What's the difference between let, const, and var?"

```
"var - Old way, function-scoped, can be redeclared (avoid it)

let - Block-scoped, can be reassigned
  let count = 0;
  count = 1;  // OK

const - Block-scoped, cannot be reassigned
  const name = 'Mohamed';
  name = 'Ali';  // Error!
  
  // But objects/arrays can be modified:
  const arr = [1, 2];
  arr.push(3);  // OK, array itself isn't reassigned

Rule: Use const by default, let when you need to reassign."
```

### "Explain async/await"

```
"async/await is a cleaner way to handle promises.

Old way with .then():
  fetch(url)
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(error => console.error(error));

With async/await:
  async function getData() {
    try {
      const response = await fetch(url);
      const data = await response.json();
      console.log(data);
    } catch (error) {
      console.error(error);
    }
  }

The await keyword pauses execution until the promise resolves.
Must be inside an async function."
```

### "What is destructuring?"

```
"Destructuring extracts values from objects or arrays.

Object destructuring:
  const user = { name: 'Mohamed', age: 25 };
  const { name, age } = user;
  // name = 'Mohamed', age = 25

Array destructuring:
  const [first, second] = [1, 2, 3];
  // first = 1, second = 2

With defaults:
  const { name, country = 'Egypt' } = user;

Renaming:
  const { name: userName } = user;
  // userName = 'Mohamed'

Very common in React:
  const [count, setCount] = useState(0);
  const { title, onPress } = props;"
```

### "What is the spread operator?"

```
"The spread operator (...) expands arrays or objects.

Arrays:
  const arr1 = [1, 2];
  const arr2 = [...arr1, 3, 4];  // [1, 2, 3, 4]

Objects:
  const user = { name: 'Mohamed' };
  const updated = { ...user, age: 25 };  // { name: 'Mohamed', age: 25 }

Common in React for immutable updates:
  // Add to array
  setTasks([...tasks, newTask]);
  
  // Update object in array
  setTasks(tasks.map(t => 
    t.id === id ? { ...t, completed: true } : t
  ));"
```

---

## React Hooks Questions

### "Explain useState"

```
"useState stores data that can change and triggers re-render.

const [count, setCount] = useState(0);

- count: current value
- setCount: function to update value
- useState(0): initial value is 0

When setCount is called, React re-renders the component 
with the new value.

Important: State updates are asynchronous!
  setCount(count + 1);
  console.log(count);  // Still old value!

For updates based on previous value:
  setCount(prev => prev + 1);  // Safer"
```

### "Explain useEffect"

```
"useEffect runs side effects (data fetching, subscriptions, etc.)

// Runs after EVERY render
useEffect(() => {
  console.log('Rendered!');
});

// Runs ONCE on mount (empty dependency array)
useEffect(() => {
  fetchData();
}, []);

// Runs when 'count' changes
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]);

// Cleanup function (runs before next effect or unmount)
useEffect(() => {
  const timer = setInterval(() => {}, 1000);
  return () => clearInterval(timer);  // Cleanup!
}, []);"
```

### "Explain useRef"

```
"useRef stores a value that persists between renders 
but doesn't cause re-render when changed.

Common uses:

1. Store timeout/interval IDs:
   const timerRef = useRef(null);
   timerRef.current = setTimeout(...);
   clearTimeout(timerRef.current);

2. Access DOM elements:
   const inputRef = useRef(null);
   <TextInput ref={inputRef} />
   inputRef.current.focus();

3. Store previous values:
   const prevCount = useRef(count);
   useEffect(() => {
     prevCount.current = count;
   }, [count]);"
```

---

## Problem-Solving Questions

### "How would you implement a search filter?"

```typescript
function SearchableList() {
  const [items] = useState(['Apple', 'Banana', 'Orange', 'Grape']);
  const [search, setSearch] = useState('');
  
  const filteredItems = items.filter(item =>
    item.toLowerCase().includes(search.toLowerCase())
  );
  
  return (
    <View>
      <TextInput
        value={search}
        onChangeText={setSearch}
        placeholder="Search..."
      />
      {filteredItems.map(item => (
        <Text key={item}>{item}</Text>
      ))}
    </View>
  );
}
```

### "How would you debounce a search input?"

```typescript
function DebouncedSearch() {
  const [search, setSearch] = useState('');
  const [debouncedSearch, setDebouncedSearch] = useState('');
  
  useEffect(() => {
    const timer = setTimeout(() => {
      setDebouncedSearch(search);
    }, 300);  // Wait 300ms after user stops typing
    
    return () => clearTimeout(timer);  // Clear if user types again
  }, [search]);
  
  useEffect(() => {
    if (debouncedSearch) {
      // Make API call here
      console.log('Searching for:', debouncedSearch);
    }
  }, [debouncedSearch]);
  
  return (
    <TextInput
      value={search}
      onChangeText={setSearch}
      placeholder="Search..."
    />
  );
}
```

### "How would you fetch data from an API?"

```typescript
function DataFetcher() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch('https://api.example.com/data');
        if (!response.ok) throw new Error('Failed to fetch');
        const json = await response.json();
        setData(json);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };
    
    fetchData();
  }, []);
  
  if (loading) return <Text>Loading...</Text>;
  if (error) return <Text>Error: {error}</Text>;
  return <Text>{JSON.stringify(data)}</Text>;
}
```

---

## Behavioral Questions

### "Tell me about yourself"

```
"I'm Mohamed, a developer passionate about building mobile apps. 

I recently completed a React Native task manager app for your 
assessment, which was my first Expo project. I enjoyed learning 
the framework and implementing features like theme support and 
animated feedback.

I'm excited about this role because I want to grow my mobile 
development skills and work with a team using modern tools 
like Cursor IDE and AI-assisted workflows."
```

### "Why do you want to work here?"

```
"Three reasons:

1. The tech stack - React Native, TypeScript, NestJS - these are 
   technologies I want to master, and working with them daily 
   would accelerate my learning.

2. The AI-assisted workflow - I'm already using Cursor IDE and 
   find it incredibly productive. A team that embraces these 
   tools is forward-thinking.

3. Remote work with a global team - I value the flexibility and 
   the opportunity to collaborate with diverse perspectives."
```

### "What's your biggest weakness?"

```
"I sometimes spend too much time trying to find the perfect 
solution instead of shipping a working version first.

I'm working on this by setting time limits for research and 
reminding myself that 'done is better than perfect' - I can 
always iterate and improve later."
```

---

## Questions to Ask Them

```
Technical:
- "What does the development workflow look like day-to-day?"
- "How do you handle code reviews?"
- "What's your testing strategy for mobile apps?"
- "How often do you release to the app stores?"

Team:
- "How big is the engineering team?"
- "How do you onboard new developers?"
- "What's the balance between new features and maintenance?"

Growth:
- "What learning opportunities are available?"
- "How do you support junior developers?"
- "What does success look like in this role after 6 months?"
```

---

**Next:** Read `06-project-maintenance.md` to learn how to update your project!

