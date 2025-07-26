# Tower Climb - Game Design Document

## Game Concept
A minimalist 2D roguelike where players climb an endless tower, room by room, facing increasingly difficult challenges. Focus on tight gameplay loop and procedural generation.

## Visual Style
- **2D top-down or side view** (TBD based on gameplay feel)
- **Programmatic shapes** for most elements (rectangles, circles)
- **Simple sprites** for player and enemies
- **Clean, readable design** with high contrast

## Core Gameplay Loop
1. Enter room
2. Fight/avoid enemies
3. Collect loot/upgrades
4. Find stairs up
5. Ascend to next floor
6. Repeat until death

## MVP Features Only
- Single player character
- Basic movement (WASD or arrows)
- Simple combat (one attack type)
- Basic enemies (2-3 types)
- Procedural room generation
- Permadeath
- Score tracking

---

# Development Phases

## Phase 0: Core Systems (Week 1)
**Goal**: Get a window with a game loop running

### System 1: Window and Display
- **P0-S1-C1**: Window Creation (800x600, resizable)
- **P0-S1-C2**: Basic Rendering Pipeline
- **P0-S1-C3**: FPS Counter and Frame Limiting

### System 2: Game Loop and States
- **P0-S2-C1**: Main Game Loop
- **P0-S2-C2**: State Management (Menu, Playing, GameOver)
- **P0-S2-C3**: Basic Input Handling (Keyboard)

### System 3: Basic Rendering
- **P0-S3-C1**: Shape Drawing (rect, circle, line)
- **P0-S3-C2**: Sprite Loading and Drawing
- **P0-S3-C3**: Camera System (follow player)

**Deliverable**: Window that shows "Press SPACE to Start", transitions to game view

---

## Phase 1: Player and World (Week 2)
**Goal**: Player can move around procedurally generated rooms

### System 1: Player Entity
- **P1-S1-C1**: Player Class with Position
- **P1-S1-C2**: Player Movement (8-direction)
- **P1-S1-C3**: Player Collision with Walls
- **P1-S1-C4**: Player Sprite and Animation

### System 2: Room Generation
- **P1-S2-C1**: Room Data Structure
- **P1-S2-C2**: Basic Room Templates (5-6 layouts)
- **P1-S2-C3**: Wall and Floor Tile System
- **P1-S2-C4**: Door/Stair Placement

### System 3: Tower Structure
- **P1-S3-C1**: Floor Manager (track current floor)
- **P1-S3-C2**: Room Transitions (enter stairs)
- **P1-S3-C3**: Floor Difficulty Scaling

**Deliverable**: Player can explore randomly generated rooms and climb floors

---

## Phase 2: Combat System (Week 3)
**Goal**: Basic combat with enemies

### System 1: Combat Mechanics
- **P2-S1-C1**: Health System (Player + Enemies)
- **P2-S1-C2**: Basic Attack (melee swing)
- **P2-S1-C3**: Damage and Death
- **P2-S1-C4**: Attack Animation and Feedback

### System 2: Enemy System
- **P2-S2-C1**: Basic Enemy Class
- **P2-S2-C2**: Slime Enemy (moves toward player)
- **P2-S2-C3**: Archer Enemy (shoots projectiles)
- **P2-S2-C4**: Enemy Spawning in Rooms

### System 3: Combat UI
- **P2-S3-C1**: Health Bar Display
- **P2-S3-C2**: Damage Numbers
- **P2-S3-C3**: Death Screen with Score

**Deliverable**: Player can fight enemies, take damage, and die

---

## Phase 3: Game Feel and Polish (Week 4)
**Goal**: Make it feel good to play

### System 1: Items and Progression
- **P3-S1-C1**: Item System (health potions)
- **P3-S1-C2**: Basic Power-ups (damage+, speed+)
- **P3-S1-C3**: Score and Floor Counter
- **P3-S1-C4**: Simple Loot Drops

### System 2: Juice and Feedback
- **P3-S2-C1**: Screen Shake on Hit
- **P3-S2-C2**: Particle Effects (basic)
- **P3-S2-C3**: Sound Effects (using pygame)
- **P3-S2-C4**: Hit Flash and Knockback

### System 3: Persistence
- **P3-S3-C1**: High Score Saving
- **P3-S3-C2**: Settings (volume, controls)
- **P3-S3-C3**: Run Statistics

**Deliverable**: Complete MVP game loop with progression

---

## MVP Completion Checklist
- [ ] Player can start game from menu
- [ ] Player can move around rooms
- [ ] Rooms are procedurally generated
- [ ] Player can climb to higher floors
- [ ] Enemies spawn and attack
- [ ] Player can attack and defeat enemies
- [ ] Player can die and see score
- [ ] High scores are saved
- [ ] Game feels responsive and fun

---

## Post-MVP Features (Future)
Only after MVP is complete and playtested:

### Phase 4: Extended Content
- More enemy types
- Boss fights every 10 floors
- More item variety
- Character classes
- Mini-map

### Phase 5: Advanced Features
- Lighting system (if it adds to gameplay)
- Advanced particle effects
- Procedural music
- Daily challenges
- Steam integration

---

## Technical Decisions

### Core Technologies
- **Language**: Python 3.11+
- **Graphics**: Pygame 2.5+
- **Architecture**: ECS-lite (Entity-Component pattern)

### Art Assets Needed (Minimal)
- Player sprite (16x16 or 32x32)
- 2-3 enemy sprites
- Wall and floor tiles
- UI elements (health bar, buttons)
- Simple particle sprites

### Audio Assets Needed (Minimal)
- Hit sound
- Death sound
- Pickup sound
- Menu click
- Background music (1 track)

---

## Contract Estimation
- **Phase 0**: 8-10 contracts (2-3 days)
- **Phase 1**: 10-12 contracts (3-4 days)
- **Phase 2**: 10-12 contracts (3-4 days)
- **Phase 3**: 10-12 contracts (3-4 days)
- **Total MVP**: ~40 contracts (2-3 weeks)

## Success Metrics for MVP
- Game runs at 60 FPS on modest hardware
- Player can complete 10+ floors in a run
- Death feels fair (player mistake, not game issue)
- Core loop is addictive ("one more run")
- Code is clean and extensible for post-MVP features