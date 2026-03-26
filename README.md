EGGMAN'S GITHUB RELEASE MANAGER
--------------------------------
Eggman’s GitHub Release Manager is a multi-repo GitHub release downloader with metadata tracking, incremental updates, hash verification, auto-update scheduling, Discord notifications, and export utilities such as generate RomVault dats, PDF/TXT summaries,  and CSV/JSON metadata.
It is built using Python + Tkinter and supports optional drag-and-drop and PDF generation.

GENERAL FEATURES
---
- Track multiple GitHub repositories at once.

- Incremental downloading:
	- Downloads only missing/changed assets.
	- Supports resume via HTTP Range requests.

- Multi-threaded downloads with adjustable thread count.

- GitHub API token support - Anonymous GitHub API access is capped at 60 requests/hour. Adding your GitHub token raises the limit to 5,000/hour.

- output a tracked repo's file structure and hash info to a RomVault XML datfile (dats should also work in other Rom managers, but I do not check for usefulness beyond RomVault). This is my MAIN reason for creating this app, so I can routinely create datfiles for repo's I track and preserve their content in my own collection, managed by RomVault.

- Full SHA-1 hashing for integrity checks. CRC32 is also computed when creating an XML datfile.

- Auto-update mode:
	- Update all repos every X minutes.
	- Interval is user-configurable.

- Per-repo and batch updates (update all tracked repos).

- Automatic folder organization:
	- Root download folder contains one folder per repo.
	- GitHub Release tags become subfolders inside the repo folder.

Metadata system:
	- Per-repo asset details stored in JSON format.
	- Additional per-repo CSV/JSON logs stored in a separate metadata folder outside of the repo's and their files.

- Optional Discord webhook notifications for update results.

- Activity log panel shows real-time progress and worker output.

- Modern dark UI theme with orange highlights. Easy on the eyes.

METADATA & FILE ORGANIZATION
---
Each repo uses the following structure:

	<root_download_folder>\
		owner_repo\
			<tag1>\
			<tag2>\
			...
			orphans\
		_metadata\
			json\
			csv\
			dat\
			pdf\
			logs\

- All metadata files (JSON/CSV/DAT/PDF/TXT) are placed under _metadata.
- Legacy metadata files found in the repo's root folder are automatically migrated into _metadata.
- When rebuilding metadata, JSON/CSV files are regenerated only if missing.

TIMESTAMPS
---
All timestamps use:

YYYY-MM-DD_HH_MM_SSZ

Format is stable, consistent, timezone-aware, and uses UTC ("Z").

DOWNLOAD PROCESS
---
- Retrieves all releases for a repo through the GitHub API.
- Skips existing files if “Only download new/changed/missing assets” is enabled.
- Downloads each asset with:
	- Resume support.
	- Size verification.
	- SHA-1 hashing.
- Progress reporting (percent, MB/s, ETA).
- Writes new metadata after each asset completes.

AUTOMATIC REBUILDS DURING “SCAN LOCAL FILES”
---
When running Scan Local Files (Rebuild Metadata):
- Local files are indexed.
- Tags are determined from top-level subfolders under the repo’s folder.
- Hashing and metadata fields are updated.
- JSON/CSV metadata inside _metadata are automatically recreated if missing.

EXPORT FEATURES
---
- Generate DAT (selected repo):
	- Produces a RomVault-style XML .dat file.
	- Game name = release tag folder name.
	- Handles nested files via relative paths.

- Generate PDF Summary (selected repo):
	- Requires reportlab.
	- If missing, falls back to generating a TXT file.

- Export DB…
	- Saves eggman_github_repos.json externally for backup.

ORPHAN CLEANUP
---
- Finds files in the repo folder that are missing from metadata.
- Moves all orphans into a dedicated orphans subfolder.
- Produces a summary of how many files were moved.

HASH RESCAN
---
- Recomputes SHA-1 for all known assets.
- Updates metadata in eggman_github_repos.json.
- Reports successes and missing files.

AUTO-UPDATE MODE
---
- When enabled, runs a full multi-repo update on a timer.
- Intervals are in minutes (minimum 5).
- Auto-update includes Discord notifications per repo.

CONTROLS & BUTTONS
---

- Update Selected Repo
	- Fetch and download assets for the current repo.

- Update ALL Repos
	- Processes repos in order of oldest “last checked.”

- Safe Stop
	- Finishes current downloads but skips remaining tasks.

- Hard Stop
	- Aborts all threads as soon as possible.

- Pause / Resume
	- Suspends active downloads temporarily.

REQUIREMENTS
---
Python 3.10+ recommended (tested on 3.13).

Modules used:
requests
tkinter (built-in)
tkinterdnd2 (optional)
reportlab (optional)

CONFIG FILES
---
There are 2 config files that are stored alongside the python script's location.

- eggman_github_dl_config.json
	- Stores general settings (root path, webhook URL, auto-update settings, etc.)

- eggman_github_repos.json
	- Persistent metadata for all tracked repos.
	- Stores last_checked, last_result, and detailed asset hashes/sizes/paths.

If you delete these, they will be recreated after you re-add the repo(s) and rescan any existing files or re-download the repo.


RUNNING THE PROGRAM
---
Run via:

	python "Eggman's GitHub Release Manager.py"
	
TIPS:
- If you decide you want to keep this app as part of your toolkit, place the script somewhere static and use one of the batch files as a shortcut for ease of use
- it is recommended to setup a static download folder to store the repo's files.
- The app is portable so you can move your stuff somewhere else, set it up and carry on with your workloads.

The interface will open immediately and load saved settings.  I've included some batch files if you want a quick way to get the app started.

DASHBOARD USAGE
---
1. Adding a Repo:
- Enter "owner/repo" in the GitHub repo box
	- example:  Eggmansworld/Datfiles
	OR 
 - paste a GitHub URL (the tool converts it automatically)
	- example: https://github.com/Eggmansworld/Datfiles
- Click "Add Repo" button, which then adds the repo to the Tracked Repos list.

2. Choose a Root Download Folder:
- This folder will contain:
	- root_folder/owner_repo/tag/asset.ext
- This is the folder where you want your repo's to be downloaded to.

3. Updating a Repo:
- highlight the repo in the Tracked Repos list
- Click "Update Selected Repo" button
- This downloads missing or changed assets and updates metadata.

4. Updating All Repos:
- Click the "Update ALL Repos" button
- Repositories are updated in order of the oldest "last checked" time.

5. Dashboard Controls:
- Double-click repo: open its folder
- Right-click repo context menu 
	- Open Repo Folder
	- Force Hash Rescan
	- Generate Dat File
	- Generate PDF Summary
	- Cleanup Orphans
	- Scan Local Files (Rebuild Metadata)
	- Remove From Tracker
	
6. Exported Metadata:
- Every update produces:
	- JSON asset list (if file missing, it is recreated)
	- CSV asset list (if file missing, it is recreated)
	- Optional DAT file (when selected)
	- Optional PDF summary (when selected)

7. Auto-Update:
- Enable auto-update and choose an interval (min 5 minutes)
- The tool will:
	- Scan all repos
	- Download updates
	- Leave a log entry
	- Send Discord notifications (optional)
	- Runs quietly in the background.

WARNING!  Avoid excessively frequent update checks to prevent GitHub rate-limiting. Anonymous GitHub access is 50 requests/hr. Using a free API token gives you 5,000/hr. If you are seeing rate limited messages in the activity window, SLOW DOWN YOUR UPDATE REQUESTS.  It's not my ass on the line if you do stupid things.

MAINTENANCE TOOLS
---

Orphan Cleanup:
- Finds files not listed in metadata
- Moves them into: owner_repo/orphans/

Force Hash Rescan:
- Recomputes hashes for every known asset
- Updates the metadata database

Discord Notifications:
- Enter a Discord webhook URL to get update summaries
- the tool will send:
	repo_name: update finished — OK=xx, failed=yy
	
NOTES
---

- GitHub rate limits apply; the script uses safe request pacing
- Private repos require manual authentication integration (not yet implemented)
- PDF support requires reportlab; otherwise TXT summary is used
- DAT exports are simplified and suitable for basic set tracking

CREDITS
---

Created for the preservation community by Eggman, with Claude’s help turning ideas into code.

If you improve the script, feel free to share your changes back with the community.
