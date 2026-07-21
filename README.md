# How Tech Works

🌐 **English** | [Bahasa Indonesia](README.id.md)

**🔗 Live demo: [yogawa.github.io/how-tech-works](https://yogawa.github.io/how-tech-works/)**

A collection of interactive visualizations that explain **how technology works** — networking, systems, security, and other topics — in plain language, step by step, so beginners can actually follow along.

Every topic is an interactive page with built-in **multi-language support**: translated text lives in JSON files separate from the code, so contributors can add a new language without touching any HTML/JavaScript.

## Running it

This project loads translated text via `fetch()`, so it **needs to run through a local server** rather than being opened directly as a file (`file://`) — browsers block `fetch` requests to local JSON files under the `file://` protocol due to CORS restrictions.

Pick one:

- **VS Code**: install the [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) extension, right-click `index.html` → "Open with Live Server".
- **Python**: `python -m http.server 8000`, then open `http://localhost:8000`.
- **Node.js**: `npx serve .`, then open the URL it prints.

For a hosted version, enable **GitHub Pages** in the repository settings (Settings → Pages → Deploy from branch `main`, folder `/`), then open `https://<username>.github.io/<repo-name>/`. This project's own live deployment is at [yogawa.github.io/how-tech-works](https://yogawa.github.io/how-tech-works/).

## Project structure

```
.
├── index.html                    # Main page: topic sidebar + content area (iframe)
├── README.md
├── README.id.md                  # Indonesian translation of this file
├── i18n/                         # Translations for index.html's UI chrome (sidebar, empty state, etc.)
│   ├── id.json
│   └── en.json
└── topics/
    ├── tcp-ip-layers.html        # A packet's journey through TCP/IP (encapsulation/decapsulation)
    ├── dns.html                  # How domain names get resolved into IP addresses
    ├── asymmetric-encryption.html # Public/private key pairs — encryption & digital signatures
    ├── blockchain.html           # Hash-linked blocks and network consensus
    ├── multithreading.html       # 4 scenarios — race condition, mutex lock, deadlock, thread pool
    ├── docker-vs-vm.html         # Containers vs. traditional virtual machines
    ├── git.html                  # Git: 5 scenarios — commit workflow, branching, conflicts, reset vs revert, rebase vs merge
    ├── https-tls.html            # HTTPS/TLS: 4 scenarios — handshake, certificate chain of trust, TLS 1.2 vs 1.3, MITM attack
    ├── event-driven-architecture.html # EDA: 4 scenarios — pub/sub, event queues, choreography vs orchestration, resilience
    ├── llm.html                   # 5 scenarios — inference pipeline, generation loop, training, context window, hallucination
    ├── aot-vs-jit.html            # AOT vs JIT: compile pipelines, startup/warm-up, adaptive optimization
    └── i18n/                     # Per-topic content translations, one file per language
        ├── tcp-ip-layers.id.json / .en.json
        ├── dns.id.json / .en.json
        ├── asymmetric-encryption.id.json / .en.json
        ├── blockchain.id.json / .en.json
        ├── multithreading.id.json / .en.json
        ├── docker-vs-vm.id.json / .en.json
        ├── git.id.json / .en.json
        ├── https-tls.id.json / .en.json
        ├── event-driven-architecture.id.json / .en.json
        ├── llm.id.json / .en.json
        └── aot-vs-jit.id.json / .en.json
```

- `index.html` holds the sidebar used to pick a topic. Clicking one loads the matching HTML page into the area on the right via `<iframe>`, with the active language passed along through a query string (`?lang=en`).
- Every file in `topics/` is a **self-contained** HTML page — its CSS and JavaScript live inside that one file, independent of `index.html`. Each can also be opened standalone (through a local server), complete with its own language toggle.
- All user-facing text (titles, descriptions, button labels, table contents, etc.) comes from the JSON files in `i18n/`, **not** hardcoded into HTML/JS. That's what makes it translatable without touching the code.

## How the multi-language setup works

- The selected language is stored in the browser's `localStorage`, so it persists across reloads.
- `index.html` loads `i18n/<language-code>.json` for chrome text (sidebar, topic titles in the list, etc.).
- Each topic page loads `topics/i18n/<topic-name>.<language-code>.json` for all of its interactive content.
- When the language is switched from the sidebar, `index.html` appends `?lang=<code>` to the iframe's URL and reloads it. When the language is switched **from inside** a topic page (the ID/EN toggle in its top-right corner), that page sends a message (`postMessage`) back to `index.html` so the sidebar stays in sync without triggering a second iframe reload.

## Topic list

- [x] TCP/IP — a packet's journey from point to point (encapsulation & decapsulation across layers)
- [x] DNS — how domain names get translated into IP addresses
- [x] Asymmetric encryption — public/private key pairs, used both for encrypting secrets and for digital signatures
- [x] Blockchain — hash-linked blocks and network consensus
- [x] Multithreading — four scenarios: a race condition, a mutex lock fix, a deadlock, and why real programs use a thread pool
- [x] Docker/containers vs. virtual machines
- [x] Git — five scenarios: basic commit workflow, branching & merging, merge conflicts, reset vs. revert, and rebase vs. merge
- [x] HTTPS/TLS — four scenarios: the full TLS handshake, the certificate chain of trust, TLS 1.2 vs. 1.3, and a man-in-the-middle attack simulation
- [x] Event-Driven Architecture — four scenarios: the basic publish/subscribe pattern, event queues for async processing, choreography vs. orchestration, and resilience to partial failure
- [x] LLM — five scenarios: the inference pipeline, the autoregressive generation loop, training, the context window limit, and hallucination
- [x] AOT vs. JIT compilers — four scenarios: the AOT pipeline, the JIT pipeline, startup time & warm-up, and adaptive optimization

Every topic from the original backlog is now covered — new topic ideas and contributions are still very welcome.

## Adding a new topic

1. Create a new HTML file inside the `topics/` folder. Use any existing page (e.g. `topics/tcp-ip-layers.html` or `topics/asymmetric-encryption.html`) as a structural reference: DOM elements get an empty `id`, then get filled in via JavaScript from a JSON file — not written as static text directly in the HTML. Each topic is free to invent its own palette, fonts and visual identity (every topic so far looks different on purpose) — the only shared contract is the `?lang=` query param and the `postMessage` language-sync handshake described below. If a topic covers several distinct scenarios instead of one linear walkthrough (see `topics/blockchain.html` or `topics/git.html`), add a tab switcher above the step viewer, with each tab owning its own step array in the i18n JSON (`i18n.steps.<scenarioKey>[...]`).
2. Create its content translation file(s) at `topics/i18n/<topic-name>.id.json` (and `.en.json`, etc. for every language already supported).
3. Open `index.html`, find the `topics` array near the bottom `<script>` tag, and add a new entry:

   ```js
   {
     key: "newTopicKey",
     file: "topics/file-name.html"
   }
   ```

   Add it under an existing category (`categoryKey`: currently `network`, `security`, `systems`, `tools`, `architecture`, or `ai`), or create a new category object if it belongs to a different area.
4. Add that topic's title & description to **every** file in `i18n/` (e.g. `i18n/id.json` and `i18n/en.json`) under `topics.<key>.title` and `topics.<key>.desc`, and add the new category's label under `categories.<categoryKey>` if it doesn't exist yet.
5. Run it through a local server (see "Running it") and confirm the new topic shows up in the sidebar and opens correctly in every supported language.

## Adding a new language

1. Duplicate `i18n/id.json` into `i18n/<language-code>.json` (e.g. `i18n/ja.json`), then translate its contents.
2. For every existing topic, also duplicate `topics/i18n/<topic-name>.id.json` into `topics/i18n/<topic-name>.<language-code>.json` and translate it. Fields like `sizeNum`, CSS classes, and array structure **don't** need translating — only the text values (`title`, `desc`, `fields`, `sizeUnit`, etc.).
3. Add that language code to the `LANGS` array in `index.html` (and in `topics/*.html` if you also want that toggle available when a topic page is opened standalone).
4. You don't need to translate every topic at once — JSON files that don't exist yet for a new language can simply be added incrementally.

## License

MIT — feel free to use, modify, and share it for learning purposes.
