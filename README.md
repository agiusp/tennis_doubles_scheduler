# STA Sunday Program Scheduler

A single-page, no-install web app for assigning available players to 6 courts on Sunday program day.

## Usage

1. Open `index.html` in any browser (double-click it, or `open index.html` on macOS).
2. Load your roster, either way:
   - **CSV file:** click **Roster file (CSV)** and select a file. Two ready-to-use examples are included:
     - `sample_data.csv` — 26 players, clean data, good for a quick check.
     - `fictitious_players.csv` — 32 players with messier real-world data: several `Availability` formats (`Yes`/`No`, `Y`/`N`, `True`/`False`, `1`/`0`, blank, and an ambiguous `Maybe`) and uneven level/sex groupings, so every Organized Play mode leaves some players on the bench or a court red — good for testing drag-and-drop fixes.
   - **Google Sheet:** the **Google Sheet URL** field comes pre-filled with a live, publicly-shared demo sheet ([view it here](https://docs.google.com/spreadsheets/d/1T8p18YhGXSUfXnrtMpeVhDhEravOVEe2bTI4U3qoQe0/edit?gid=0#gid=0)) holding the same 32-player roster as `fictitious_players.csv` — just click **Load from Google Sheet** to try it immediately. It also doubles as a template for the sharing settings and column layout a working sheet needs, including a checkbox-style `Availability` column (Google Sheets renders checkboxes as `TRUE`/`FALSE`, which the app also understands). To use your own instead, paste its URL over the pre-filled one (click **Reset to the example** to bring the demo link back). Your sheet must be shared as *"Anyone with the link — Viewer"* (Share button, top right of Sheets) and its first row must have the same `Name`/`Level`/`Sex`/`Availability` headers as the CSV format below. It reads whichever tab the link opens to by default; to target a different tab, open that tab in Sheets and copy its URL (it will end in `#gid=<number>`) into the box.
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

## Data format

Whether it's a CSV file or a Google Sheet, the first row must be a header row containing these columns (case-insensitive, any column order):

| Column | Notes |
|---|---|
| `Name` | Player's full name |
| `Level` | Play level, e.g. NTRP rating (3.0, 3.5, 4.0, ...) — matched exactly when grouping "by play level" |
| `Sex` | `M`/`F` (or `Male`/`Female`) |
| `Availability` | Recognized "available" values: `Yes`, `Y`, `True`, `1`, `Available`, `Avail` (case-insensitive). Anything else counts as unavailable. |

Each court chip is displayed as `Name, Sex, Level` so you can see at a glance whether it fits the selected format.

## Notes

- Everything runs client-side; no server, backend, or API key is required, and your roster data never passes through any third party besides Google itself when you use the Sheet option.
- Reading a Google Sheet works by loading it as a script tag (the standard trick for public Sheets, since Google's CSV export doesn't allow direct cross-site fetches) — it only works for sheets shared publicly via link, not private ones. If the sheet isn't shared correctly, or the URL/tab is wrong, you'll get an error after a few seconds rather than a silent failure.
- The app doesn't save state between page reloads — reload the CSV file or Google Sheet URL again if you refresh the page.
