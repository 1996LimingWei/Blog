# React’s `useState` defines **two things**:

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

# What is `useEffect`?

- `useEffect` lets us run **side effects** in our React component.
- A **side effect** is anything that affects something outside the component, or needs to happen after the component renders (like fetching data, setting up a timer, or subscribing to events).

---

## How is it different from `useState`?

- **`useState`** is for storing and updating values (state) in your component.
- **`useEffect`** is for running code in response to changes in state, props, or when the component mounts/unmounts.

---

## Example Explained

```js
useEffect(() => {
    async function checkHostStatus() {
        try {
            // Call the server to check if the user is host
            const response = await axios.get<{ isHost: boolean }>(`${serverURL}/room/${roomId}/isHost`, {
                params: {
                    roomId: roomId,
                    userName: currentUser
                }
            });
            setIsHost(response.data.isHost);
        } catch (error) {
            console.error("Error checking host status:", error);
            setIsHost(false); // fallback
        }
    }
    checkHostStatus();
}, [roomId, currentUser]);
```

### What does this do?

- The code inside `useEffect` runs **after the component renders**.
- It defines and calls `checkHostStatus`, which:
  - Makes a request to the server to check if the current user is the host.
  - Updates the `isHost` state with the result.
- The array `[roomId, currentUser]` is called the **dependency array**:
  - The effect runs **every time** `roomId` or `currentUser` changes.

---

## Why do we need `useEffect`?

- You can’t put async code or side effects directly in the main body of a React component.

---
