# Publish this exhibition on Render

1. Create a GitHub account if needed. Make a new repository and upload the **contents of this `Exhibition Template` folder** (`app.py`, `index.html`, `requirements.txt`, `render.yaml`, the guides, and the `static` folder containing the stadium hero image). Do not upload your CFBD key or the old `exports` files.
2. Create a Render account at https://render.com/ and choose **New → Blueprint**. Connect the GitHub repository. Render reads `render.yaml` and creates a Python web service.
3. When asked for `CFBD_API_KEY`, enter your private College Football Data API key. `CFBD_SEASON` defaults to 2026; change it in Render's environment settings for a different season.
4. Wait for the build and the first Numba compilation. Open the public URL ending in `.onrender.com` shown on the service page. If deployment fails, inspect Render's Logs; do not post the key in screenshots.
5. Open the website's source panel and confirm numerical values and `ok` status. If you see `HTTP 401`, check the key. If `no completed game`, check the season and team names in `app.py`.

The Free service can sleep after 15 minutes idle, so the first visit later may take time. The Python simulation is CPU-intensive; if the free instance runs slowly or exceeds its resources, choose a larger instance or reduce `FRAME_HZ` in `app.py`. Layout changes made in Place mode are shared by all visitors while the process runs; to keep a layout, copy it into `PLACEMENTS` and redeploy. JSON exports on the server are temporary, and the hosted server's file paths cannot be read directly by Grasshopper on your computer. Use the local version for Rhino/Grasshopper handoff.

## Design update

The site is titled **Game Day Climate** and uses a sports-dashboard visual style. The **What drives the room** cards describe the API metric, rule threshold, target output, and latest reading for all nine systems. Push the updated `index.html` to the same GitHub repository to redeploy the existing Render site; keep `app.py` and `render.yaml` alongside it.

## Stadium edition

The site now has three heat pumps, three humidifiers, and three fans, plus the original six fixed ceiling lights. Upload the `static/stadium-night.png` image with the files. The HTML refers to `/static/stadium-night.png`, which Flask serves automatically. On GitHub, confirm the image is under a folder named `static` at the repository root.
