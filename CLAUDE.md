# CLAUDE.md - AI Assistant Guide

This file provides guidance for AI assistants working with this codebase.

## Repository Overview

**Repository:** marketingwithjustinopay/app
**Status:** New project (initial setup)
**Last Updated:** 2026-01-30

## Project Structure

```
/home/user/app/
├── CLAUDE.md          # AI assistant guidance (this file)
└── .git/              # Git repository
```

> **Note:** This repository is in initial setup. Update this section as the project structure evolves.

## Development Workflow

### Branch Strategy

- **Main branch:** Primary stable branch
- **Feature branches:** Use descriptive names (e.g., `feature/user-auth`, `fix/login-bug`)
- **Claude branches:** AI-assisted work uses `claude/` prefix with session IDs

### Git Commands

```bash
# Check status
git status

# Stage changes
git add <file>          # Specific files (preferred)
git add .               # All changes (use cautiously)

# Commit with descriptive message
git commit -m "type: description"

# Push to remote
git push -u origin <branch-name>
```

### Commit Message Convention

Use conventional commit format:

- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation changes
- `style:` - Code style (formatting, no logic change)
- `refactor:` - Code refactoring
- `test:` - Adding or updating tests
- `chore:` - Maintenance tasks

Example: `feat: add user authentication module`

## Code Conventions

### General Principles

1. **Simplicity** - Write clear, straightforward code
2. **Consistency** - Follow existing patterns in the codebase
3. **Documentation** - Comment complex logic, not obvious code
4. **Security** - Never commit secrets, credentials, or sensitive data

### File Organization

- Keep related files together
- Use descriptive file and folder names
- Maintain a flat structure when possible

## Testing

> **TODO:** Add testing instructions once test framework is configured.

## Build & Deployment

> **TODO:** Add build and deployment instructions once configured.

## Key Files to Know

| File | Purpose |
|------|---------|
| `CLAUDE.md` | AI assistant guidance |
| `README.md` | Project documentation (to be created) |
| `package.json` | Dependencies (if Node.js project) |
| `.env.example` | Environment variable template |

## Common Tasks

### Starting Development

```bash
# Clone repository (if not already done)
git clone <repository-url>

# Navigate to project
cd /home/user/app

# Install dependencies (once package.json exists)
npm install  # or yarn install
```

### Making Changes

1. Create/switch to feature branch
2. Make changes
3. Test locally
4. Commit with descriptive message
5. Push to remote
6. Create pull request (if applicable)

## Important Notes for AI Assistants

### Do

- Read files before modifying them
- Use specific file staging over `git add .`
- Follow existing code patterns
- Keep changes focused and minimal
- Test changes when possible
- Ask for clarification when requirements are unclear

### Don't

- Commit sensitive data (API keys, passwords, tokens)
- Make unnecessary changes to unrelated files
- Over-engineer solutions
- Add features not explicitly requested
- Use force push without explicit permission

### Security Checklist

- [ ] No hardcoded credentials
- [ ] No API keys in code
- [ ] No sensitive data in logs
- [ ] Input validation on user data
- [ ] Proper error handling without exposing internals

## Environment Variables

Create a `.env` file based on `.env.example` (when available):

```bash
# Example environment variables
NODE_ENV=development
# Add project-specific variables here
```

**Never commit `.env` files to version control.**

## Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| Git push fails | Check branch name, verify remote access |
| Dependencies missing | Run `npm install` or equivalent |
| Build fails | Check error logs, verify environment setup |

## Resources

- [Project README](./README.md) (to be created)
- [Contributing Guidelines](./CONTRIBUTING.md) (to be created)

---

*This CLAUDE.md will be updated as the project develops. When adding new features or configurations, please update the relevant sections.*
