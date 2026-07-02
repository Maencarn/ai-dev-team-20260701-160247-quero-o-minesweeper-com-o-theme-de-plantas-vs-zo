## Technical proposal
Deliver a lightweight, web-only Minesweeper reskin that runs 100 % in the browser—no server round-trips, no database.  
A single-page application (SPA) built with React + TypeScript will:

1. Contain all classic Minesweeper logic inside the client (grid creation, flood-fill reveal, flagging, win/lose detection).
2. Swap Mines (💣) for PvZ zombies and safe tiles for plants through CSS classes and downloadable sprite sheets.
3. Persist only minimal session data (current game, chosen difficulty) in `localStorage`; no user accounts or highscores in v1.
4. Be shipped as a static bundle that can be hosted on GitHub Pages, Netlify, Vercel, or any CDN.

Why this option?  
• Fast time-to-value: a two-person team can reach MVP in < 2 weeks.  
• Zero backend, zero DevOps.  
• React’s component model fits a tile grid perfectly and eases future enhancements (touch support, animations, dark mode, etc.).  
• TypeScript gives compile-time safety for grid operations.

## Required components
• React SPA scaffold (Vite + React + TS)  
• GameEngine module  
  – Board generator (guaranteed first-click safe)  
  – Flood-fill reveal algorithm  
  – Win / lose evaluator  
• UI Components  
  – Header (timer, remaining zombies counter, reset button)  
  – DifficultySelector modal  
  – Tile component (handles left / right / long press)  
  – BoardGrid component  
  – EndGame overlay (win / lose)  
• Theming layer  
  – PvZ sprite sheet (plants, zombies, numerals, background)  
  – SCSS variables for spacing / colours  
• Asset loader (lazy-loads sprite sheet, shows fallback skeleton)  
• State management (React Context or Zustand—keep it tiny)  
• Simple persistence service (read/write to `localStorage`)  
• Unit tests (Jest + React Testing Library) for core engine  
• CI pipeline (GitHub Actions) – lint, test, build

## Suggested data model
(The entire model lives in browser memory or `localStorage`)

Tile  
```ts
interface Tile {
  x: number
  y: number
  isMine: boolean      // zombie
  isRevealed: boolean
  isFlagged: boolean
  adjacentMines: number // 0-8
}
```

Board  
```ts
interface Board {
  width: number
  height: number
  mineCount: number
  tiles: Tile[]        // length = width * height
}
```

GameSession  
```ts
interface GameSession {
  id: string           // uuid
  difficulty: 'beginner' | 'intermediate' | 'expert'
  board: Board
  startedAt: number
  elapsed: number      // seconds, updated via timer
  status: 'playing' | 'won' | 'lost'
}
```

Persisted shape in `localStorage`  
```json
{
  "currentSession": { ...GameSession }
}
```

No relational data is required; a flat JSON blob suffices.

## Suggested APIs/endpoints
External HTTP APIs: none (static hosting only).  
Internal service interfaces:

1. `GameEngine.newGame(difficulty): GameSession`  
2. `GameEngine.reveal(session: GameSession, x, y): GameSession`  
3. `GameEngine.toggleFlag(session: GameSession, x, y): GameSession`  
4. `Persistence.save(session)` / `Persistence.load()`  

These synchronous functions mutate/return plain objects, keeping the UI reactive and testable.

## Implementation plan
Phase 0 – Setup (0.5 week)  
• Scaffold Vite + React + TS project  
• ESLint + Prettier + Jest  
• Basic responsive grid layout

Phase 1 – Core gameplay (1 week)  
• Implement Board generator (with first-click safety)  
• Implement reveal + flood-fill + flag logic with unit tests  
• Wire BoardGrid and Tile components  
• MVP CSS (plain colours, no PvZ assets yet)

Phase 2 – PvZ theming (0.5 week)  
• Acquire/create sprite sheet (plants, zombies, numbers)  
• Replace colours with images via CSS classes  
• Add background, fonts, buttons consistent with PvZ

Phase 3 – UX polish & responsiveness (0.5 week)  
• Mobile touch gestures (tap = reveal, long-press = flag)  
• Timer, remaining zombies counter, restart button  
• DifficultySelector modal  
• Lightweight animations (CSS transitions on reveal)

Phase 4 – QA & deploy (0.5 week)  
• Cross-browser testing (Chrome, Firefox, Safari, Edge, mobile)  
• Lighthouse performance check (< 2 s load)  
• GitHub Actions build → deploy to chosen static host  
• Tag v1.0 release

Total effort: ~3 weeks for two developers/designer.

## Technical risks
1. Asset licensing: PvZ graphics are EA/PopCap IP. Mitigation: create custom “inspired” art or obtain explicit permission.  
2. First-click safety & flood-fill bugs can lock the board; comprehensive unit tests mitigate.  
3. Mobile long-press detection inconsistencies across browsers; fall back to flag-toggle button if needed.  
4. Large sprite sheet may slow first paint; mitigate with lazy-loading and SVG/PNG optimisation.  
5. Accessibility (colour-blind users) – ensure numbers also have distinct shapes or ARIA labels.

## Assumptions
• Web is the only target platform for v1.  
• No user accounts, highscores, or multiplayer features are required now.  
• Audio, additional PvZ mechanics, or in-game purchases are out of scope for v1.  
• Team has rights to use or recreate PvZ-style graphics.