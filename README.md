# Ombli

A glimmerling that lives in a web page: a small glowing creature you can chat with.

**Meet it: https://tjrtm.github.io/ombli/**

Ombli's mind is [PyBot](https://github.com/huggingface/pybot), a chatbot made of about 640,000 lines of hand-written Python rules, with no AI model and no weights. The whole thing runs in your browser tab through [Pyodide](https://pyodide.org) (Python compiled to WebAssembly), so nothing you type leaves your device.

## What is in here

`index.html` is the whole chat: the page, the creature, Python itself and PyBot's code, bundled into one self-contained file of about 17 MB. It makes no network requests after it loads, unless you turn on the voice.

`voice/` holds Ombli's optional voice model (see below).

## Run it yourself

Serve the file from any web server, for example:

```sh
python3 -m http.server 8080
```

Then open `http://localhost:8080/`. The first visit compiles PyBot once, which takes a little while; later visits are faster. It needs a browser with WebAssembly, so it will not run in sandboxed previews that block it.

## Ways to keep Ombli

The ✦ button in the header switches between modes. Each one is part of the same page and works for anybody with a browser.

| Mode | Link | What it does |
| --- | --- | --- |
| Chat | `/` | Just Ombli and you. |
| Kitchen sprite | `?mode=kitchen` | Big text, the screen stays on, and the chat tidies itself after three quiet minutes. For a tablet on the counter or in a guest room. |
| Desk pet | `?mode=pet` | Only the creature. It asks itself a question now and then, naps after five minutes without a tap, and counts boops. |
| Rule-scroll safari | `?mode=safari` | Shows which of PyBot's modules answered and collects the ones you have found. |
| Stump the glimmerling | `?mode=stump` | A game: you score when Ombli has to shrug, Ombli scores when it has an answer. First to five. |
| Guess the rule | `?mode=lesson` | For classrooms: the module behind each reply stays hidden until you guess and tap. |

Links can carry more:

- `?q=How long should I boil an egg?` opens Ombli already answering that question. Put such a link in a QR code or NFC tag on the kettle or the washing machine.
- `?sparks=Question one|Question two` replaces the suggested questions, for example for guests.
- "Copy a link" in the ✦ menu builds a link to the current mode and your last question.
- "Save for offline" downloads the page as `ombli.html`, which opens without a connection.

The About sheet counts the network requests made since Ombli woke, so you can watch it stay at zero and keep chatting in airplane mode.

`404.html` is a small lost-page screen for static hosts such as GitHub Pages. It sends visitors to Ombli with `?lost=1`, which changes the greeting.

## Ombli's voice

The speaker button in the header gives Ombli a voice. It is [Piper](https://github.com/rhasspy/piper), a small neural text-to-speech model, running in a Web Worker inside the tab. Nothing you type or Ombli says is sent anywhere: the only network traffic is the one-time download of the voice files, about 45 MB:

| File | From | Size (transferred) |
| --- | --- | --- |
| `voice/en_US-kristin-medium.fp16.onnx` | this repo | 32 MB |
| espeak-ng pronunciation data and phonemizer ([`@diffusionstudio/piper-wasm`](https://www.npmjs.com/package/@diffusionstudio/piper-wasm) 1.0.0) | jsDelivr | about 9.5 MB |
| [ONNX Runtime Web](https://onnxruntime.ai) 1.30.0 | cdnjs | about 2.5 MB |

The files are kept in the browser's Cache Storage, so later visits wake the voice without downloading anything. The About sheet counts the voice downloads separately from everything else.

The cute part is a pitch trick: Piper is asked to speak slower by some factor, and the audio is then played faster by the same factor, which raises the pitch and formants without changing the speed. The three styles use different factors: Glimmer (×1.32), Sprout (×1.55) and Dusk (×1.12).

The voice is the [`kristin` medium voice](https://huggingface.co/rhasspy/piper-voices/tree/main/en/en_US/kristin/medium) by Bryce Beattie, trained on public-domain LibriVox recordings. The copy in `voice/` stores its weights as 16-bit floats, which halves the download; they are converted back to 32-bit when the model loads, and its output matches the original. If `voice/` is missing, for example when the page is saved and opened as a file, it falls back to the original 63 MB model on Hugging Face.

## What Ombli can and cannot do

It is good at everyday questions: cooking, cleaning, household fixes, simple maths, and remembering what you told it during the chat. It cannot write essays, poems or code, has no live data, and knows nothing outside its rule tables.

Every answer is PyBot's own text, unchanged. Only the wake-up greeting, the small gestures shown above replies and the game notes were added on top.

## Credits and licenses

- PyBot © Hugging Face, [Apache License 2.0](LICENSE). Its code is bundled with comments and docstrings removed; its logic is unchanged.
- Pyodide 314.0.8, [Mozilla Public License 2.0](https://github.com/pyodide/pyodide/blob/main/LICENSE), which includes CPython under the [PSF License](https://docs.python.org/3/license.html).
- The Ombli page and creature are released under the Apache License 2.0.
- Voice (optional, downloaded only when switched on): Piper and the `kristin` voice (MIT, from [rhasspy/piper-voices](https://huggingface.co/rhasspy/piper-voices); trained on public-domain LibriVox recordings), ONNX Runtime Web (MIT), and the piper-phonemize build of [espeak-ng](https://github.com/espeak-ng/espeak-ng) (GPL-3.0), which is loaded from jsDelivr and is not included in this repository.

See [NOTICE](NOTICE) for details.
