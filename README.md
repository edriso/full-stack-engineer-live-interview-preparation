# Interview Preparation Docs 📚

> Your complete guide to ace the Chapter One live coding interview!

---

## Quick Start: What to Read First

**If you have limited time, read in this order:**

1. ⭐ **[03-your-project-explained.md](./03-your-project-explained.md)** - Know YOUR code inside out
2. ⭐ **[04-live-coding-tips.md](./04-live-coding-tips.md)** - How to succeed in the interview
3. **[05-common-interview-questions.md](./05-common-interview-questions.md)** - Practice answers

**If you have more time, also read:**

4. **[01-react-native-basics.md](./01-react-native-basics.md)** - React Native fundamentals
5. **[02-typescript-essentials.md](./02-typescript-essentials.md)** - TypeScript basics
6. **[06-project-maintenance.md](./06-project-maintenance.md)** - How to update your project

---

## Document Overview

| File | What It Covers | Time to Read |
|------|---------------|--------------|
| [01-react-native-basics.md](./01-react-native-basics.md) | Components, styling, hooks, Expo Router | 20 min |
| [02-typescript-essentials.md](./02-typescript-essentials.md) | Types, interfaces, generics, common patterns | 15 min |
| [03-your-project-explained.md](./03-your-project-explained.md) | Your Do Meows app code breakdown | 15 min |
| [04-live-coding-tips.md](./04-live-coding-tips.md) | Interview strategy and problem-solving | 15 min |
| [05-common-interview-questions.md](./05-common-interview-questions.md) | Questions and sample answers | 20 min |
| [06-project-maintenance.md](./06-project-maintenance.md) | How to run, update, and improve the app | 10 min |

---

## The Interview: What to Expect

**Format:** 60-minute live coding session
**What they test:** Problem-solving approach, coding style, communication

### Key Things to Remember

1. **Think out loud** - They want to see HOW you think
2. **Ask questions** - Clarify requirements before coding
3. **Start simple** - Get something working, then improve
4. **Test your code** - Walk through examples
5. **Be honest** - If you don't know something, say so

---

## Your 30-Second Project Pitch

> "I built 'Do Meows', a task manager using React Native and Expo. It has add, complete, and delete features with cat GIF animations for feedback. I used TypeScript for type safety, created a theme system supporting light/dark mode, and used useRef for the feedback timer to avoid memory leaks. If I had more time, I'd add data persistence and use FlatList for better performance."

---

## Day Before Checklist

- [ ] Re-read your project code (`app/(tabs)/index.tsx`)
- [ ] Practice explaining your code out loud
- [ ] Review the live coding tips
- [ ] Test your computer/camera/mic
- [ ] Get good sleep!

## Interview Day Checklist

- [ ] Have water nearby
- [ ] Find a quiet place
- [ ] Take a breath before answering
- [ ] Think out loud while coding
- [ ] Ask clarifying questions
- [ ] Test your solution with examples

---

## Quick Reference: Your Tech Stack

| Technology | What It Does |
|------------|--------------|
| **React Native** | Build mobile apps with JavaScript |
| **Expo** | Tools to make React Native easier |
| **TypeScript** | JavaScript with types (catches bugs early) |
| **Expo Router** | File-based navigation |
| **expo-image** | Better image/GIF handling |
| **nanoid** | Generate unique IDs |

---

## Emergency Cheat Sheet

### State Updates

```typescript
// Add to array
setItems(prev => [...prev, newItem]);

// Remove from array
setItems(prev => prev.filter(item => item.id !== targetId));

// Update item in array
setItems(prev => prev.map(item => 
  item.id === targetId ? { ...item, done: true } : item
));
```

### Common Hooks

```typescript
// State
const [value, setValue] = useState(initialValue);

// Effect (run on mount)
useEffect(() => { /* code */ }, []);

// Effect (run when dependency changes)
useEffect(() => { /* code */ }, [dependency]);

// Ref (no re-render)
const ref = useRef(null);
```

### TypeScript Basics

```typescript
// Types
let name: string = "Mohamed";
let age: number = 25;
let items: string[] = [];

// Interface
interface Task {
  id: string;
  text: string;
  completed: boolean;
}

// Function
function add(a: number, b: number): number {
  return a + b;
}
```

---

## Good Luck! 🚀

You've got this! Remember:
- They already liked your assessment - you're qualified!
- Be yourself and show enthusiasm
- It's okay to not know everything
- Ask questions and communicate clearly

---

*Created for Mohamed's Chapter One interview preparation*

