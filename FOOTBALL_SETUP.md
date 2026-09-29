# College football exhibition setup

This version uses the College Football Data `/games` endpoint. For Tennessee, Georgia, and Alabama, it selects each team's most recent **completed regular-season game** in the configured season. It checks for new data every 15 minutes. It does not show live, in-progress scores. If a team has no completed game, its linked systems keep their default setpoints until data arrives.

## Start

1. Install Python 3.11 or newer. Open a terminal inside this `Exhibition Template` folder.
2. Install dependencies: `python -m pip install -r requirements.txt`.
3. Obtain a College Football Data API key from https://collegefootballdata.com/ . In macOS/Linux terminal: `export CFBD_API_KEY='paste-your-key-here'`. In Windows PowerShell: `$env:CFBD_API_KEY='paste-your-key-here'`. Keep the key private; do not paste it into `app.py` or Antigravity prompts.
4. Run `python app.py`, then open http://127.0.0.1:5000 . Initial simulation compilation may take several seconds.
5. The API source status should say `ok` and show numerical points/margins. `HTTP 401` means the key is missing or invalid; `HTTP 403` may indicate your account cannot access the endpoint. Check your CFBD account and the terminal environment. `no completed game` means the configured season/team has none yet.

To use a different season, set `CFBD_SEASON` before starting (`export CFBD_SEASON=2025` or `$env:CFBD_SEASON='2025'`). To change teams, edit the `TEAMS` tuple near the top of `app.py`, using CFBD's exact team names. Restart after edits. The first, second, and third teams correspond to indices 1, 2, and 3.

## Mapping

Each of Tennessee, Georgia, and Alabama has its own **heat pump, humidifier, and fan**, plus two of the six fixed ceiling lights. The three team bays are arranged left to right across the room. All values come from each team's most recent completed regular-season game.

| System for each team | Input | Condition | Output |
| --- | --- | --- | --- |
| Heat pump | Team points scored | 30 or more | 27°C; otherwise 19°C |
| Humidifier | Team scoring margin | Between −7 and +7 | 70% RH; otherwise 35% RH |
| Fan | Team scoring margin | 14 or more | 4 m/s; otherwise 0.5 m/s |
| Front light (zones 1–3) | Team points scored | 30 or more | 1.0 intensity; otherwise 0.15 |
| Back light (zones 4–6) | Team scoring margin | Win (>0) | 0.85 intensity; otherwise 0.12 |

`margin = team points − opponent points`. Team 1 is Tennessee, Team 2 Georgia, Team 3 Alabama. The lights remain in their original fixed ceiling zones. Conditions and outputs are editable in `RULES` in `app.py`. The simulation is conceptual, not instructions for physical HVAC equipment.

## Layout and Rhino

You do **not** need Rhino to position systems in the web simulation. In the webpage's **Place** mode, drag any of the three heat pumps, three humidifiers, and three fans; scroll over a unit to rotate it and shift-scroll for power. Click **Copy placement for Claude**, then replace the `PLACEMENTS` list in `app.py` with the copied Python and restart. Alternatively edit `x`, `y`, and `rot` directly there. The room is 12 m wide by 8 m deep; x increases left to right, y front to back. Six ceiling lights are fixed in their zones.

Use **Rhino/Grasshopper only if your assignment also calls for a Rhino model or physical spatial representation**. In Simulate mode, click **Write JSON to exports/**. In Grasshopper, use File Read with a Timer to read `exports/<system_id>.json` or `exports/_all_systems.json`. `position_m` and `rotation_deg` provide geometry placement. Re-export after results change. The template writes snapshots when you click the button; check with your instructor if they expect a continuously updating Rhino model.
