# UTF Contracts

This directory contains UTF (Universal Test Framework) contracts that define the behavior of Tower Climb systems.

## Contract Format

Contracts follow the P#-S#-C# format:
- **P#** - Phase number (P0, P1, P2...)
- **S#** - System number within phase (S1, S2, S3...)
- **C#** - Contract number within system (C1, C2, C3...)

Example: `P0-S1-C1` = Phase 0, System 1, Contract 1

## Phase Organization

- **Phase 0**: Core Systems (Window, Game Loop, Basic Rendering)
- **Phase 1**: Player and Tower (Movement, Basic Generation)
- **Phase 2**: Combat System (Enemies, Damage, Health)
- **Phase 3**: Items and Progression (Loot, Upgrades, Saves)
- **Phase 4**: Polish and Features (Audio, Effects, Advanced Content)

## Contract Structure

Each contract includes:
- Contract metadata (id, name, priority, time estimate)
- Specification (requirements, constraints)
- UTF test blocks (test scenarios with given/when/then)
- DDERF strategies (error recovery approaches)
- Security considerations (threat model, gates)
- Acceptance criteria