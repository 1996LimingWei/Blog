## Work Flow to add a new feature

Start with a a short todo list and start with the backend events, then the frontend UI.

Before we begin: Understand what component we need to modify/add. Is it purely frontend/backend update or both? Streamline the workflow for this new feature.

Step 1 — Define queue types and socket events (backend)

- File: `server/src/types/index.ts` (or `server/src/types/room.ts`, wherever room state lives)
  - Add minimal queue contracts:
```ts
export type TrackSummary = {
  id: string;
  name: string;
  artists: string;
  durationMs: number;
  requestedBy: string;
};

export type RoomQueueState = {
  queue: TrackSummary[];
  currentTrackId?: string;
};
```

- File: `server/src/sockets.ts`
  - Ensure the room state includes a queue (if you already have room state, extend it):
```ts
// room.queue: TrackSummary[]
// room.currentTrackId?: string
```
  - Add handlers with host checks:
```ts
// queue:clear
socket.on('queue:clear', ({ roomId }) => {
  const room = rooms.get(roomId);
  if (!room || !socketIsHost(socket, room)) return;
  room.queue = [];
  io.to(roomId).emit('queue:update', { roomId, queue: [], currentTrackId: room.currentTrackId });
});

// queue:delete
socket.on('queue:delete', ({ roomId, trackId }) => {
  const room = rooms.get(roomId);
  if (!room || !socketIsHost(socket, room)) return;
  room.queue = room.queue.filter(t => t.id !== trackId);
  if (room.currentTrackId === trackId) room.currentTrackId = undefined;
  io.to(roomId).emit('queue:update', { roomId, queue: room.queue, currentTrackId: room.currentTrackId });
});

// queue:reorder
socket.on('queue:reorder', ({ roomId, newOrderIds }) => {
  const room = rooms.get(roomId);
  if (!room || !socketIsHost(socket, room)) return;
  const idToTrack = new Map(room.queue.map(t => [t.id, t]));
  room.queue = newOrderIds.map(id => idToTrack.get(id)).filter(Boolean) as TrackSummary[];
  io.to(roomId).emit('queue:update', { roomId, queue: room.queue, currentTrackId: room.currentTrackId });
});

// nowPlaying:update (if not present)
socket.on('nowPlaying:update', ({ roomId, currentTrackId }) => {
  const room = rooms.get(roomId);
  if (!room || !socketIsHost(socket, room)) return;
  room.currentTrackId = currentTrackId;
  io.to(roomId).emit('nowPlaying:update', { roomId, currentTrackId });
});
```
  - Note: Replace `rooms` and `socketIsHost` with your actual room store and host check.

Step 2 — Subscribe and hold queue state (frontend)

- File: `client/trip-frontend/src/hooks/useSocket.ts` (or wherever you centralize socket)
  - Subscribe to updates and expose `queue` and `currentTrackId`:
```ts
// inside effect after connecting
socket.on('queue:update', ({ queue, currentTrackId }) => {
  setQueue(queue);
  setCurrentTrackId(currentTrackId);
});

socket.on('nowPlaying:update', ({ currentTrackId }) => {
  setCurrentTrackId(currentTrackId);
});
```
  - Expose methods to emit:
```ts
const clearQueue = (roomId: string) => socket.emit('queue:clear', { roomId });
const deleteFromQueue = (roomId: string, trackId: string) => socket.emit('queue:delete', { roomId, trackId });
const reorderQueue = (roomId: string, newOrderIds: string[]) => socket.emit('queue:reorder', { roomId, newOrderIds });
```

Step 3 — UI: host-only controls, delete/clear, highlight current

- File: `client/trip-frontend/src/components/CurrentSongQueue.tsx`
  - Render the list based on `queue` prop.
  - Highlight current:
```tsx
<li className={track.id === currentTrackId ? "bg-blue-50" : ""}>
```
  - Host-only controls:
```tsx
{isHost && (
  <>
    <button onClick={() => onDelete(track.id)}>Delete</button>
    {/* drag handle UI will be added next step */}
  </>
)}
```
  - Clear button (host-only) at header:
```tsx
{isHost && <button onClick={onClear}>Clear</button>}
```
  - Component props you’ll need:
```ts
type Props = {
  queue: TrackSummary[];
  currentTrackId?: string;
  isHost: boolean;
  roomId: string;
  onClear: () => void;
  onDelete: (trackId: string) => void;
  onReorder: (newOrderIds: string[]) => void;
};
```

Step 4 — Wire Room to queue UI

- File: `client/trip-frontend/src/pages/Room.tsx`
  - Read `queue`, `currentTrackId` from your socket hook/state.
  - Pass host-only handlers:
```tsx
<CurrentSongQueue
  queue={queue}
  currentTrackId={currentTrackId}
  isHost={isHost}
  roomId={roomId}
  onClear={() => clearQueue(roomId)}
  onDelete={(trackId) => deleteFromQueue(roomId, trackId)}
  onReorder={(ids) => reorderQueue(roomId, ids)}
/>
```

Step 5 — Drag-and-drop reorder

- Still in `CurrentSongQueue.tsx`:
  - Use any library (dnd-kit recommended) or the simplest possible approach:
    - Maintain a local ordered list, apply reordering on drag end, call `onReorder(newOrderIds)`.
    - Optionally do optimistic update (apply local reorder immediately), and reconcile on the next `queue:update`.

That’s the logical flow. If you paste your `server/src/sockets.ts` room state/host check, I’ll tailor the exact snippets to your code.

- Summary of what we did:
  - Backend: added queue contracts and host-gated socket events for clear, delete, reorder, nowPlaying.
  - Frontend: subscribed to updates; added host-only actions, highlight current, and stubs for drag-and-drop reorder.
