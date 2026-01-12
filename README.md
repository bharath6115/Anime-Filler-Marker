# SeriesGraph: Anime Filler Marker

Ever used SeriesGraph to see the episode wise rating of an anime only to be disappointed by the straight 6's on some filler episodes? With this script, we can visually distinguish the filler episodes!

This is a Tampermonkey userscript that automatically highlights filler episodes on [SeriesGraph.com](https://seriesgraph.com) by overlaying them with a distinct visual marker (dimming + white border).

## ✨ Features
* **Automatic Highlighting:** Fetches filler data automatically for the show currently viewing.
* **Toggle Switch:** Easily show or hide the filler markers using a native-looking UI switch.
* **Dynamic Updating:** Re-calculates markers when you change the graph layout (Linear/Grid) or resize your browser window.
* **Responsive Tooltips:** Ensures episode tooltips appear above the filler markers so you can still read episode details.

## 🚀 Installation
1. Install the **Tampermonkey** extension for your browser ([Chrome](https://chrome.google.com/webstore/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo), [Firefox](https://addons.mozilla.org/en-US/firefox/addon/tampermonkey/)).
2. Click the **Raw** button on the `SeriesGraph_AnimeFillerTracker.user.js` file in this repository.
3. Hit the "create a new script button"  
   <img width="321" height="353" alt="image" src="https://github.com/user-attachments/assets/4f82bded-a6ad-4dd1-80fa-b9ce1f0d5f92" />
4. Copy the code and paste it in new script section and save it (ctrl + S)
5. Navigate to any series on [SeriesGraph.com](https://seriesgraph.com) to see it in action.

## 🛠️ How it Works
I built this script to act as a bridge between the rating charts on SeriesGraph and external filler databases. Here is the behind-the-scenes "magic" that makes it happen:
### 1. Making sense of the URL
Every time you load a page, the script looks at the URL and grabs the series title. The `toSkewerCase` function cleans up the title—stripping out accents (like `é`), removing special symbols, and swapping spaces for hyphens, so it can talk to the API successfully.
### 2. Fetching the "Filler List"
The script sends a quick request to a Worker API (`filler-list.chaiwala-anime.workers.dev`). If the anime is in the database, it returns a list of episode numbers.
### 3. Finding the Bars on the Graph
This was the tricky part. SeriesGraph builds its charts using **SVG `<rect>` elements**. Because you can't easily add standard CSS borders or complex layers inside an SVG without breaking the site's layout, I took a different approach:
* The script finds every bar (rectangle) on the chart.
* It calculates exactly where that bar is sitting on your screen using `.getBoundingClientRect()`.
### 4. The Filler Overlay
Instead of changing the site's actual code, the script creates a new, semi-transparent `div` (an **overlay**) and pins it directly on top of the filler bars.
### 5. Staying in Sync
If you resize your window or toggle between "Linear" and "Grid" views, the bars move. The script uses a `MutationObserver` to watch for these changes and automatically re-aligns the markers so they never get left behind.

## ⚠️ Known Issues / Limitations
* **Slug Matching:** The script relies on "skewer-case" titles (e.g., `naruto-shippuden`) to match the API. If the SeriesGraph title differs significantly from the database entry, the data may not load.
* **Database Coverage:** The script currently only supports anime present in the [Chaiwala Anime API](https://filler-list.chaiwala-anime.workers.dev/). If an anime is missing, no markers will appear.
* **Alignment (Z-Index/Positioning):** Because markers are absolute-positioned overlays, they may occasionally shift if the graph scales after the script runs. 
    * **Fix:** Use the **"Remark Fillers"** button to re-sync the overlays to the current bar positions.
* **"BLOCKED BY CLIENT" error in console:** Some ad-blockers prevent the request sent to external API's. 
    * **Fix:** White list "filler-list.chaiwala-anime.workers.dev" in your ad-blocker.

## 📸 Screenshots
### Bleach:
<img width="1188" height="897" alt="image" src="https://github.com/user-attachments/assets/798c300e-1ca0-46e7-ba46-99c0ecd3ce24" />

### Naruto Shippūden:
<img width="982" height="878" alt="image" src="https://github.com/user-attachments/assets/2e53dc79-8274-413f-9b40-5d9530191b0c" />


## 📜 License
MIT
