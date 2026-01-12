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
The script interacts with a custom worker API (`filler-list.chaiwala-anime.workers.dev`) to retrieve episode lists. It then matches those episode numbers against the `<rect>` elements (SVG) used by SeriesGraph to generate the episode rating bars.

## ⚠️ Known Issues / Limitations
* **Slug Matching:** The script relies on "skewer-case" titles (e.g., `naruto-shippuden`) to match the API. If the SeriesGraph title differs significantly from the database entry, the data may not load.
* **Database Coverage:** The script currently only supports anime present in the [Chaiwala Anime API](https://filler-list.chaiwala-anime.workers.dev/). If an anime is missing, no markers will appear.
* **Alignment (Z-Index/Positioning):** Because markers are absolute-positioned overlays, they may occasionally shift if the graph scales after the script runs. 
    * **Fix:** Use the **"Remark Fillers"** button to re-sync the overlays to the current bar positions.
## 📜 License
MIT
