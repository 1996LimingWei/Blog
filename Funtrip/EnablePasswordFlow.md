# User should optionally set a password when creating a room

## Workflow
---

## 1. **Backend**

1. **Frontend** emits:  
   ```js
   socket.emit('joinRoom', { roomId, username, action: 'create', password });
   ```
2. **Backend** receives the event, and calls:  
   ```js
   createAndJoinRoom(roomId, socket, password);
   ```
3. **RoomInfo** is created with the password:  
   ```js
   rooms[roomId] = new RoomInfo(roomId, password);
   ```
---

| Step | File | What to Change |
|------|------|----------------|
| 1    | `sockets.ts` | Update `"joinRoom"` event handler to accept `password` |
| 2    | `sockets.ts` | Update `createAndJoinRoom` to accept and use `password` |
| 3    | `types/room.ts` | Update `RoomInfo` class to store `password` |

---

## 2. **Frontend**

1. **User clicks "Create Room"** → Show password modal
2. **User chooses "Yes"** → Show password input
3. **User chooses "No"** → Skip password and create room
4. **Send room creation request** → Include password (if set)

---

#### Room Creation Component

**File:** `client/trip-frontend/src/pages/Home.tsx`

1. **Add state variables** for modal management
2. **Modify `handleJoinRoom`** to show password modal for room creation
3. **Add password handling functions**
4. **Create `PasswordModal.tsx`** component
5. **Add modal to JSX**

---

## **Visual Flow**

1. User types password: `roomPassword = "mypassword123"`
2. User clicks "Create Room": `createRoomWithPassword("mypassword123")`
3. Backend receives: `password: "mypassword123"` ✅
4. Frontend clears state: `roomPassword = ""` (for security/clean UI)

---


