# Rem CLI Project: Comprehensive Report

## Project Overview

The **Rem CLI tool** is a Bash-based command-line reminder system designed to provide users with a lightweight, flexible, and fast way to manage time-sensitive and recurring tasks directly from the terminal. The tool operates on a single plaintext `reminders.txt` file that acts as the persistent store or "database," while the CLI interface offers users powerful capabilities to manipulate, inspect, and track their reminders.

## Project Goals

- Provide a **simple, local, text-based** personal reminder system
- Integrate seamlessly into a terminal-centric workflow
- Avoid dependence on third-party apps, cloud services, or GUI software
- Allow flexibility through **direct file editing** and robust CLI commands
- Maintain an **extensible, modular design** for future growth
- Keep the data model human-readable and editable at all times

## Core Accomplishments

- Implemented command parsing for intuitive syntax (`rem a`, `rem l`, etc.)
- Support for adding, listing, editing, deleting, snoozing, rescheduling reminders
- Support for repeating reminders via `--repeat [weekly|monthly|yearly|every N units]`
- Tracking and marking reminders as `PENDING` or `DONE`
- Automatic sorting of reminders by date
- Automatic date validation and formatting (`YYYY-MM-DD` enforced)
- Fully self-contained with no dependencies outside POSIX utilities
- Backup capability via daily snapshots of `reminders.txt`
- Robust error handling and clear usage/help guidance

## Detailed Specifications

### Reminder Format (in `reminders.txt`):

```
YYYY-MM-DD | STATUS | message text [ --repeat pattern ]
```

- `STATUS` may be empty, `PENDING`, or `DONE`
- Repeat pattern is optional and must follow the `--repeat` keyword

### Supported Commands:

- `rem add YYYY-MM-DD message [--repeat pattern]`
- `rem list`
- `rem done N` (mark line `N` as `DONE`)
- `rem remove N` (delete line `N`)
- `rem edit N` (open line `N` in `$EDITOR`)
- `rem snooze N +Nd|+Nw|+Nm|+Ny` (postpone line `N` from today)
- `rem reschedule N YYYY-MM-DD` (set new date for line `N`)
- `rem open` (opens entire reminder file in editor)
- `rem help`

### Repeat Patterns Supported:

- `--repeat weekly`
- `--repeat monthly`
- `--repeat yearly`
- `--repeat every 10d` / `every 3 weeks` / `every 2 months`

### Behavior Details:

- Repeats are triggered only when a reminder is marked DONE
- New repeated entry is scheduled based on today’s date (not original)
- DONE entries remain until cleared by the `remd` daemon
- Reminders are sorted on every add/edit/snooze/reschedule operation
- Listing uses aligned formatting and highlights `PENDING`/`DONE` items

## Project Structure

- `~/projects/rem-cli/` - project folder
  - `rem` - main CLI script
  - `reminders.txt` - primary data file
  - `backup/` - archived snapshots of previous reminder files
  - `README.md` - usage and setup documentation
- `~/bin/rem` - symlink to script for convenient access

## Key Design Philosophy

- **Everything is editable**: the `reminders.txt` file remains readable and hand-editable
- **CLI-first design**: minimal friction for power users
- **Minimal dependencies**: works on any Unix-like system with core utilities
- **Respects user environment**: uses `$EDITOR`, local time, standard mail system

## Future Improvements

### Short-Term Enhancements:

- `rem search keyword`
- Filter by date ranges or status
- Batch operations (e.g. mark multiple lines DONE)
- Metadata tracking (snooze counts, original date, etc.)
- Colored output for better readability

### Long-Term Possibilities:

- Anchored repeats (e.g. 1st of month, 2nd Tuesday)
- Richer repeat logic parsing
- Transition to Python for better date handling and scaling
- Persistent snooze context (tracking how long reminders are snoozed)
- Web or TUI frontend while keeping `reminders.txt` as backend
- Export to calendar or sync with external services (optional)
- Notifications integration with desktop or system services

## Conclusion

The Rem CLI tool is a lean, powerful solution for managing reminders in a text-based workflow. It is extensible, robust, and designed to scale gracefully with future needs. It provides a refreshing alternative to cloud-dependent tools by focusing on simplicity, local control, and direct data access. This report should provide a strong base for any future developer or user seeking to build on, extend, or maintain the system.

