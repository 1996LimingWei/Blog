## Work Flow to add a new feature

Start with a a short todo list and start with the backend events, then the frontend UI.

Before we begin: Understand what component we need to modify/add. Is it purely frontend/backend update or both? Streamline the workflow for this new feature.

A sample feature update: "Highlight the current song playing in the list" feature. Let's first examine the current code structure to understand how the queue and playback are managed.

Read client/trip-frontend/src/components/CurrentSongQueue.tsx, we know that the song queue is rendered in this component. 

Next, a component must be used in the frontend UI. By searching CurrentSongQueue in search bar, we can find: client/trip-frontend/src/pages/Room.tsx is using this to render the party room.

Now we know how songs queue is rendered in room. But to highlight the current song, shouldn't we know how current song is playing/handling in the project? Yes, by examining the project, we can find that client/trip-frontend/src/components/Audio/MainAudioPlayer.tsx is used to call server and download song, and tracks current song playing.

From above audioplayer, we can also see it imports a "AudioPlayerProps" which defines the prop of the player at client/trip-frontend/src/types/index.ts, let's also take a look at that(maybe we need to add a new type to track when the current song change, which helps us change the highlight position, but)

Now we have the big picture, let's draw the logical flow:

1. **Track Current Song Index**: The `MainAudioPlayer` already tracks `currentIndex` - this is our source of truth
2. **Pass Current Index Down**: Pass `currentIndex` from `MainAudioPlayer` to `Room` component, then to `CurrentSongQueue`
3. **Highlight Logic**: In `CurrentSongQueue`, compare each song's index with `currentIndex` and apply highlighting styles
4. **Real-time Updates**: When songs change (next/previous), the highlight updates automatically


Next we should Implementate the code change following the logical flow

### **Step-by-Step Implementation:**

1. **Added Callback in MainAudioPlayer** (`MainAudioPlayer.tsx`):
   - Added `onCurrentSongChange` prop to notify parent when current song index changes
   - Added `useEffect` to call the callback whenever `currentIndex` changes

2. **Added State Management in Room** (`Room.tsx`):
   - Added `currentSongIndex` state to track which song is currently playing
   - Passed `onCurrentSongChange={setCurrentSongIndex}` to `MainAudioPlayer`
   - Passed `currentSongIndex={currentSongIndex}` to `CurrentSongQueue`

3. **Enhanced CurrentSongQueue Component** (`CurrentSongQueue.tsx`):
   - Added `currentSongIndex` prop to the component interface
   - Implemented highlighting logic: current song gets blue background with white text
   - Added "Now Playing" indicator with play icon for the current song
   - Applied smooth transitions for visual feedback

### **How It Works:**

- **Real-time Updates**: When a song changes (next/previous), `MainAudioPlayer` calls `onCurrentSongChange`
- **State Propagation**: The new index flows down from `Room` → `CurrentSongQueue`
- **Visual Highlighting**: The current song gets:
  - Blue background (`bg-blue-500`)
  - White text (`text-white`)
  - Bold font (`font-semibold`)
  - Shadow effect (`shadow-md`)
  - "Now Playing" indicator with play icon
