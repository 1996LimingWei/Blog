# User should optionally set a password when creating a room

### **Step 1: User Initiates Room Creation**
- User fills in username and room ID on the Welcome Page
- User clicks "Create Room" button
- Frontend triggers `handleJoinRoom('create')` function

### **Step 2: Password Modal Appears**
- Frontend shows password modal with question: "Would you like to set a password for your room?"
- Two options presented:
  - **"Yes, I want to set a password"** → Proceeds to password input
  - **"No, just take me to the beat"** → Creates room without password

### **Step 3A: User Chooses "No"**
- Frontend calls `createRoomWithPassword('')` 
- Room is created immediately without password protection
- User is taken directly to the room

### **Step 3B: User Chooses "Yes"**
- Modal updates to show password input field
- User enters desired password
- User clicks "Create Room" to proceed or "Cancel" to go back

---

# Workflow
---

## 1. **Backend**

### **A. Room Data Structure**
```typescript
// server/src/types/room.ts
export class RoomInfo {
    roomID: string;
    hostID: string;
    requiresPassword: boolean;
    password?: string; // Added for password storage
    private users: Map<string, User>;
    // ... other properties
}
```

### **B. Socket Event Handling**
```typescript
// server/src/sockets.ts
socket.on('joinRoom', ({roomId, username, action, password}) => {
    if (action === 'create') {
        if (roomExist(roomId)) {
            socket.emit('roomError', { message: 'Room ID already in use' });
            return;
        }
        
        // Create new room with password if provided
        rooms[roomId] = new RoomInfo(roomId);
        if (password) {
            rooms[roomId].requiresPassword = true;
            rooms[roomId].password = password;
        }
        
        // Add user as host
        addUserToRoom(socket.id, roomId, username);
    }
    // ... join room logic
});
```

---

## 2. **Frontend**

### **Key Components**

#### **A. Home.tsx (Main Controller)**
```typescript
// State Management
const [showPasswordModal, setShowPasswordModal] = useState(false);
const [showPasswordInput, setShowPasswordInput] = useState(false);
const [password, setPassword] = useState('');
const [passwordInput, setPasswordInput] = useState('');

// Flow Control Functions
const handleJoinRoom = (action: 'create' | 'join') => {
    if (action === 'create') {
        setShowPasswordModal(true); // Show password modal
        return;
    }
    // Handle join room logic...
};

const handlePasswordChoice = (wantsPassword: boolean) => {
    if (wantsPassword) {
        setShowPasswordInput(true); // Show password input
    } else {
        createRoomWithPassword(''); // Create room without password
    }
};

const createRoomWithPassword = (password?: string) => {
    // Send room creation request to backend
    socket.emit("joinRoom", {roomId, username: userName, action: 'create', password});
    // Reset modal state and proceed to room
};
```

#### **B. PasswordModal.tsx (UI Component)**
```typescript
// Two-step modal rendering
{!showPasswordInput ? (
    // Step 1: Yes/No choice
    <div>
        <button onClick={() => onPasswordChoice(true)}>Yes</button>
        <button onClick={() => onPasswordChoice(false)}>No</button>
    </div>
) : (
    // Step 2: Password input
    <div>
        <input type="password" placeholder="Enter password" />
        <button onClick={onSubmitPassword}>Create Room</button>
        <button onClick={onClose}>Cancel</button>
    </div>
)}
```

---

## 3. **Data Flow Diagram**

```
User clicks "Create Room"
         ↓
Frontend: handleJoinRoom('create')
         ↓
Frontend: setShowPasswordModal(true)
         ↓
Frontend: PasswordModal renders (Step 1)
         ↓
User chooses Yes/No
         ↓
If "No": createRoomWithPassword('')
If "Yes": setShowPasswordInput(true) → PasswordModal (Step 2)
         ↓
Frontend: socket.emit("joinRoom", {action: 'create', password})
         ↓
Backend: Socket event handler receives request
         ↓
Backend: Creates RoomInfo with password (if provided)
         ↓
Backend: Adds user as host
         ↓
Frontend: setUserJoined(true) → Navigate to Room
```

---

## 5. **State Management**

### **Frontend State Variables**
| Variable | Purpose | Initial Value |
|----------|---------|---------------|
| `showPasswordModal` | Controls modal visibility | `false` |
| `showPasswordInput` | Controls modal step (Yes/No vs Password input) | `false` |
| `password` | Stores password value | `''` |
| `passwordInput` | Stores password input field value | `''` |

### **Backend State Variables**
| Variable | Purpose | Location |
|----------|---------|----------|
| `rooms` | HashMap storing all active rooms | `sockets.ts` |
| `requiresPassword` | Boolean flag for room protection | `RoomInfo` class |
| `password` | Actual password string | `RoomInfo` class |

---
