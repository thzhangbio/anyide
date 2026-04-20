# iPad Codespaces UI Product Spec

## Goal
Build an iPad-optimized interface for GitHub Codespaces that makes common development tasks easier with touch-first navigation and simplified workflows.

## Target users
- Developers using iPad as a secondary or mobile workstation
- Technical users making quick edits, previews, and commits
- Maintainers who need to resume work quickly away from a desktop

## Core problems
- Desktop-style web IDE layouts are too dense for touch
- Important actions are buried in menus and shortcut-heavy flows
- File navigation, preview, run, and Git actions require too many steps on iPad

## Product direction
The product should be a lightweight iPad-first Codespaces workspace, not a full replacement for the official Codespaces IDE.

It should optimize high-frequency tasks:
- open or resume a codespace
- browse and edit files
- run the project and open preview
- review changes and commit/push
- fall back to the official Codespaces IDE for advanced workflows

## MVP scope
### In scope
- Codespace dashboard: list, create, start, stop, delete, open
- Workspace shell with large touch targets and bottom navigation
- File tree and editor view
- Basic search
- Run actions and preview entry
- Basic Git status, commit, and push
- One-tap link to open the official Codespaces web IDE

### Out of scope
- Full VS Code feature parity
- Full extension ecosystem
- Advanced debugger workflows
- Complex terminal multiplexing
- Real-time collaboration

## UX principles
- iPad-first, not desktop-shrunk
- Touch-friendly controls with clear primary actions
- Keep the editor area central and uncluttered
- Make high-frequency actions one or two taps away
- Preserve context across orientation changes
- Provide explicit fallback for unsupported advanced tasks

## Primary surfaces
### Dashboard
Shows recent and available codespaces with direct open/start/stop actions.

### Workspace
Main area for file editing, Git actions, run controls, and preview access.

### Settings
Lightweight preferences such as autosave and default layout behavior.

## Success criteria
- A user can reopen a recent codespace quickly on iPad
- A user can edit and save files without relying on keyboard shortcuts
- A user can run the project and open preview with minimal steps
- A user can complete a basic commit/push flow from the touch UI
- The interface feels simpler and more usable on iPad than the default web experience
