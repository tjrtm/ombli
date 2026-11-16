# Ombli

A glimmerling that lives in a web page: a small glowing creature you can chat with.

**Meet it: https://tjrtm.github.io/ombli/**

Ombli's mind is [PyBot](https://github.com/huggingface/pybot), a chatbot made of about 640,000 lines of hand-written Python rules, with no AI model and no weights. The whole thing runs in your browser tab through [Pyodide](https://pyodide.org) (Python compiled to WebAssembly), so nothing you type leaves your device.

## What is in here

`index.html` is the entire project: the page, the creature, Python itself and PyBot's code, bundled into one self-contained file of about 17 MB. It makes no network requests after it loads.

## Run it yourself

Serve the file from any web server, for example:

```sh
python3 -m http.server 8080
```

Then open `http://localhost:8080/`. The first visit compiles PyBot once, which takes a little while; later visits are faster. It needs a browser with WebAssembly, so it will not run in sandboxed previews that block it.

## What Ombli can and cannot do

It is good at everyday questions: cooking, cleaning, household fixes, simple maths, and remembering what you told it during the chat. It cannot write essays, poems or code, has no live data, and knows nothing outside its rule tables.

Every answer is PyBot's own text, unchanged. Only the wake-up greeting and the small gestures shown above replies were added on top.

## Credits and licenses

- PyBot © Hugging Face, [Apache License 2.0](LICENSE). Its code is bundled with comments and docstrings removed; its logic is unchanged.
- Pyodide 314.0.8, [Mozilla Public License 2.0](https://github.com/pyodide/pyodide/blob/main/LICENSE), which includes CPython under the [PSF License](https://docs.python.org/3/license.html).
- The Ombli page and creature are released under the Apache License 2.0.

See [NOTICE](NOTICE) for details.
