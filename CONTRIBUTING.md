# Contributing to TidyFactor Cleaner

Thank you for contributing to the `tidyfactor-cleaner` open-source ecosystem.

## Development Workflow

1. Clone repository and inspect single source of truth in `SKILL.md` and `references/`.
2. Adhere strictly to the non-negotiable safety constraints in `SKILL.md`.
3. Run validation script:
   ```bash
   python tools/validate_skill.py
   ```
4. Build distribution bundle:
   ```bash
   node tools/build-skill.js
   ```
5. Submit PR with detailed description and test verification.
