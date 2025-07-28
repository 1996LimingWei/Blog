## React’s `useState` defines **two things**:

1. **A variable** that holds the current value of your state.
2. **A function** that lets you update that value.

---

## Example

```js
const [count, setCount] = useState(0);
```

- `count` is the **state variable** (starts at `0`)
- `setCount` is the **function to update** `count`

---

**Example:**
```js
console.log(count); // shows the current value

setCount(count + 1); // increases count by 1
```

When you call `setCount`, React will:
- Update the value of `count`
- Re-render your component with the new value

---
