# **React.FC<>**

- **`React.FC`** stands for **"React Function Component"**
- It's a TypeScript type that tells TypeScript this is a React function component
- The `<>` part is for **generic types** - you put the props interface inside it

**Example:**
```typescript
// This tells TypeScript: "This is a React component that accepts PasswordModalProps"
React.FC<PasswordModalProps>
```

---

## 1. **What does this function declaration do?**

```typescript
const PasswordModal: React.FC<PasswordModalProps> = ({
    isOpen,
    onClose,
    onPasswordChoice,
    showPasswordInput,
    password,
    setPassword,
    onSubmitPassword
}) => {
```

This is a **function component declaration** that:
- Creates a React component called `PasswordModal`
- Accepts props (parameters) defined in `PasswordModalProps`
- Uses **destructuring** to extract specific props from the props object

---

## 2. **What is destructuring?**

Instead of writing:
```typescript
const PasswordModal = (props) => {
    const isOpen = props.isOpen;
    const onClose = props.onClose;
    // ... etc
}
```

We can write:
```typescript
const PasswordModal = ({ isOpen, onClose, ... }) => {
    // Now you can use isOpen, onClose directly
}
```

This extracts properties from an object.

---

## 3. **How to render the modal?**

```typescript
if (!isOpen) return null;
```

- **`isOpen`** is a boolean prop that controls whether the modal should be visible
- **`!isOpen`** means "if isOpen is false"
- **`return null;`** means "don't render anything" (hide the modal)

// If isOpen is false, the component returns null (nothing shows)
// If isOpen is true, the component renders the modal

---

## 4. **Why do we need this check?**

-  If the modal is closed, we don't want to render all the modal HTML
-  The modal should only appear when `isOpen` is true
-  Return `null` when you don't want to render anything

---

## 5. **Example**

```typescript
// This is the props interface (defined elsewhere)
interface PasswordModalProps {
    isOpen: boolean;
    onClose: () => void;
    // ... other props
}

// This is the component
const PasswordModal: React.FC<PasswordModalProps> = ({ isOpen, onClose }) => {
    // If modal is closed, don't render anything
    if (!isOpen) return null;
    
    // If modal is open, render the modal
    return (
        <div className="modal">
            <h2>Password Modal</h2>
            <button onClick={onClose}>Close</button>
        </div>
    );
};
```

---
