# Fragment Atlas v0.33

A browser-based viewer for GraphML molecular networks and tandem mass spectra. Open a network, link an MGF file, compare fragment peaks, and write node annotations back to GraphML.

## Quick start

From the folder containing `index.html`, run:

```sh
python3 -m http.server 8000 --bind 127.0.0.1
```

Open [http://127.0.0.1:8000](http://127.0.0.1:8000) in a current browser. On Windows, you can use `py -m http.server 8000 --bind 127.0.0.1`. Use the local server for the source tree so the browser can load the JavaScript modules and WebAssembly. Python is only used to serve files in this example. The app has no backend, npm install, or API key.

Start with **Load example** or open your own GraphML and companion MGF. Both files are read in the browser. The included example uses fabricated spectra and is intended to demonstrate the controls.

See the [user and publishing manual](MANUAL.md) for the full workflow, troubleshooting, GitHub Pages instructions, and the single-file offline build.

## Third-party code

The bundled [Indigo](https://github.com/epam/Indigo) renderer is from `indigo-ketcher` 1.46.0. Its Apache-2.0 license is in [vendor/indigo/LICENSE](vendor/indigo/LICENSE). The application has no project-wide license in this package. Choose one before inviting others to reuse or redistribute the app.
