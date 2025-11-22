Eggman’s GitHub Release Manager is a multi-repo GitHub release downloader designed for digital preservation, long-term archiving, and automated syncing of release assets.
It supports metadata tracking, incremental updates, SHA-1 hashing, automatic rebuilds, Discord webhook notifications, and multiple export formats (DAT, CSV, JSON, PDF/TXT).

This tool is built using Python + Tkinter, with optional enhancements such as drag-and-drop (tkinterdnd2) and PDF generation (reportlab).

✨ Features
Release Downloading

Multi-threaded downloads with adjustable thread count

Resume support via HTTP Range

Detects and downloads only changed/missing assets

Per-asset progress reporting (%, speed, ETA)

Per-tag folder organization inside each repo

SHA-1 hashing for integrity verification

Repository Management

Track multiple repositories

Update one repo or all repos (oldest checked first)

Choose a custom root download directory

Automatic update mode with configurable intervals

Pause / Resume / Safe Stop / Hard Stop controls

Detailed activity log panel

Metadata System

All metadata is stored under:

_metadata/
   json/
   csv/
   dat/
   pdf/
   logs/


Includes:

Per-repo JSON metadata files

CSV files listing all assets

RomVault-compatible XML DAT export

TXT and optional PDF summaries

Metadata logs and download timestamps

Automatic Metadata Rebuilds

Running Scan Local Files (Rebuild Metadata) will:

Reindex all local files

Update internal metadata for each asset

Automatically regenerate JSON/CSV metadata only if missing

Hashing

SHA-1 for all downloaded or rescanned files

“Force Hash Rescan” option

CRC32 included in DAT exports

Orphan Detection

Finds files not present in metadata

Moves them to a dedicated /orphans folder

Provides a summary of moved files

Export Tools

JSON export

CSV export

TXT summary

PDF summary (requires reportlab)

RomVault XML DAT file generator

Discord Webhook Support

If configured:

Sends update notifications for each repo

Supports errors, successes, and batch update reports

Works with auto-update mode

📁 Folder Structure

A typical repo download looks like:

owner_repo/
   <tag1>/
   <tag2>/
   ...
   orphans/
_metadata/
   json/
   csv/
   dat/
   pdf/
   logs/


All repository metadata, logs, and exports live inside _metadata, keeping the root folder clean.

🧠 Timestamps

All timestamps use a universal UTC format:

YYYY-MM-DD_HH_MM_SSZ


Examples:

2025-11-22_13_45_09Z
2025-05-31_08_10_22Z


This format avoids ambiguity, avoids locale conflicts, and is stable for archival purposes.

🛠 Requirements

Python 3.10+ (tested fully on Python 3.13)

Standard libraries:

tkinter

json

csv

threading

datetime

Additional modules:

requests

Optional modules:

reportlab (PDF support)

tkinterdnd2 (drag-and-drop support)

🚀 Running the Program

From your command line:

python "Eggman's GitHub Release Manager.py"


When the UI starts:

Choose or create your root download folder.

Add a GitHub repo (full URL or owner/repo format).

Click Update Selected Repo or Update All Repos.

View downloaded assets under the repo’s folder.

Your repositories and settings will be saved between runs.

⚙ Configuration Files
eggman_github_dl_config.json

Stores:

Root download path

Auto-update settings

Discord webhook URL

UI options

eggman_github_repos.json

Stores:

Tracked repositories

Tags discovered

Asset metadata (size, SHA-1, path)

Per-repo timestamps

Last update results

These are created automatically.

🔒 Preservation-Focused Integrity

This tool emphasizes:

Accurate file preservation

Reproducible metadata

Clean version tracking

Consistent UTC timestamps

Compatibility with RomVault, Logiqx DATs, and other archival pipelines

Every asset includes:

Filename

Release tag

Absolute path

SHA-1 hash

Size in bytes

Download timestamp

📌 Notes

PDF generation requires reportlab; otherwise a TXT summary is generated.

Auto-update mode enforces a minimum interval of 5 minutes.

Metadata is automatically recreated when missing but not overwritten unless needed.

📣 Credits

Created by Eggman for large-scale GitHub release archiving, digital preservation workflows, and consistent long-term metadata tracking.
