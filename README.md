# BasePilot

**BasePilot** — autopilot for your Clash of Clans base: a farming and base-progression bot for **Google Play Games on PC**.
PySide6 desktop UI, OpenCV template matching + Tesseract OCR vision, Win32 window
capture and input. No memory reading, no packet work — it plays the game the way a
human does: by looking at the screen and clicking.

Works at **any Town Hall level** — all detection is driven by templates, OCR, and the
game's own UI signals (builder chip, lab chip, full-storage icons), not hardcoded
per-TH values.

## Download

Grab the latest `BasePilot.exe` from the
[Releases page](https://github.com/efebolukbasi/BasePilot/releases/latest) and run
it — Tesseract is bundled inside, so there is nothing else to install. The exe is
unsigned, so Windows SmartScreen warns on first launch: *More info → Run anyway*.

Everything below is for running or building from source.

## Features

- **Loot farming** — finds matches, deploys (Valkyries / Sneaky Goblins / Super
  Minions / Edrags), collects, returns home, recovers from popups, disconnects,
  and stray screens. Builder Base farming included.
- **Wall upgrades** — batch-buys walls when loot passes a configurable threshold or
  storages fill, elixir-first, never spends gems, keeps the match entry fee.
- **Auto upgrade (beta)** — reads the builder menu with OCR and starts upgrades
  with your loot:
  - **Maxer**: cheapest affordable upgrade first, never the Town Hall.
  - **Rusher**: takes the Town Hall the moment it is affordable.
  - **Dry run**: logs what it *would* start, clicks nothing.
- **Run until maxed** — no time limit: farm → upgrade → and when storages are full
  with every builder busy and nothing startable, the bot **idles** and re-checks
  every 5 minutes instead of attacking for nothing. It resumes by itself when a
  builder frees up. Runs until you press Stop.
- **Live status panel** — free builders, lab state, storages, session loot and
  loot/hour, and the last upgrade decision.
- Multi-account sessions, star-bonus collection runs, ranked attack fill.

Not automated (yet): starting laboratory research and Pet House upgrades — the bot
tracks the lab chip and reminds you when the lab is idle, but you start those
yourself. Dark elixir storage has no "full" indicator in the game's UI, so idling
keys on gold + elixir only.

## Requirements

- Windows, Clash of Clans running in **Google Play Games on PC** at **16:9** or
  16:10. Ultrawide monitor? Use *Settings → Switch display to 16:9*, restart the
  game, restore — the game keeps its aspect (GPG locks it at launch).
- Python 3.11+ with `PySide6`, `opencv-python`, `pytesseract`, `numpy`, `pywin32`.
- A Tesseract 5 install in `tesseract_bundle/` (not committed — copy `tesseract.exe`,
  its DLLs and `tessdata/` from a [UB-Mannheim build](https://github.com/UB-Mannheim/tesseract/wiki)).

## Running

```
python main.py
```

Headless-ish autostart (used for scheduled/overnight runs):

```
BasePilot.exe --autostart --minutes 0 --walls --upgrades maxer
```

`--minutes 0` = run until maxed (no time limit). `--upgrades off|dry|maxer|rusher`.

Build a one-file exe with `pyinstaller BasePilot.spec`.

## Safety rails

- Never confirms a purchase whose cost reads red (unaffordable → gem-offer risk).
- Escapes unknown dialogs via the X / empty-ground taps, never a blind "Okay".
- Verifies every upgrade start against the builder counter; anything unverified is
  logged, screenshotted to `%LOCALAPPDATA%\BasePilot\debug\`, and cooled down.
- Keeps a gold buffer so matchmaking entry fees are never spent away.

## License

MIT — see [LICENSE](LICENSE). Tesseract, bundled into the released exe, ships
under the Apache 2.0 license.

## Disclaimer

Automation violates Supercell's Terms of Service and can get an account banned.
This project exists for educational purposes — computer vision, OCR, and UI
automation on a real, adversarially animated target. Use a throwaway account, or
don't use it at all.
