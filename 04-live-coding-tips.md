# Live Coding Interview Tips

> How to succeed in your 60-minute technical interview

---

## The Interview Format

**Duration:** 60 minutes
**What they're testing:**
- Problem-solving approach
- Coding style
- Communication skills
- How you handle pressure

**They're NOT expecting:**
- Perfect code on first try
- Memorized solutions
- Expert-level knowledge

---

## The Golden Rules

### 1. THINK OUT LOUD

This is the most important thing!

```
❌ Bad: *silence for 5 minutes* "Done!"

✅ Good: "Okay, so I need to... let me think about the data structure first. 
         I think an array would work here because... Actually, wait, 
         a Set might be better for lookups. Let me try that..."
```

**Why?** They want to see HOW you think, not just the final answer.

### 2. ASK CLARIFYING QUESTIONS

Before you code, ask questions!

```
Good questions to ask:
- "What should happen if the input is empty?"
- "Should this work for very large lists?"
- "Can there be duplicate items?"
- "Should I handle errors, or assume valid input?"
- "Is there a specific time/space complexity you're looking for?"
```

### 3. START WITH A PLAN

Don't jump into coding immediately.

```
1. Understand the problem (ask questions)
2. Think of examples (edge cases)
3. Explain your approach
4. Write pseudocode or outline
5. Code it
6. Test with examples
7. Discuss improvements
```

### 4. IT'S OKAY TO NOT KNOW

```
❌ Bad: Pretending you know something you don't

✅ Good: "I'm not sure about that specific API, but I would approach it by..."
         "I haven't used that before, but based on what I know about similar tools..."
         "Can I look that up, or would you prefer I describe my approach?"
```

---

## Time Management (60 minutes)

### Suggested Breakdown

| Time | Activity |
|------|----------|
| 0-5 min | Introductions, problem explanation |
| 5-10 min | Ask questions, understand requirements |
| 10-15 min | Plan your approach, discuss with interviewer |
| 15-45 min | Code the solution |
| 45-55 min | Test, debug, optimize |
| 55-60 min | Discuss improvements, ask questions |

### If You're Running Out of Time

```
"I'm running low on time, so let me explain what I would do next:
1. I'd add error handling here
2. I'd optimize this loop
3. I'd add tests for edge cases

Should I implement any of these, or would you like to discuss them?"
```

---

## Problem-Solving Framework

### Step 1: Understand

```
Interviewer: "Build a function that finds duplicates in an array"

You: "Let me make sure I understand:
- Input: an array of... numbers? strings? any type?
- Output: the duplicate values? or true/false if duplicates exist?
- Should I return all duplicates or just the first one?
- What about empty arrays?"
```

### Step 2: Examples

```
"Let me think of some examples:
- [1, 2, 3, 2] → should return [2] or 2?
- [1, 1, 1] → should return [1] or [1, 1]?
- [] → empty array, return []?
- [1, 2, 3] → no duplicates, return []?"
```

### Step 3: Approach

```
"I can think of a few approaches:

1. Brute force: nested loops, O(n²) time
2. Sort first: O(n log n) time, O(1) space
3. Use a Set: O(n) time, O(n) space

I'll go with the Set approach since it's most efficient for time.
Does that sound good?"
```

### Step 4: Code

```typescript
function findDuplicates(arr: number[]): number[] {
  const seen = new Set<number>();
  const duplicates = new Set<number>();
  
  for (const num of arr) {
    if (seen.has(num)) {
      duplicates.add(num);
    } else {
      seen.add(num);
    }
  }
  
  return Array.from(duplicates);
}
```

### Step 5: Test

```
"Let me trace through with [1, 2, 3, 2]:
- num=1: seen={}, not in seen, add to seen → seen={1}
- num=2: seen={1}, not in seen, add to seen → seen={1,2}
- num=3: seen={1,2}, not in seen, add to seen → seen={1,2,3}
- num=2: seen={1,2,3}, IS in seen, add to duplicates → duplicates={2}
- Return [2] ✓"
```

### Step 6: Discuss

```
"This solution is O(n) time and O(n) space.
If memory was a concern, I could sort first and compare adjacent elements.
I could also add input validation for null/undefined arrays."
```

---

## Common Patterns to Know

### 1. Array Operations

```typescript
// Filter
const completed = tasks.filter(t => t.completed);

// Map (transform)
const names = users.map(u => u.name);

// Find
const task = tasks.find(t => t.id === targetId);

// Some/Every
const hasCompleted = tasks.some(t => t.completed);
const allCompleted = tasks.every(t => t.completed);

// Reduce
const total = numbers.reduce((sum, n) => sum + n, 0);
```

### 2. Object Operations

```typescript
// Spread (copy and modify)
const updated = { ...user, name: 'New Name' };

// Destructuring
const { name, email } = user;

// Object.keys/values/entries
Object.keys(obj);    // ['name', 'email']
Object.values(obj);  // ['Mohamed', 'test@test.com']
Object.entries(obj); // [['name', 'Mohamed'], ['email', 'test@test.com']]
```

### 3. String Operations

```typescript
// Common methods
str.trim();           // Remove whitespace
str.toLowerCase();    // Lowercase
str.split(',');       // Split into array
str.includes('test'); // Check if contains
str.startsWith('Hi'); // Check start
```

### 4. Set and Map

```typescript
// Set - unique values
const unique = new Set([1, 2, 2, 3]); // {1, 2, 3}
unique.has(2);  // true
unique.add(4);  // {1, 2, 3, 4}

// Map - key-value pairs
const map = new Map();
map.set('key', 'value');
map.get('key');  // 'value'
map.has('key');  // true
```

---

## React Native Specific Questions

### State Management

```typescript
// Adding to array
setItems(prev => [...prev, newItem]);

// Removing from array
setItems(prev => prev.filter(item => item.id !== targetId));

// Updating item in array
setItems(prev => prev.map(item => 
  item.id === targetId ? { ...item, completed: true } : item
));
```

### Handling Async Operations

```typescript
const [loading, setLoading] = useState(false);
const [error, setError] = useState<string | null>(null);
const [data, setData] = useState<Data | null>(null);

const fetchData = async () => {
  setLoading(true);
  setError(null);
  
  try {
    const response = await fetch(url);
    const json = await response.json();
    setData(json);
  } catch (err) {
    setError('Failed to fetch data');
  } finally {
    setLoading(false);
  }
};

useEffect(() => {
  fetchData();
}, []);
```

### Component Structure

```typescript
interface Props {
  title: string;
  onPress: () => void;
}

export function MyComponent({ title, onPress }: Props) {
  const [count, setCount] = useState(0);
  
  const handlePress = () => {
    setCount(c => c + 1);
    onPress();
  };
  
  return (
    <Pressable onPress={handlePress}>
      <Text>{title}: {count}</Text>
    </Pressable>
  );
}
```

---

## When You Get Stuck

### Strategy 1: Break It Down

```
"Let me break this into smaller parts:
1. First, I need to parse the input
2. Then, process each item
3. Finally, format the output

Let me start with step 1..."
```

### Strategy 2: Start Simple

```
"Let me start with a simple solution that works, 
then optimize if we have time."
```

### Strategy 3: Ask for Hints

```
"I'm thinking about using [approach], but I'm not sure if that's 
the right direction. Could you give me a hint?"
```

### Strategy 4: Use Examples

```
"Let me trace through a simple example to understand the pattern..."
```

---

## What NOT to Do

### ❌ Don't Stay Silent
Even if thinking, say "Let me think about this for a moment..."

### ❌ Don't Give Up
If stuck, explain what you've tried and ask for guidance

### ❌ Don't Memorize Solutions
They can tell. Focus on understanding patterns instead.

### ❌ Don't Argue
If they suggest a different approach, be open to it

### ❌ Don't Rush
Take time to understand before coding

### ❌ Don't Forget Edge Cases
Empty input, null, single item, duplicates, etc.

---

## Questions to Ask Them

At the end, ask thoughtful questions:

```
About the role:
- "What does a typical day look like for this role?"
- "What are the biggest challenges the team is facing?"
- "How do you handle code reviews?"

About the tech:
- "What's your testing strategy for mobile apps?"
- "How do you handle app releases and updates?"
- "What's the split between new features and maintenance?"

About growth:
- "How do you support junior developers' growth?"
- "What learning resources does the team use?"
```

---

## Day Before Checklist

- [ ] Review your project code (be ready to explain any line)
- [ ] Practice explaining your approach out loud
- [ ] Get good sleep
- [ ] Test your computer/internet/camera/mic
- [ ] Have water nearby
- [ ] Find a quiet place

## During Interview Checklist

- [ ] Take a breath before answering
- [ ] Ask clarifying questions
- [ ] Think out loud
- [ ] Start with a plan
- [ ] Test your solution
- [ ] Discuss trade-offs
- [ ] Ask questions at the end

---

## Sample Practice Problems

Try these before your interview:

1. **Easy:** Reverse a string
2. **Easy:** Find the maximum number in an array
3. **Medium:** Remove duplicates from an array
4. **Medium:** Implement a simple todo list (add, remove, toggle)
5. **Medium:** Debounce a function
6. **React:** Build a counter with increment/decrement
7. **React:** Build a search filter for a list

---

**Next:** Read `05-common-interview-questions.md` for specific questions they might ask!

