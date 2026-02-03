# TypeScript Essentials

> Simple TypeScript guide for React Native developers

---

## What is TypeScript?

TypeScript is **JavaScript with types**. It helps you catch bugs before your code runs.

Think of it like spell-check for your code!

```typescript
// JavaScript - No error until you run it
let name = "Mohamed";
name = 123;  // Oops! This might cause bugs later

// TypeScript - Error immediately!
let name: string = "Mohamed";
name = 123;  // ❌ Error: Type 'number' is not assignable to type 'string'
```

---

## Basic Types

### The Simple Ones

```typescript
// String - text
let name: string = "Mohamed";

// Number - any number (integer or decimal)
let age: number = 25;
let price: number = 19.99;

// Boolean - true or false
let isActive: boolean = true;

// Array - list of items
let names: string[] = ["Ali", "Sara", "Omar"];
let numbers: number[] = [1, 2, 3, 4, 5];

// Any - turns off type checking (avoid when possible!)
let anything: any = "hello";
anything = 123;  // No error, but defeats the purpose of TypeScript
```

### Type Inference (TypeScript is Smart!)

You don't always need to write the type - TypeScript can figure it out:

```typescript
// TypeScript knows these types automatically
let name = "Mohamed";      // TypeScript knows it's a string
let age = 25;              // TypeScript knows it's a number
let isActive = true;       // TypeScript knows it's a boolean

// But sometimes you need to be explicit
let items: string[] = [];  // Empty array - TypeScript needs help
```

---

## Functions with Types

### Basic Function Types

```typescript
// Function that takes a string and returns a string
function greet(name: string): string {
  return "Hello, " + name;
}

// Function that takes a number and returns nothing (void)
function logNumber(num: number): void {
  console.log(num);
}

// Arrow function with types
const add = (a: number, b: number): number => {
  return a + b;
};
```

### Optional Parameters

```typescript
// The '?' makes a parameter optional
function greet(name: string, greeting?: string): string {
  if (greeting) {
    return greeting + ", " + name;
  }
  return "Hello, " + name;
}

greet("Mohamed");           // "Hello, Mohamed"
greet("Mohamed", "Hi");     // "Hi, Mohamed"
```

### Default Parameters

```typescript
// Default value if not provided
function greet(name: string, greeting: string = "Hello"): string {
  return greeting + ", " + name;
}

greet("Mohamed");           // "Hello, Mohamed"
greet("Mohamed", "Hi");     // "Hi, Mohamed"
```

---

## Objects and Interfaces

### Defining Object Shape with Interface

An **interface** describes what an object should look like:

```typescript
// Define the shape of a Task
interface Task {
  id: string;
  text: string;
  completed: boolean;
}

// Now TypeScript knows what a Task should have
const myTask: Task = {
  id: "1",
  text: "Learn TypeScript",
  completed: false,
};

// ❌ Error! Missing 'completed' property
const badTask: Task = {
  id: "2",
  text: "This is wrong",
};
```

### Optional Properties in Interface

```typescript
interface User {
  name: string;
  email: string;
  phone?: string;  // Optional - might not exist
}

// Both are valid:
const user1: User = { name: "Ali", email: "ali@test.com" };
const user2: User = { name: "Sara", email: "sara@test.com", phone: "123456" };
```

### Interface for Function Props

```typescript
// Define what props a component accepts
interface ButtonProps {
  title: string;
  onPress: () => void;  // Function that takes nothing, returns nothing
  disabled?: boolean;
}

function MyButton({ title, onPress, disabled }: ButtonProps) {
  return (
    <Pressable onPress={onPress} disabled={disabled}>
      <Text>{title}</Text>
    </Pressable>
  );
}
```

---

## Type vs Interface

Both can define object shapes. Use **interface** for objects, **type** for everything else:

```typescript
// Interface - for objects (can be extended)
interface Task {
  id: string;
  text: string;
}

// Type - for unions, primitives, or complex types
type TaskStatus = 'pending' | 'completed' | 'deleted';
type ID = string | number;

// Type for object (also works, but interface is preferred)
type TaskType = {
  id: string;
  text: string;
};
```

---

## Union Types (This OR That)

```typescript
// Can be string OR number
let id: string | number;
id = "abc";  // ✅ OK
id = 123;    // ✅ OK

// Can be one of specific values
type Status = 'loading' | 'success' | 'error';
let currentStatus: Status = 'loading';  // ✅ OK
currentStatus = 'done';  // ❌ Error! 'done' is not in the list
```

### Using Union Types in Your Project

```typescript
// From your project - feedback can only be these 4 values
type FeedbackType = 'completed' | 'deleted' | 'added' | 'uncompleted';

// This prevents typos!
const feedback: FeedbackType = 'complted';  // ❌ Error! Typo caught!
```

---

## Generics (Reusable Types)

Generics let you create flexible, reusable code:

```typescript
// Without generics - need separate functions
function getFirstString(arr: string[]): string {
  return arr[0];
}
function getFirstNumber(arr: number[]): number {
  return arr[0];
}

// With generics - one function works for any type!
function getFirst<T>(arr: T[]): T {
  return arr[0];
}

getFirst<string>(["a", "b", "c"]);  // Returns "a"
getFirst<number>([1, 2, 3]);        // Returns 1
```

### Common Generic You'll See

```typescript
// useState with type
const [tasks, setTasks] = useState<Task[]>([]);

// This tells TypeScript that 'tasks' is an array of Task objects
```

---

## TypeScript in React Native

### Typing Component Props

```typescript
import { Text, View, ViewProps } from 'react-native';

// Extend existing props
interface CardProps extends ViewProps {
  title: string;
  children: React.ReactNode;
}

function Card({ title, children, style, ...rest }: CardProps) {
  return (
    <View style={[styles.card, style]} {...rest}>
      <Text>{title}</Text>
      {children}
    </View>
  );
}
```

### Typing State

```typescript
// Simple state
const [count, setCount] = useState<number>(0);
const [name, setName] = useState<string>('');

// Array state
const [tasks, setTasks] = useState<Task[]>([]);

// Nullable state
const [user, setUser] = useState<User | null>(null);
```

### Typing Events

```typescript
import { NativeSyntheticEvent, TextInputChangeEventData } from 'react-native';

// For TextInput
const handleChange = (text: string) => {
  setInputValue(text);
};

<TextInput onChangeText={handleChange} />
```

---

## Common Patterns in Your Project

### Task Type Definition

```typescript
// This is from your project!
export type TaskType = {
  id: string;
  text: string;
  completed: boolean;
};
```

### Feedback Type (Union)

```typescript
// Only these 4 values are allowed
type FeedbackType = 'completed' | 'deleted' | 'added' | 'uncompleted';
```

### Record Type (Object with Known Keys)

```typescript
// Object where keys are FeedbackType and values are numbers (image sources)
const CAT_IMAGES: Record<FeedbackType, number> = {
  completed: require('./cat-yay.gif'),
  deleted: require('./cat-sus.gif'),
  added: require('./cat-watching.gif'),
  uncompleted: require('./cat-close.gif'),
};
```

### Using `keyof` and `typeof`

```typescript
// Get keys of an object type
const Colors = {
  light: { text: '#000', background: '#fff' },
  dark: { text: '#fff', background: '#000' },
};

type ColorScheme = keyof typeof Colors;  // 'light' | 'dark'
type ColorKeys = keyof typeof Colors.light;  // 'text' | 'background'
```

---

## Common TypeScript Errors and Fixes

### Error: "Property does not exist"

```typescript
// ❌ Error
const user = {};
console.log(user.name);  // Property 'name' does not exist

// ✅ Fix - Define the type
interface User {
  name: string;
}
const user: User = { name: 'Mohamed' };
console.log(user.name);  // Works!
```

### Error: "Type 'X' is not assignable to type 'Y'"

```typescript
// ❌ Error
const name: string = 123;

// ✅ Fix - Use correct type
const name: string = "Mohamed";
// OR change the type
const name: number = 123;
```

### Error: "Object is possibly 'null'"

```typescript
// ❌ Error
const [user, setUser] = useState<User | null>(null);
console.log(user.name);  // Error! user might be null

// ✅ Fix - Check for null first
if (user) {
  console.log(user.name);  // Now TypeScript knows user exists
}

// OR use optional chaining
console.log(user?.name);  // Returns undefined if user is null
```

### Error: "Argument of type 'X' is not assignable to parameter of type 'Y'"

```typescript
// ❌ Error
function greet(name: string) {
  console.log("Hello " + name);
}
greet(123);  // Error! Expected string, got number

// ✅ Fix - Pass correct type
greet("Mohamed");
```

---

## TypeScript Tips for Interviews

### 1. Always Type Your State

```typescript
// Good practice
const [items, setItems] = useState<Item[]>([]);
const [loading, setLoading] = useState<boolean>(false);
```

### 2. Use Interfaces for Props

```typescript
interface MyComponentProps {
  title: string;
  onPress: () => void;
}

function MyComponent({ title, onPress }: MyComponentProps) {
  // ...
}
```

### 3. Use Union Types for Limited Options

```typescript
type ButtonVariant = 'primary' | 'secondary' | 'danger';

interface ButtonProps {
  variant: ButtonVariant;
}
```

### 4. Handle Null/Undefined Properly

```typescript
// Optional chaining
const name = user?.profile?.name;

// Nullish coalescing (default value)
const name = user?.name ?? 'Guest';
```

---

## Quick Reference

```typescript
// Basic types
let str: string = "hello";
let num: number = 42;
let bool: boolean = true;
let arr: string[] = ["a", "b"];

// Function
function add(a: number, b: number): number {
  return a + b;
}

// Interface
interface User {
  name: string;
  age?: number;  // optional
}

// Type alias
type Status = 'loading' | 'success' | 'error';

// Generic
function first<T>(arr: T[]): T {
  return arr[0];
}

// React state with type
const [items, setItems] = useState<Item[]>([]);
```

---

**Next:** Read `03-your-project-explained.md` to understand what you built!

