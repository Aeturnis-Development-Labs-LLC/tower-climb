# Changelog

All notable changes to Tower Climb will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.4] - 2025-07-26

### Added
- State management system (P0-S2-C1)
- StateMachine with state stack support
- Three basic states: Menu, Playing, GameOver
- Atomic state transitions with error handling
- Stack depth limit to prevent overflow
- Pause/resume support for states

### Changed
- GameState now requires handle_event method
- GameState includes optional pause/resume methods
- License changed from MIT to CC BY-NC 4.0
- Added experimental software disclaimer

## [0.1.3] - 2025-07-26

### Added
- FPS counter with rolling window averaging (P0-S1-C3)
- Debug display with semi-transparent background
- Toggle functionality (F3 ready for integration)
- Cached rendering for performance

### Changed
- Coverage requirement relaxed from 90% to 80%

## [0.1.2] - 2025-07-26

### Added
- Main game loop implementation (P0-S1-C2)
- GameLoop class with 60 FPS target
- GameState abstract base class
- Deferred state transitions
- Delta time capping at 50ms

### Changed
- Window title now shows version number

## [0.1.1] - 2025-07-26

### Added
- Window creation system (P0-S1-C1)
- Basic Pygame integration
- Window event handling

## [0.1.0] - 2025-07-26

### Added
- Initial project structure
- CAFE methodology integration
- CI/CD pipeline setup
- Development documentation
- UTF contracts for Phase 0

[0.1.2]: https://github.com/Aeturnis-Development-Labs-LLC/tower-climb/compare/v0.1.1...v0.1.2
[0.1.1]: https://github.com/Aeturnis-Development-Labs-LLC/tower-climb/compare/v0.1.0...v0.1.1
[0.1.0]: https://github.com/Aeturnis-Development-Labs-LLC/tower-climb/releases/tag/v0.1.0