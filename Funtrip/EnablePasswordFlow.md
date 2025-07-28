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

## **Summary Table**

| Step | File | What to Change |
|------|------|----------------|
| 1    | `sockets.ts` | Update `"joinRoom"` event handler to accept `password` |
| 2    | `sockets.ts` | Update `createAndJoinRoom` to accept and use `password` |
| 3    | `types/room.ts` | Update `RoomInfo` class to store `password` |

---
