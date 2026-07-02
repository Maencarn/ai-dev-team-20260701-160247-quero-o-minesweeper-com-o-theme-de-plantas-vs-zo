# Traceability Matrix

## Coverage Summary

- Total rows: 6
- Complete rows: 6
- Partial rows: 0
- Missing coverage rows: 0
- Coverage status: complete

## Matrix

| ID | Requirement | User Story | Component | Test Case | Artifact | Status |
| --- | --- | --- | --- | --- | --- | --- |
| TR-001 | Start Game & Select Difficulty | As a player, I want to select a difficulty level and start a new game so that I can play a match suited to my skill l... | • Zero backend, zero DevOps. | TC01: First Click Safety Verification | - | complete |
| TR-002 | Reveal Tiles with First-Click Safety | As a player, I want to click on a tile to reveal it, ensuring my first click is always safe, so that I can start the... | • React’s component model fits a tile grid perfectly and eases future enhancements (touch support, animations, dark m... | TC02: Flood-Fill Reveal on Empty Tile | - | complete |
| TR-003 | Automatic Reveal of Empty Areas | As a player, I want empty tiles with no adjacent zombies to automatically reveal their neighbors so that I do not hav... | ## Required components | TC03: Game Over on Mine Reveal | - | complete |
| TR-004 | Flag and Unflag Suspected Zombies | As a player, I want to flag tiles I suspect contain zombies so that I can prevent myself from accidentally clicking t... | • GameEngine module | TC04: Flagging and Unflagging Tiles | - | complete |
| TR-005 | Win Condition and Game Status Tracking | As a player, I want to see my game progress (timer, remaining zombies) and receive a clear victory message when I win... | • UI Components | TC05: Win Condition Validation | - | complete |
| TR-006 | Game State Persistence | As a player, I want my current game state to be saved automatically so that I can resume playing if I refresh the pag... | – Tile component (handles left / right / long press) | TC06: LocalStorage Session Recovery | - | complete |
