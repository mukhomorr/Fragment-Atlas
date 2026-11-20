# Fragment Atlas v0.33 manual

Fragment Atlas opens GraphML molecular networks, links fragment spectra from MGF, and lets you inspect or edit node annotations. The app runs in your browser. Use the bundled synthetic network to try the controls before importing your own data.

## 1. Start the app

| Setup | What to do |
| --- | --- |
| Local source | From the folder containing `index.html`, run `python3 -m http.server 8000 --bind 127.0.0.1` and open [http://127.0.0.1:8000](http://127.0.0.1:8000). On Windows, use `py` in place of `python3`. |
| Offline HTML | Build the single HTML file as described below, then open that file directly in a current browser. |
| GitHub Pages | Follow the publishing steps below and open the URL GitHub shows in the repository’s **Pages** settings. |

The source uses JavaScript modules and a WebAssembly renderer. Serve the source over HTTP or HTTPS, or use the bundled HTML for direct file opening. GraphML, MGF, and structure rendering all run locally in the browser. The app sends no imported data to a backend.

## 2. Open a network and link spectra

1. Click **Open GraphML** and choose your network.
2. Click **Import MGF** and select the matching MGF file or files in one import.
3. Check the status below the workspace for the linked-node count.

Fragment Atlas first tries MGF `SCANS` or `FEATURE_ID` against the GraphML scan or feature identifier and node ID, then tries an exact MGF `TITLE` match. If a scan identifier occurs more than once, that scan match is treated as ambiguous. Nodes can also show peaks embedded in GraphML. Importing another MGF selection replaces the current set of imported spectra.

## 3. Explore and arrange the network

| Action | Control |
| --- | --- |
| Inspect a node | Click the node or choose it in the **Spectrum A** menu. |
| Compare two nodes | Click their connecting edge, Shift-click a second node, or choose the nodes in the **Spectrum A** and **Spectrum B** menus. |
| Find a node | Use the network search to find it by ID, scan, or label. |
| Move or group nodes | Drag a node. To move several together, use **Box select**, draw around the nodes, then drag one of the outlined nodes. |
| Refit or rearrange | Use **Fit network** to recenter the view or **Auto arrange** to recompute positions. Drag the background to pan and scroll to zoom. |
| Change labels | Choose an attribute under **Node labels**. Parent mass is selected initially. |
| Color by an attribute | Choose a GraphML column under **Color by** and open **Color legend**. A column of hex colors is used directly. Missing values appear in gray. |

Manual positions last for the current session. GraphML export saves edits to node attributes and preserves the original network topology.

## 4. Inspect and align spectra

The inspector shows each selected node’s precursor m/z, retention time, charge, and library identifiers when available. It displays a mirror plot when two spectra are available.

- Use **Plot height** to resize the plot. On desktop, drag the divider between the network and inspector to change the pane width.
- Drag across an m/z interval to zoom. Use **Full range** to reset it. **Show peak m/z** labels peaks where space permits.
- Set **Peak tolerance** in Da. **Direct m/z** compares fragment masses directly. **Direct + precursor shift** also considers matches after shifting by the precursor m/z difference.
- Blue marks direct matches, red marks shifted matches, and gray marks unmatched peaks. Click a matched peak or use **Matched peak pairs** to see its partner and signed **A − B** difference.
- The similarity score uses the full spectra even while the plot is zoomed. Fragment Atlas calculates its own peak matches and cosine score from the loaded peaks and current settings. GNPS jobs can use different preprocessing and settings.

## 5. Annotate and export nodes

1. Select the node and click **Annotate A** or **Annotate B** above its spectrum.
2. Enter the compound name, InChI, SMILES, library ID, library spectrum ID, or notes.
3. Click **Save annotation** to update the node’s GraphML row. The form reuses compatible existing fields and creates a string column for a new field when needed.
4. Click **Export GraphML** to download an edited copy. Reopen that copy and reimport the MGF to continue a saved session.

For other attributes, use the searchable **GraphML node table**. An InChI beginning with `InChI=1/` or `InChI=1S/` is displayed in the inspector. A valid, supported InChI also produces a 2D depiction. The original identifier stays visible if rendering fails. The depiction uses the bundled Indigo engine and requires no external lookup.

GraphML export does not embed a separately imported MGF. Keep the MGF alongside the edited network.

## 6. Upload to GitHub

Unzip the source archive. Upload the **contents** of the extracted `fragment-atlas-source` folder so `index.html`, `README.md`, and `vendor/` sit at the repository root. The package includes a synthetic example and the renderer assets. Keep any private datasets outside the repository.

For a new, empty GitHub repository, you can push from the extracted folder:

```sh
cd fragment-atlas-source
git init -b main
git add .
git commit -m "Add Fragment Atlas v0.33"
git remote add origin https://github.com/YOUR-ACCOUNT/YOUR-REPOSITORY.git
git push -u origin main
```

To publish the app, open **Settings → Pages**, choose **Deploy from a branch**, select **main** and **/(root)**, then save. Use the published URL shown on that page. The included `.nojekyll` file lets Pages serve the static source directly. [GitHub Pages publishing instructions](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)

## 7. Build the offline HTML

From the repository root, run:

```sh
python3 scripts/build_offline.py
```

Open `build/fragment-atlas-v0.33.html` in a browser. The builder embeds the app, synthetic example, license, and compressed WebAssembly. A modern browser with WebAssembly and `DecompressionStream` support is needed for offline structure rendering. You can pass `--output PATH` to change the destination.

## 8. Source map

| File | Role |
| --- | --- |
| `index.html`, `style.css` | Page structure and appearance |
| `app.js`, `layout.mjs` | Interactive network, layout, and spectrum plots |
| `data.mjs` | GraphML and MGF parsing, metadata, coloring, and peak alignment |
| `workspace.mjs` | Pane resizing, node table, annotation form, and GraphML export |
| `depiction.mjs`, `vendor/indigo/` | Local InChI-to-SVG preview and renderer |
| `demo.mjs` | Synthetic example network and spectra |
| `scripts/build_offline.py` | Single-file offline builder |

## Troubleshooting

| Symptom | Check |
| --- | --- |
| The source page opens but the app does not load | Start the local HTTP server and open the localhost URL. Keep the source files and `vendor/` together. |
| Zero nodes link to spectra | Compare the node IDs and scan attributes with MGF `SCANS`, `FEATURE_ID`, or `TITLE`. Use the MGF exported for that network. |
| No shifted matches appear | Select two spectra with precursor m/z values, choose **Direct + precursor shift**, and check the peak tolerance. |
| The InChI is visible but no structure appears | Check that the identifier is complete, and try a current browser. For a hosted build, confirm the WebAssembly file was uploaded with the rest of `vendor/indigo/`. |
| Edits vanished after closing the tab | Export a GraphML copy before leaving the session and reopen that copy next time. |
