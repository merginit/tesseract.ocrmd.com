Tesseract.js Core (WebAssembly builds)
======================================

Prebuilt, CDN‑ready Tesseract.js core WebAssembly bundles for browser and Node. Use these files directly without compiling Tesseract yourself.

**What’s here**
- WebAssembly builds and JS wrappers: baseline, SIMD, LSTM, and SIMD+LSTM variants.
- A minified worker script compatible with `tesseract.js`.
- Static hosting‑friendly headers for secure CDN usage.

**Files**
- `tesseract-core.wasm.js`, `tesseract-core.wasm`
- `tesseract-core-simd.wasm.js`, `tesseract-core-simd.wasm`
- `tesseract-core-lstm.wasm.js`, `tesseract-core-lstm.wasm`
- `tesseract-core-simd-lstm.wasm.js`, `tesseract-core-simd-lstm.wasm`
- `tesseract-worker.min.js`

Quick Start — Browser (CDN)
---------------------------
```js
import Tesseract from 'tesseract.js';

const worker = await Tesseract.createWorker({
  corePath: 'https://tesseract.ocrmd.com/tesseract-core-simd.wasm.js',
  workerPath: 'https://tesseract.ocrmd.com/tesseract-worker.min.js',
});

await worker.load();
await worker.loadLanguage('eng');
await worker.initialize('eng');
const { data: { text } } = await worker.recognize(imageElementOrBlobOrUrl);
await worker.terminate();
```

Quick Start — Node (npm)
------------------------
```bash
npm i tesseract.js tesseract.js-core
```

```js
const Tesseract = require('tesseract.js');

const worker = await Tesseract.createWorker({
  corePath: require.resolve('tesseract.js-core/tesseract-core-simd.wasm.js'),
});

await worker.load();
await worker.loadLanguage('eng');
await worker.initialize('eng');
const { data: { text } } = await worker.recognize('/path/to/image.png');
await worker.terminate();
```

Build Variants
--------------
- `tesseract-core.wasm.js` — baseline WebAssembly build.
- `tesseract-core-simd.wasm.js` — SIMD‑optimized; fastest where supported.
- `tesseract-core-lstm.wasm.js` — accuracy‑focused LSTM build.
- `tesseract-core-simd-lstm.wasm.js` — best accuracy + speed; requires SIMD support.

Recommendation: prefer `*-simd.wasm.js` in modern browsers. Feature‑detect SIMD and fall back to baseline when unavailable.

Compatibility & Requirements
----------------------------
- WebAssembly required; asm.js fallback is not provided in this distribution.
- Works best with `tesseract.js` v5+.
- SIMD support varies by browser/CPU.

Hosting, CORS & CSP
-------------------
When using files from `https://tesseract.ocrmd.com`, response headers enforce strict security policies:
- CORS allows `https://ocrmd.com`.
- CSP restricts `worker-src`, `script-src`, `connect-src`, etc. Ensure your app’s CSP permits loading the worker and WASM from your chosen origin.

Troubleshooting
---------------
- 404 or `TypeError: WebAssembly`: use the `*.wasm.js` wrapper, not the raw `*.wasm`.
- CORS/CSP errors: align your site’s CSP with hosting policies.
- Empty OCR results: verify language initialization and image quality.

License & Credits
-----------------
- License: Apache‑2.0 (see `LICENSE.txt`).
- Upstream source: https://github.com/naptha/tesseract.js-core
