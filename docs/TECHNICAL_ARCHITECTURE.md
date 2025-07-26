# Tower Climb - Technical Architecture

## Core Architecture Pattern: Simple ECS-lite

### Why ECS-lite?
- Flexible for game objects
- Easy to extend
- Good performance
- Avoids deep inheritance

### Basic Structure
```python
# Entity = ID + Components
class Entity:
    def __init__(self):
        self.id = generate_id()
        self.components = {}

# Component = Data only
@dataclass
class Position:
    x: float
    y: float

@dataclass  
class Shape:
    type: str  # "rect", "circle", "triangle"
    color: Tuple[int, int, int]
    size: Tuple[float, float]
    
# System = Logic that operates on components
class MovementSystem:
    def update(self, entities, dt):
        for entity in entities:
            if has_components(entity, Position, Velocity):
                # Update position based on velocity
```

---

## Game View: 2D Top-Down

### Coordinate System
- **Origin**: Top-left (0,0)
- **Unit**: Pixels
- **Room Size**: 800x600 pixels
- **Tile Size**: 32x32 pixels
- **View**: Direct top-down (no perspective)

### Window Configuration
- **Platform**: Windows only (initial target)
- **Default Size**: 800x600
- **Resizable**: Yes (maintain aspect ratio)
- **Fullscreen**: Optional

---

## Visual Design: Pure Programmatic

### No External Assets Required
Everything rendered using pygame drawing primitives with minimalist human designs:

```python
# Player (minimalist human - pill body with head)
# Body
body_rect = pygame.Rect(x - 5, y - 4, 10, 16)
pygame.draw.rect(screen, PLAYER_BLUE, body_rect)
pygame.draw.circle(screen, PLAYER_BLUE, (x, y - 4), 5)  # Top round
pygame.draw.circle(screen, PLAYER_BLUE, (x, y + 12), 5)  # Bottom round
# Head
pygame.draw.circle(screen, SKIN_TONE, (x, y - 12), 5)
# Eyes
pygame.draw.circle(screen, BLACK, (x - 2, y - 12), 1)
pygame.draw.circle(screen, BLACK, (x + 2, y - 12), 1)
# Weapon shows direction
weapon_end = (x + cos(angle) * 15, y + sin(angle) * 15)
pygame.draw.line(screen, WHITE, (x, y), weapon_end, 3)

# Slime Enemy (blob with eyes)
radius = 10 + sin(time) * 2
pygame.draw.circle(screen, ENEMY_GREEN, (x, y), radius)
pygame.draw.circle(screen, BLACK, (x - 3, y - 2), 2)  # Left eye
pygame.draw.circle(screen, BLACK, (x + 3, y - 2), 2)  # Right eye

# Archer Enemy (smaller human with bow)
# Mini pill body
body_rect = pygame.Rect(x - 4, y - 3, 8, 14)
pygame.draw.rect(screen, ENEMY_RED, body_rect)
pygame.draw.circle(screen, ENEMY_RED, (x, y - 3), 4)
pygame.draw.circle(screen, ENEMY_RED, (x, y + 11), 4)
# Head
pygame.draw.circle(screen, ENEMY_DARK_RED, (x, y - 10), 4)
# Bow (arc)
bow_rect = pygame.Rect(x - 10, y - 10, 20, 20)
pygame.draw.arc(screen, WHITE, bow_rect, aim_angle - 0.5, aim_angle + 0.5, 2)

# Walls (dark gray rectangles)
pygame.draw.rect(screen, DARK_GRAY, wall_rect)

# Floor (lighter gray with grid pattern)
pygame.draw.rect(screen, LIGHT_GRAY, floor_rect)
for x in range(0, width, 32):
    pygame.draw.line(screen, GRAY, (x, 0), (x, height), 1)

# Doors (brown rectangle outline)
pygame.draw.rect(screen, BROWN, door_rect, 3)

# Stairs Up (yellow triangle pointing up)
pygame.draw.polygon(screen, YELLOW, [(x, y-16), (x-16, y+16), (x+16, y+16)])

# Health Potion (red circle with cross)
pygame.draw.circle(screen, RED, (x, y), 8)
pygame.draw.rect(screen, WHITE, (x-6, y-2, 12, 4))
pygame.draw.rect(screen, WHITE, (x-2, y-6, 4, 12))

# Attack Swing (arc)
pygame.draw.arc(screen, WHITE, swing_rect, start_angle, end_angle, 3)
```

### Color Palette
```python
# Background
BLACK = (0, 0, 0)
DARK_GRAY = (32, 32, 32)

# Environment  
LIGHT_GRAY = (96, 96, 96)
GRAY = (64, 64, 64)
BROWN = (101, 67, 33)
YELLOW = (255, 215, 0)

# Characters
PLAYER_BLUE = (64, 128, 255)
SKIN_TONE = (255, 220, 177)
ENEMY_GREEN = (64, 255, 64)
ENEMY_DARK_GREEN = (32, 180, 32)
ENEMY_RED = (255, 64, 64)
ENEMY_DARK_RED = (180, 32, 32)

# UI
WHITE = (255, 255, 255)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
UI_GOLD = (255, 215, 0)

# Effects
DAMAGE_FLASH = (255, 100, 100)
HEAL_FLASH = (100, 255, 100)
```

---

## Game Object Specifications

### Player
- **Visual**: Minimalist human (pill body + head)
- **Size**: Body 10x24 pixels, head radius 5
- **Weapon**: White line that rotates with facing direction
- **Collision**: Rectangle 10x20 (slightly smaller than visual)
- **Movement**: 8-directional, 150 pixels/second

### Enemies
1. **Slime**
   - Visual: Green blob with eyes (radius 8-12, pulses)
   - Behavior: Moves toward player at 50 px/s
   - HP: 2
   - Collision: Circle radius 10

2. **Archer**
   - Visual: Smaller red human with bow (8x20 body)
   - Behavior: Shoots when player in line of sight
   - HP: 1
   - Projectile: Small white arrow (6x2)

### Environment
- **Wall**: Dark gray filled rectangle
- **Floor**: Light gray with subtle grid
- **Door**: Brown outline rectangle (open)
- **Stairs**: Yellow triangle pointing up

### Items
- **Health Potion**: Red circle with white cross
- **Power Up**: Colored diamond shape
- **Gold**: Small yellow circles

### UI Elements
- **Health Bar**: Red rectangle with white outline
- **Floor Counter**: White text "Floor: X"
- **Score**: Gold text with number
- **Damage Numbers**: Float up and fade

---

## Room Generation

### Room Size
- 25x19 tiles (800x608 pixels)
- Border walls on all edges
- 1-4 doors (cardinal directions)
- 1 stairs up per room

### Room Templates (Programmatic)
```python
# Empty room with walls
def generate_empty_room():
    # All edges are walls
    # Center is floor
    # Add random pillars

# Cross-shaped room
def generate_cross_room():
    # Walls in corners
    # Open cross pattern

# Arena room
def generate_arena_room():
    # Open center
    # Pillars in pattern
```

---

## Performance Considerations

### Windows-Specific
- Use `pygame.HWSURFACE | pygame.DOUBLEBUF`
- Handle Windows DPI scaling
- Test on Windows 10/11

### Rendering Optimization
- Draw static elements to background surface
- Only redraw changed areas when possible
- Batch similar draw calls

### Target Performance
- 60 FPS on mid-range Windows laptop
- <5% CPU usage when idle
- <100MB RAM usage

---

## Development Setup (Windows)

### Requirements
```bash
# Python 3.11+ (from python.org or Windows Store)
python -m venv venv
venv\Scripts\activate
pip install pygame==2.5.2
pip install -e .
```

### Running
```bash
# Development
python -m tower_climb

# Or after install
tower-climb
```

---

## File Structure (Simplified)
```
src/tower_climb/
├── __init__.py
├── __main__.py
├── core/
│   ├── game.py      # Main game class
│   ├── entity.py    # ECS base
│   └── state.py     # State management
├── systems/
│   ├── render.py    # All drawing code
│   ├── movement.py  # Position updates
│   ├── combat.py    # Damage dealing
│   └── input.py     # Keyboard handling
├── world/
│   ├── room.py      # Room generation
│   └── floor.py     # Floor management
└── ui/
    ├── menu.py      # Main menu
    └── hud.py       # In-game UI
```

---

## No External Dependencies
- Pure Python + Pygame
- No image files needed
- No sound files (use pygame.mixer.Sound generation if needed)
- All visuals from code
- Fonts from pygame.font.Font(None, size)

This keeps the project:
- Easy to distribute
- No asset pipeline
- Fast iteration
- Consistent visual style