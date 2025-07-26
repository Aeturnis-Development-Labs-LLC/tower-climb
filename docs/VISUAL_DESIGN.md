# Tower Climb - Visual Design Specification

## Character Design: Minimalist Human

### Player Character
```python
def draw_player(screen, x: int, y: int, facing_angle: float):
    """Minimalist human design - pill body with head."""
    
    # Body (pill shape - overlapping circles + rectangle)
    body_height = 24
    body_width = 10
    
    # Main body rectangle
    body_rect = pygame.Rect(x - body_width//2, y - body_height//2 + 8, body_width, body_height - 8)
    pygame.draw.rect(screen, PLAYER_BLUE, body_rect)
    
    # Rounded top and bottom
    pygame.draw.circle(screen, PLAYER_BLUE, (x, y - body_height//2 + 8), body_width//2)
    pygame.draw.circle(screen, PLAYER_BLUE, (x, y + body_height//2 - 8), body_width//2)
    
    # Head
    pygame.draw.circle(screen, SKIN_TONE, (x, y - body_height//2), 5)
    
    # Eyes (simple dots)
    pygame.draw.circle(screen, BLACK, (x - 2, y - body_height//2), 1)
    pygame.draw.circle(screen, BLACK, (x + 2, y - body_height//2), 1)
    
    # Weapon/tool to show direction
    weapon_length = 15
    weapon_x = x + math.cos(facing_angle) * weapon_length
    weapon_y = y + math.sin(facing_angle) * weapon_length
    pygame.draw.line(screen, WHITE, (x, y), (weapon_x, weapon_y), 3)
    pygame.draw.circle(screen, WHITE, (int(weapon_x), int(weapon_y)), 2)
```

### Enemy Designs (Matching Minimalist Style)

#### Slime Enemy
```python
def draw_slime(screen, x: int, y: int, pulse_time: float):
    """Blob-like enemy that pulses."""
    # Pulsing effect
    base_radius = 10
    pulse = math.sin(pulse_time * 3) * 2
    radius = base_radius + pulse
    
    # Main body (green blob)
    pygame.draw.circle(screen, ENEMY_GREEN, (x, y), int(radius))
    pygame.draw.circle(screen, ENEMY_DARK_GREEN, (x, y), int(radius), 2)
    
    # Simple eyes
    eye_y = y - 2
    pygame.draw.circle(screen, BLACK, (x - 3, eye_y), 2)
    pygame.draw.circle(screen, BLACK, (x + 3, eye_y), 2)
```

#### Archer Enemy
```python
def draw_archer(screen, x: int, y: int, aim_angle: float):
    """Minimalist archer with bow."""
    # Body (smaller pill shape)
    body_height = 20
    body_width = 8
    
    # Main body
    body_rect = pygame.Rect(x - body_width//2, y - body_height//2 + 6, body_width, body_height - 6)
    pygame.draw.rect(screen, ENEMY_RED, body_rect)
    pygame.draw.circle(screen, ENEMY_RED, (x, y - body_height//2 + 6), body_width//2)
    pygame.draw.circle(screen, ENEMY_RED, (x, y + body_height//2 - 6), body_width//2)
    
    # Head
    pygame.draw.circle(screen, ENEMY_DARK_RED, (x, y - body_height//2), 4)
    
    # Bow (arc shape)
    bow_rect = pygame.Rect(x - 10, y - 10, 20, 20)
    pygame.draw.arc(screen, WHITE, bow_rect, aim_angle - 0.5, aim_angle + 0.5, 2)
    
    # Arrow indicator
    arrow_x = x + math.cos(aim_angle) * 12
    arrow_y = y + math.sin(aim_angle) * 12
    pygame.draw.line(screen, WHITE, (x, y), (arrow_x, arrow_y), 1)
```

---

## Environment Design

### Walls
```python
# Standard wall tile (32x32)
pygame.draw.rect(screen, DARK_GRAY, wall_rect)
pygame.draw.rect(screen, GRAY, wall_rect, 1)  # Border for depth
```

### Floor
```python
# Floor with subtle pattern
pygame.draw.rect(screen, LIGHT_GRAY, floor_rect)
# Add simple pattern every other tile
if (tile_x + tile_y) % 2 == 0:
    pygame.draw.rect(screen, MEDIUM_GRAY, inner_rect)
```

### Doors
```python
# Open doorway
pygame.draw.rect(screen, BLACK, door_rect)  # Darkness beyond
pygame.draw.rect(screen, BROWN, door_rect, 3)  # Door frame
```

### Stairs
```python
# Upward stairs (chevron pattern)
points = [
    (x - 12, y + 8),
    (x, y - 8),
    (x + 12, y + 8),
    (x + 8, y + 12),
    (x, y - 4),
    (x - 8, y + 12)
]
pygame.draw.polygon(screen, YELLOW, points)
pygame.draw.polygon(screen, GOLD, points, 2)
```

---

## Item Designs

### Health Potion
```python
# Simple flask shape
pygame.draw.circle(screen, RED, (x, y + 3), 5)  # Bottom
pygame.draw.rect(screen, RED, (x - 3, y - 5, 6, 8))  # Body
pygame.draw.rect(screen, DARK_RED, (x - 2, y - 7, 4, 3))  # Cork
```

### Power-Ups
```python
# Diamond shape with glow
points = [(x, y - 8), (x + 6, y), (x, y + 8), (x - 6, y)]
pygame.draw.polygon(screen, POWER_COLOR, points)
pygame.draw.polygon(screen, WHITE, points, 1)  # Highlight
```

### Gold Coins
```python
# Simple circles with shine
pygame.draw.circle(screen, GOLD, (x, y), 4)
pygame.draw.circle(screen, YELLOW, (x - 1, y - 1), 2)  # Shine
```

---

## UI Elements

### Health Bar
```python
def draw_health_bar(screen, x: int, y: int, current: int, max_hp: int):
    # Background
    bar_width = 100
    bar_height = 10
    pygame.draw.rect(screen, BLACK, (x, y, bar_width, bar_height))
    
    # Health fill
    fill_width = int((current / max_hp) * (bar_width - 2))
    pygame.draw.rect(screen, RED, (x + 1, y + 1, fill_width, bar_height - 2))
    
    # Border
    pygame.draw.rect(screen, WHITE, (x, y, bar_width, bar_height), 1)
    
    # Text overlay
    font = pygame.font.Font(None, 16)
    text = font.render(f"{current}/{max_hp}", True, WHITE)
    text_rect = text.get_rect(center=(x + bar_width//2, y + bar_height//2))
    screen.blit(text, text_rect)
```

### Damage Numbers
```python
def draw_damage_number(screen, x: int, y: int, damage: int, age: float):
    # Float up and fade
    float_y = y - (age * 30)
    alpha = max(0, 255 - (age * 255))
    
    font = pygame.font.Font(None, 20)
    text = font.render(str(damage), True, RED)
    # Note: Pygame doesn't support per-surface alpha well, 
    # so we'd use a fade by color instead
    fade_color = (255, int(255 - age * 200), int(255 - age * 200))
    text = font.render(str(damage), True, fade_color)
    screen.blit(text, (x - 10, float_y))
```

---

## Color Palette

```python
# Core Colors
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)

# Environment
DARK_GRAY = (32, 32, 32)    # Walls
GRAY = (64, 64, 64)         # Wall borders
MEDIUM_GRAY = (80, 80, 80)  # Floor pattern
LIGHT_GRAY = (96, 96, 96)   # Floor base
BROWN = (101, 67, 33)       # Doors
YELLOW = (255, 215, 0)      # Stairs
GOLD = (255, 180, 0)        # Stair border, coins

# Characters
PLAYER_BLUE = (64, 128, 255)     # Player body
SKIN_TONE = (255, 220, 177)      # Player head
ENEMY_GREEN = (64, 255, 64)      # Slime body
ENEMY_DARK_GREEN = (32, 180, 32) # Slime outline
ENEMY_RED = (255, 64, 64)        # Archer body
ENEMY_DARK_RED = (180, 32, 32)   # Archer head

# Items & UI
RED = (255, 0, 0)           # Health, damage
DARK_RED = (180, 0, 0)      # Potion cork
GREEN = (0, 255, 0)         # Healing
BLUE = (0, 128, 255)        # Mana/special
PURPLE = (255, 0, 255)      # Rare items
```

---

## Animation Guidelines

### Player Movement
- No sprite animation needed
- Weapon angle follows mouse/last direction
- Slight position bob while walking (±1 pixel vertical)

### Attack Animation
- Weapon swings in an arc (rotate 90 degrees)
- Quick white flash on hit
- Enemy knockback 4-8 pixels

### Death Animation
- Player: Collapse (shrink pill body to flat)
- Enemies: Burst into colored particles

### Visual Effects
- Damage: Red flash overlay on sprite
- Healing: Green particles floating up
- Level up: White ring expanding from player
- Hit impact: Small white burst at contact point

---

## Performance Notes

- All shapes drawn each frame (no sprite caching needed)
- Use `pygame.draw` for all visuals
- Batch similar draw calls together
- Avoid alpha blending where possible
- Target: 200+ entities on screen at 60 FPS