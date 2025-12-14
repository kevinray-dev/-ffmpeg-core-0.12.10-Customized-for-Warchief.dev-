Warcut Studio + FFmpeg.wasm Assets (Chunked WASM)
================================================

GitHub description (suggested)
-----------------------------
Browser-based FFmpeg.wasm demo + runtime assets, with a cloud-friendly “chunked” ffmpeg-core.wasm loader.

What this repo contains
-----------------------
- Warcut_Studio_PROMO_FFmpeg_Fixed_v14_MP4_LowerThirds_BurnIn_TSFix.html
  A standalone HTML app/mockup that loads FFmpeg.wasm in the browser (via a Web Worker),
  includes a chunked-WASM loader, and provides a UI for running FFmpeg-style workflows.

- ffmpeg.js
  The FFmpeg wrapper/bundle that exposes the FFmpeg API used by the HTML app.

- worker.js
  The worker runtime that loads the FFmpeg core and handles commands (LOAD/EXEC/FFPROBE and FS ops),
  posting back logs/progress.

- ffmpeg-core.js / ffmpeg-core.min.js
  The FFmpeg “core” (Emscripten build) that the worker loads.

- ffmpeg-core.wasm.part00 .. ffmpeg-core.wasm.part03
  The FFmpeg WebAssembly binary split into multiple pieces to make hosting easier on platforms
  with per-file size limits.

Quick start
-----------
1) Serve these files from a local/static web server (NOT file://).
   Examples:
   - python -m http.server 8080
   - npx serve

2) Keep the expected paths consistent.
   The HTML is set up to fetch the WASM parts from a folder like:
     /ffmpeg/ffmpeg-core.wasm.part00
     /ffmpeg/ffmpeg-core.wasm.part01
     /ffmpeg/ffmpeg-core.wasm.part02
     /ffmpeg/ffmpeg-core.wasm.part03

   If you place them elsewhere, update the __WASM_PART_URLS list in the HTML.

3) Open the HTML page in your browser from the server URL:
   http://localhost:8080/Warcut_Studio_PROMO_FFmpeg_Fixed_v14_MP4_LowerThirds_BurnIn_TSFix.html

How the “chunked WASM” works
----------------------------
The app fetches each ffmpeg-core.wasm.partXX file, concatenates them into a single Uint8Array,
then creates a Blob URL (application/wasm) and points the FFmpeg core loader at that Blob URL.

This approach is useful when:
- Your host/CDN has maximum file-size limits
- You want more flexible caching or incremental downloads

Attribution (Chunked WASM)
--------------------------
The idea/approach of splitting the WASM into cloud-friendly chunks and reassembling at runtime
is attributed to: warchief.dev

Notes & troubleshooting
-----------------------
- You must run from http(s). Workers + fetch won’t work reliably from file://.
- Same-origin hosting is simplest (CORS can break worker/core/wasm loading).
- If FFmpeg fails to load, confirm:
  - The worker script path is correct
  - The core .js file path is correct
  - All WASM parts are reachable (no 404s)

Licensing reminder
------------------
FFmpeg, ffmpeg.wasm, and related npm packages have their own licenses and compliance requirements.
Make sure you review and comply with upstream licensing for your distribution and use-case.

