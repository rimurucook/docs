# Game to Earn — Arcade

Noelclaw includes a built-in arcade with 4 playable games. Playing earns credits that can be used across the platform.

---

## Games

### 1. Nightfall
**Genre:** Arcade shooter
**Engine:** Custom canvas renderer

- Wave-based enemy shooter
- Survive as long as possible
- Credits based on level reached
- Score tracked: `score`, `kills`, `levelReached`

### 2. Uppy
**Genre:** Platformer
**Engine:** Custom canvas renderer

- Climb as high as possible
- Platform-jumping mechanic
- Credits based on height/level reached

### 3. Taevaria
**Genre:** Top-down action RPG
**Engine:** Custom canvas renderer

- Explore map, fight enemies, collect resources
- Character leveling system
- Combat with different enemy types
- Credits based on: `kills × level`
- Most depth of the 4 games

### 4. PokemonAutoChess
**Genre:** Auto-battler strategy
**Engine:** Phaser 3.90 (full physics engine)

- Place Pokemon units on a board
- Auto-battle rounds
- Synergy bonuses between unit types
- Most complex game in terms of setup

---

## Credits Formula

```
creditsEarned = Math.floor(levelReached × 100)
```

Examples:
| Level Reached | Credits Earned |
|--------------|----------------|
| 1 | 100 |
| 5 | 500 |
| 10 | 1,000 |
| 25 | 2,500 |

Credits are recorded per game session and aggregate in the user's `gameProfile`.

---

## Score Submission Flow

```
Player finishes game
      │
      ▼
Score calculated in frontend
(score, kills, levelReached, time, creditsEarned)
      │
      ▼
Convex action: api.gameActions.submitScore
      │
      ▼
Saved to gameScores table
      │
      ▼
gameProfile updated:
  totalGames++
  totalScore += score
  totalKills += kills
  totalCredits += creditsEarned
  bestNightfall / bestUppy / bestTaevaria / bestPac updated if new best
      │
      ▼
Activity feed entry created
      │
      ▼
User sees updated credits + profile stats
```

---

## Database Schema

```
gameScores:
  userId        — player
  gameType      — "nightfall" | "uppy" | "taevaria" | "pokemonAutoChess"
  score         — raw score
  kills         — enemies killed (optional)
  levelReached  — highest level (optional)
  time          — time survived in ms (optional)
  creditsEarned — credits from this session
  timestamp     — when submitted
  Indexes: by_user, by_game, by_user_game

gameProfiles:
  userId        — player
  username      — display name
  totalGames    — all-time game count
  totalScore    — all-time score sum
  totalKills    — all-time kills
  totalCredits  — total credits ever earned
  bestNightfall — best score in Nightfall
  bestUppy      — best score in Uppy
  bestTaevaria  — best score in Taevaria
  bestPac       — best score in PokemonAutoChess
  updatedAt
  Index: by_user
```

---

## Leaderboard

Scores are indexed by `gameType` — querying the leaderboard for any game:

```typescript
// Top 10 for Nightfall
ctx.db.query("gameScores")
  .withIndex("by_game", q => q.eq("gameType", "nightfall"))
  .order("desc")
  .take(10)
```

---

## Credits Usage

Credits earned from games can be used for:
- Running AI agents (deducted per action)
- Accessing premium features
- Token-based agent access

Credits are stored in the `transactions` table as balance — same system as USDC deposits.

---

## Arcade Page

The **Arcade** page (`/arcade`) shows:
- 4 game cards with game type, description
- "Play" button launches the game (full-screen canvas)
- After game over: score summary + credits earned popup
- Navigation back to arcade hub
