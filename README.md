# signal-map-models

Offline speech-recognition engine for Signal Map's auto-subtitles, served
through GitHub Pages so the apps can fetch it on demand (with CORS). Nothing
here is bundled into the app; it is downloaded the first time a user turns
captions on and kept in the browser's Cache Storage.

Built from [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx) v1.13.7
(`sherpa-onnx-wasm-simd-v1.13.7-en-asr-zipformer`, Apache-2.0), repacked
with smaller int8 models:

| dir | model | data | use |
|---|---|---|---|
| `v1/en/` | `sherpa-onnx-streaming-zipformer-en-2023-06-26` int8 | 70 MB | default |
| `v1/en-small/` | `sherpa-onnx-streaming-zipformer-en-20M-2023-02-17` int8 | 35 MB | devices with ≤ 2 GB RAM, or fallback |

Each dir holds the same four files: `sherpa-onnx-wasm-main-asr.{js,wasm,data}`
and `sherpa-onnx-asr.js`. The app loads them by `<base>/<dir>/<file>`.

Versions (clients cache by URL, so a changed bundle gets a new directory):

- `v2/` — wasm memory limits patched to initial 128 MB / max 1 GB. The stock build
  declares 512 MB up front, which a 2 GB TV cannot allocate (`WebAssembly.instantiate(): Out of memory`).
- `v1/` — stock sherpa-onnx memory limits; kept for reference.
