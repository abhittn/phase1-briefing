# OTT App: Watchlist Feature – AI Task

## 1. Feature Breakdown

We will split the "Watchlist Feature" into 6 discrete AI tasks, ordered as we would actually build them:

1. **Redux Slice Setup**

   - Create a `watchlistSlice.js` using Redux Toolkit.
   - Define state: `{ items: [] }`.
   - Include actions: `addToWatchlist`, `removeFromWatchlist`, `loadWatchlistFromStorage`.

2. **API Integration / Persistence**

   - Implement API calls to persist watchlist per logged-in user.
   - Actions: `fetchWatchlist(userId)`, `saveWatchlist(userId, items)`.

3. **Content Card Watchlist Indicator**

   - Update `ContentCard` component to display a "Watchlist" icon if the item is saved.
   - Handle click to toggle watchlist state.

4. **Watchlist Screen UI**

   - Create a dedicated `WatchlistScreen.jsx` page.
   - Display all saved items using `ContentCard` components.
   - Include "Remove" functionality per item.

5. **Local Storage Fallback**

   - Implement saving/retrieving watchlist from `localStorage` for unauthenticated users.
   - Sync with Redux state on app load.

6. **Unit & Integration Tests**
   - Write tests for Redux slice actions and reducer.
   - Test `WatchlistScreen` and `ContentCard` interactions.

---

## 2. Two Full Prompts (CDIR)

### Prompt 1: Redux Slice Setup

**Context:**  
We are building a "Watchlist Feature" for an OTT app in React using Redux Toolkit.  
The watchlist should store content items saved by a logged-in user. We have an existing `store.js` setup with Redux Toolkit.

**Decomposition:**

1. Create `src/store/slices/watchlistSlice.js`.
2. Initial state: `{ items: [] }`.
3. Actions: `addToWatchlist(item)`, `removeFromWatchlist(itemId)`, `loadWatchlistFromStorage(items)`.
4. Reducer logic:
   - `addToWatchlist` should append item if not already present.
   - `removeFromWatchlist` should remove by `itemId`.
   - `loadWatchlistFromStorage` should replace state.items with provided array.
5. Export actions and reducer.

**Instructions:**

- Use Redux Toolkit's `createSlice`.
- Include JS Doc comments for each action.
- Save the file at `src/store/slices/watchlistSlice.js`.

---

### Prompt 2: Content Card Watchlist Indicator

**Context:**  
In the OTT app, `ContentCard.jsx` displays each title.  
We want to add a "Watchlist" indicator icon that shows if the item is already in the watchlist.  
Clicking the icon toggles the watchlist state using Redux actions.

**Decomposition:**

1. Import `useSelector` and `useDispatch` from React Redux.
2. Import `addToWatchlist` and `removeFromWatchlist` actions.
3. Add a watchlist icon in the card header.
4. On click:
   - If item is in watchlist, dispatch `removeFromWatchlist(item.id)`.
   - Else, dispatch `addToWatchlist(item)`.
5. Icon should visually change state .
6. Ensure component re-renders on Redux state change.

**Instructions:**

- Use Material UI's `Bookmark` and `BookmarkBorder` icons.
- Maintain accessibility with `aria-label`.
- Save changes in `src/components/ContentCard.jsx`.

---

## 3. Plan Mode Outline

**Expectations for Plan Mode:**

**Questions Agent should ask:**

- Should we paginate or lazy-load if watchlist is long?
- Are we reusing `ContentCard` from main feed?
- How should empty states be handled ?
- What rollback strategy is required?
- How to handle duplicate add requests?

**Files to reference:**

- `src/components/ContentCard.jsx`
- `src/store/slices/watchlistSlice.js`
- `src/routes/AppRoutes.jsx`

**Steps in Plan Mode:**

1. Import Redux state with `useSelector` for `items`.
2. Map over `items` to render `ContentCard` components.
3. Add "Remove" button per card, dispatching `removeFromWatchlist`.
4. Handle empty state with a message like "No items in Watchlist".
5. Ensure route `/watchlist` is registered in `AppRoutes.jsx`.

---

## 4. .cursor/rules Additions

1. **Rule:** `Watchlist actions must not mutate state directly`

   - Reason: Enforce Redux Toolkit immutability patterns.

2. **Rule:** `ContentCard icon must toggle correctly on state change`

   - Reason: Prevent UI from displaying incorrect watchlist state.

3. **Rule:** `Persist watchlist to localStorage on every change`
   - Reason: Ensure unauthenticated users have consistent experience.

---

## 5. AI Failure Anticipation

1. **Wrong Redux logic:** AI might allow duplicates in the watchlist.

   - **Check:** Review reducer logic, test with duplicate items.
   - **Fix:** Update prompt to explicitly prevent duplicates with `findIndex`.

2. **Incorrect icon behavior:** AI might not re-render on Redux changes.

   - **Check:** Interact with `ContentCard` after adding/removing items.
   - **Fix:** Add instructions to use `useSelector` to trigger re-render.

3. **Incorrect icon behavior:** UI updates but backend fails

   - **Check:** Simulate API failure
   - **Fix:** "Add rollback by storing previous state and restoring it on rejected action."

---

## 6. One Thing Learned

AI-assisted development requires **precise context and decomposition**. I realized that specifying **file paths, component names, and exact Redux patterns** dramatically increases output correctness. Without it, AI tends to hallucinate generic code that breaks project structure.
