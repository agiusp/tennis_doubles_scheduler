# STA Sunday Program Scheduler

A single-page, no-install web app for assigning available players to 6 courts on Sunday program day.

## Usage

1. Open `index.html` in any browser (double-click it, or `open index.html` on macOS).
2. Click **Roster file (CSV)** and select your data file. Two ready-to-use examples are included:
   - `sample_data.csv` — 26 players, clean data, good for a quick check.
   - `fictitious_players.csv` — 32 players with messier real-world data: several `Availability` formats (`Yes`/`No`, `Y`/`N`, `True`/`False`, `1`/`0`, blank, and an ambiguous `Maybe`) and uneven level/sex groupings, so every Organized Play mode leaves some players on the bench or a court red — good for testing drag-and-drop fixes.
3. Pick an **Organized Play** format:
   - Same-sex doubles
   - Mixed doubles
   - By play level
   - By play level and same-sex doubles
   - By play level and mixed doubles
4. Click **Auto-Assign Courts** to fill the 6 boxes (up to 4 players each) from the available pool, using a best-effort grouping for the selected format.
5. Move players between courts or the Bench to adjust manually:
   - **Mouse (desktop):** drag a name chip and drop it on another court or the Bench.
   - **Touch (phone/tablet):** tap a name to pick it up (it highlights and a banner appears), then tap the destination court or the Bench to place it there. Tap the same name again to cancel. This works better than drag-and-drop on touchscreens, and also lets you pick up a player, scroll down, and place them without holding a drag the whole way.
   
   A court turns **green** when it has exactly 4 players who satisfy the selected format, and **red** otherwise.
6. Use **Clear Courts** to send everyone back to the bench without re-running the file load.

Only players marked as available are ever placed on a court or shown in the bench; unavailable players are excluded entirely (the status line reports how many were excluded).

## Using it on a phone

The page itself is fully responsive (single-column court layout, larger tap targets, tap-to-move interaction) and works in any mobile browser. The only catch is *getting the file onto the phone*, since `index.html` lives on your computer:

- **Easiest — local Wi-Fi server:** on the computer, from this folder run `python3 -m http.server 8000`, then find the computer's local IP (e.g. `ipconfig getifaddr en0` on macOS) and visit `http://<that-ip>:8000/index.html` in the phone's browser (same Wi-Fi network required).
- **Or airdrop/email/cloud-sync** `index.html` (and a CSV) directly to the phone and open it from Files/Downloads — it runs standalone with no server needed, since everything is client-side.

## Data file format

CSV with a header row containing these columns (case-insensitive, any column order):

| Column | Notes |
|---|---|
| `Name` | Player's full name |
| `Level` | Play level, e.g. NTRP rating (3.0, 3.5, 4.0, ...) — matched exactly when grouping "by play level" |
| `Sex` | `M`/`F` (or `Male`/`Female`) |
| `Availability` | Recognized "available" values: `Yes`, `Y`, `True`, `1`, `Available`, `Avail` (case-insensitive). Anything else counts as unavailable. |

Each court chip is displayed as `Name, Sex, Level` so you can see at a glance whether it fits the selected format.

## Notes

- Everything runs client-side; no server or internet connection is required, and no data leaves your browser.
- The app doesn't save state between page reloads — re-select the CSV file if you refresh the page.
