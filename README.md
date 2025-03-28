# Rem CLI

A minimalist command-line reminder tool written in Bash, designed for fast and flexible task management directly from your terminal. It uses a plain-text `reminders.txt` file as its database and supports recurring reminders, snoozing, rescheduling, and more.

## Features
- Add, list, edit, complete, and remove reminders
- Supports repeat logic (`weekly`, `monthly`, `yearly`, or `every N days`)
- Snooze reminders with relative offsets like `+3d`, `+2w`
- Reschedule reminders to specific future dates
- Clean, readable formatting with status tracking (`PENDING`, `DONE`)
- All reminders stored in a single, editable text file
- Fully local, with no dependencies beyond standard Unix tools

## Installation

### Scheduled Execution

The `remd` script must run daily to check `reminders.txt`, send notifications, apply repeat logic, add `PENDING` status, and process items marked `DONE`.

To automate this, schedule it with cron:

```bash
crontab -e
```

Add this line to run it daily at 5:00 AM:

```cron
0 5 * * * /home/youruser/bin/remd >/dev/null 2>&1
```

Make sure the `remd` script is executable and has correct paths set for your system.

### Mail Notifications

The script uses the `mail` command to send SMS/email notifications. The recipient is currently defined by the `RECIPIENT` variable inside `remd`. You must have `mail` configured properly (e.g. using `mailutils` or `msmtp`) for notifications to work.

A future improvement will likely include a configuration file to define notification settings, editor preference, backup location, default snooze/repeat behavior, etc.
Clone this repo and symlink `rem` into your `~/bin` directory (or somewhere in your `$PATH`):

```bash
mkdir -p ~/projects
cd ~/projects
# Copy or clone this project here
ln -s "$PWD/rem" ~/bin/rem
chmod +x rem
```

## Usage
```bash
rem add 2025-04-01 Call Mom
rem list
rem done 2
rem snooze 3 +4d
rem reschedule 3 2025-06-30
rem edit 4
rem remove 5
rem open
```

## Reminder Format
Reminders are stored line-by-line in `~/reminders.txt` as:
```
YYYY-MM-DD | STATUS | message [ --repeat pattern ]
```
- `STATUS` may be `PENDING`, `DONE`, or blank
- `--repeat` pattern is optional (see below)

## Supported Commands
| Command        | Shortcut | Description                                      |
|----------------|----------|--------------------------------------------------|
| `add`          | `a`      | Add a new reminder                              |
| `list`         | `l`      | Display all reminders, sorted by date           |
| `done <N>`     | `d`      | Mark reminder at line `N` as DONE               |
| `remove <N>`   | `r`      | Remove reminder at line `N`                     |
| `edit <N>`     | `e`      | Edit line `N` in your `$EDITOR` (e.g. `vim`)    |
| `snooze <N> +Nd`| `z`     | Snooze line `N` forward by N days (or w/m/y)    |
| `reschedule <N> YYYY-MM-DD` | `s` | Set new exact date for line `N`         |
| `open`         | `o`      | Open full `reminders.txt` in editor             |
| `help`         | `h`      | Show help                                       |

## Repeat Options
Repeat patterns allow reminders to recur indefinitely when marked DONE.

Examples:
```bash
rem add 2025-04-06 Team sync --repeat weekly
rem add 2025-05-01 Review finances --repeat every 30d
```

Supported patterns:
- `--repeat weekly`
- `--repeat monthly`
- `--repeat yearly`
- `--repeat every 90d` / `every 3 months`

## File Location
Reminders are stored in:
```
~/reminders.txt
```
Daily backups are created automatically in a backup directory (1-month rolling).

## Status Format
- Reminders marked `[ PENDING ]` require action and are shown at the top
- Reminders marked `[ DONE ]` will be archived the next morning

## Examples
```bash
rem add 2025-04-01 Renew passport
rem add 2025-04-02 Buy birthday gift
rem add 2025-04-06 Team sync --repeat weekly
rem snooze 2 +3d
rem done 1
```

## License
MIT (or modify as you wish — this is your personal tool)

---
For full documentation, see `Rem_Project_Report.md`.

