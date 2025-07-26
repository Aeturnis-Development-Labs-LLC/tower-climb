# CI/CD Strategy - Tower Climb

## Philosophy: Simple and Pragmatic

Based on lessons from ASCENT project:
- **Start simple, expand carefully**
- **Windows-first, expand later**
- **Local development priority**
- **Avoid CI/CD complexity spirals**

## GitHub Actions Workflows

### 1. CI Workflow (`ci.yml`)
**Triggers**: Push to main/develop, PRs

**Jobs**:
- **Test** (Windows only initially)
  - Python 3.11 and 3.12
  - Linting (flake8) - warnings only
  - Formatting check (black)
  - Type checking (mypy) - best effort
  - Tests with coverage (90% threshold)
  - Upload coverage artifacts

- **Security** (Simple checks)
  - Bandit for code security
  - Safety for dependency vulnerabilities
  - Non-blocking (records but doesn't fail)

### 2. Release Workflow (`release.yml`)
**Triggers**: Version tags (v*)

**Jobs**:
- Build Python wheel
- Build Windows executable (PyInstaller)
- Create GitHub release
- Upload artifacts

## Local Development Setup

### Pre-commit Hooks (Optional)
```bash
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/psf/black
    rev: 23.0.0
    hooks:
      - id: black
  
  - repo: https://github.com/pycqa/flake8
    rev: 6.0.0
    hooks:
      - id: flake8
        args: [--max-line-length=88]
```

### Local Quality Checks
```bash
# Run before pushing
make quality  # Runs format, lint, typecheck, test
```

## What We're NOT Doing (Yet)

1. **Multi-platform CI**: Windows only for MVP
2. **Complex test matrices**: Just 2 Python versions
3. **Deployment pipelines**: Manual releases
4. **Docker containers**: Not needed for simple game
5. **Code signing**: Future enhancement
6. **Coverage enforcement**: 90% target, not strict
7. **Automated version bumping**: Manual for now

## Quality Gates

### Strict Requirements
- Tests must pass
- No Python syntax errors
- Coverage ≥ 90%

### Warnings Only
- Black formatting (auto-fixable)
- Flake8 style issues
- Type checking issues
- Security warnings

## Deployment Strategy

### Development
1. Work on feature branches
2. PR to develop branch
3. CI runs automatically
4. Merge when green

### Release
1. Merge develop → main
2. Tag with version (e.g., v0.1.0)
3. GitHub Actions builds release
4. Manually test executable
5. Publish release notes

## Monitoring

### What to Track
- CI build times (should be <5 minutes)
- Test coverage trends
- Security scan results
- Release download counts

### Red Flags
- CI taking >10 minutes
- Coverage dropping below 85%
- Security issues marked HIGH
- Build failures on main branch

## Future Enhancements (Post-MVP)

### Phase 1 (After MVP ships)
- Add Linux CI (Ubuntu)
- Add macOS CI if needed
- Automated changelog generation

### Phase 2 (With user base)
- Code signing for Windows
- Auto-update mechanism
- Crash reporting
- Analytics (optional)

### Phase 3 (If successful)
- Steam deployment pipeline
- Multiple distribution channels
- Localization checks
- Performance benchmarks

## Remember

> "CI/CD should help, not hinder. If you're debugging CI more than your game, something's wrong."

The goal is to ship a fun game, not to have perfect CI/CD. Keep it simple, keep it working, keep it maintainable.