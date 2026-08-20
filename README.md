# BasePilot

**Autopilot for your Clash of Clans base.** BasePilot farms, upgrades, and knows when
to do nothing — it runs unattended until your village genuinely has nothing left to
start, then waits for a builder to free up and gets back to work.

Windows desktop app for **Clash of Clans on Google Play Games (PC)**. It plays the
game the way a person does: screen capture, computer vision, and clicks. No memory
reading, no packet manipulation, no modified client.

**[Download the latest release](../../releases/latest)** — a single `BasePilot.exe`
with the OCR engine bundled in, so there is nothing to install. This repository holds
the full source; see [Running from source](#running-from-source) to build it yourself.

![BasePilot running beside Clash of Clans: the Run page shows Maxer mode with "Run until
maxed" enabled, and the Live Status panel reports IDLING with 0/6 builders free and both
storages full](docs/basepilot-screenshot.png)

*BasePilot idling on purpose: storages are capped and every builder is busy, so it holds
position and rechecks instead of raiding for loot that would overflow.*

---

## What it does

**Farming.** Finds matches, deploys your army (Valkyries, Sneaky Goblins, Super
Minions, or Edrags), collects loot, returns home, and recovers on its own from popups,
disconnects, and stray screens. Builder Base farming included.

**Auto upgrade (beta).** Reads the builder menu with OCR and spends your loot:

- **Maxer** — starts an affordable upgrade and never touches your Town Hall. Priciest
  first by default, since a farmed account is builder-limited rather than loot-limited;
  switch to cheapest first in *Settings → Upgrade order* to spread builders across more
  jobs. Dark elixir upgrades (heroes) always get first claim either way.
- **Rusher** — takes the Town Hall as soon as it's affordable.
- **Dry run** — logs what it *would* start and clicks nothing. Good first setting.

**Run until maxed.** No time limit. Farm → spend → and when storages are full with
every builder busy, BasePilot **idles** instead of raiding for loot that would
overflow, rechecking every few minutes and resuming the moment something frees up.
It only stops when you tell it to.

**Wall upgrades.** Batch-buys walls when loot passes a threshold you set, elixir first,
keeping enough gold for match entry fees.

**Loot tracking.** Every raid's gold, elixir, and dark elixir gains are read straight
off the HUD and accumulated into a session total plus a **loot-per-hour rate**, so you
can see what an army or strategy is actually earning you instead of guessing. Readings
require agreement across consecutive frames and are sanity-checked against what a
single raid can plausibly yield, so a bad OCR frame can't inflate your numbers. (Note
that a capped storage banks nothing — the rate reflects real gains, not raid count.)

**Live status.** Current state, free builders, laboratory, and storage levels at a
glance, alongside the loot readout.

Works at **any Town Hall level** — detection reads the game's own UI signals (builder
chip, lab chip, storage indicators) rather than hardcoded per-TH values.

Not automated yet: starting laboratory research and Pet House upgrades. BasePilot
tracks the lab and tells you when it's idle, but you start those two yourself.

## Safety rails

Automation that spends resources has to be careful, so BasePilot:

- Never confirms a purchase whose cost shows red (unaffordable → gem-spend risk).
- Requires the screen to **name the building it picked** before any purchase click, so
  a mis-aimed click can't buy the wrong thing.
- Verifies every upgrade actually started by checking the builder counter afterward.
- Escapes unknown dialogs via their close button or empty ground — never a blind "OK".
- Keeps a gold buffer so matchmaking entry fees are never spent away.
- Screenshots anything it couldn't verify to `%LOCALAPPDATA%\BasePilot\debug\` and
  benches that upgrade instead of retrying blindly.

## Requirements

- Windows 10/11
- Clash of Clans running in **Google Play Games on PC**
- The game rendering at **16:9** or 16:10

**Ultrawide / 21:9 monitors:** Google Play Games locks the game's aspect ratio to your
display resolution at launch. Use *Settings → Switch display to 16:9*, fully close and
reopen Clash, then *Restore my display* — the running game keeps 16:9.

## Getting started

1. Download `BasePilot.exe` from [Releases](../../releases/latest) and run it (no
   installer; settings live in `%LOCALAPPDATA%\BasePilot`). The exe is unsigned, so
   Windows SmartScreen warns on first launch — *More info → Run anyway*.
2. Open the game, then press **Test** on the Settings page to confirm BasePilot can see
   it. Use **Auto-detect** or pick the window manually if needed.
3. On the Run page, choose your army, set **Auto upgrade → Dry run** for the first
   session, and press **Start**. Watch the Logs page to see what it would do.
4. Happy with its choices? Switch to **Maxer**, enable **Run until maxed**, and set
   *Settings → Reserve builders* (use 0 if your walls are maxed).

Command line, for scheduled or overnight runs:

```
BasePilot.exe --autostart --minutes 0 --walls --upgrades maxer
```

`--minutes 0` means run until maxed. `--upgrades off|dry|maxer|rusher`.

## Running from source

Python 3.11+ on Windows:

```
pip install -r requirements.txt
python main.py
```

OCR needs a Tesseract 5 install. For development, BasePilot falls back to
`C:\Program Files\Tesseract-OCR\tesseract.exe`, so a
[UB-Mannheim build](https://github.com/UB-Mannheim/tesseract/wiki) is enough — or point
`TESSERACT_CMD` at any `tesseract.exe` you prefer.

To build the one-file exe, copy that Tesseract install (`tesseract.exe`, its DLLs, and
`tessdata/`) into `tesseract_bundle/` and run:

```
pyinstaller BasePilot.spec
```

`tesseract_bundle/` is gitignored to keep the repo light — it is ~40 MB of Apache-2.0
binaries. The release workflow in `.github/workflows/release.yml` recreates it on a
Windows runner and publishes the exe automatically on every `v*` tag.

## License

MIT — see [LICENSE](LICENSE). Tesseract, bundled into the released exe, ships under the
Apache 2.0 license.

## Disclaimer

Automating Clash of Clans **violates Supercell's Terms of Service and can get your
account banned.** BasePilot is published for educational purposes — it's a real-world
exercise in computer vision, OCR, and UI automation against an animated, adversarial
target. Use it on an account you're willing to lose, or don't use it at all. No
warranty; you accept all risk.

Not affiliated with, endorsed by, or associated with Supercell. Clash of Clans is a
trademark of Supercell Oy.
