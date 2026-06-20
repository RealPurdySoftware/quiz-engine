# Quiz Engine

A single-file, no-build, no-server quiz app. Open it in a browser and take a quiz with instant feedback after every answer. Drop in your own `.json` quiz files to study anything — built for exam prep, but the format is generic enough for any subject.

No installation, no dependencies, no backend, no tracking. Everything runs client-side; quiz files are read directly from your file system via the browser's File API and never leave your machine.

![status](https://img.shields.io/badge/build-none--needed-brightgreen) ![deps](https://img.shields.io/badge/dependencies-zero-blue)

## Features

- ✅ Multiple choice questions with instant right/wrong feedback and an explanation after every answer
- 📊 Live score tracking and a per-question progress tracker
- 📂 Load any quiz by selecting a local `.json` file — no upload, no server round-trip
- 🔁 Retake the current quiz or swap in a different one at any time
- ♿ Keyboard navigable (Enter to confirm/advance), visible focus states, respects `prefers-reduced-motion`
- 📱 Responsive down to mobile
- 🧩 Plain HTML/CSS/JS — no React, no npm install, no build step

## Getting started

1. Clone this repo or download index.html.
2. Open `index.html` directly in a browser (double-click it, or right-click → Open With → your browser).

That's it. The app launches with a built-in sample quiz, no setup required.

> **Note:** `index.html` links to Google Fonts on first load for the display typeface. If you're fully offline, it gracefully falls back to your system fonts — nothing breaks.

## Generating quizzes with an LLM

[llm-instructions.md](./llm-instructions.md) is written to be handed directly to an LLM (Claude, ChatGPT, etc.) along with your source material — notes, a textbook chapter, a study guide. Paste both into a conversation and ask it to generate a quiz; the file includes the schema, content rules, and a ready-to-use prompt template. Save the model's output as a `.json` file and load it directly — no reformatting needed.  Place the .json file in the same folder as index.html

## Loading your own quiz

Click **Load Quiz File** in the app and select any `.json` file that follows the format documented in [llm-instructions.md](./llm-instructions.md). `sample-quiz-sections-1-3.json` in this repo is a working example you can use to confirm everything's working before trying your own.

## Quiz file format

Quiz files are plain JSON. Full schema, field-by-field requirements, validation rules, and examples are documented in **[llm-instructions.md](./llm-instructions.md)**.


## Project structure

```
.
├── index.html                       # the app — open this in a browser
├── sample-quiz-sections-1-3.json    # example quiz file / format reference
├── llm-instructions.md              # quiz JSON schema + LLM generation guide
├── LICENSE                          # MIT license
└── README.md
```

## Browser support

Any modern evergreen browser (Chrome, Firefox, Safari, Edge). Uses standard File/FileReader APIs — no polyfills needed.

## Privacy

There is no backend. Quiz files you load are read locally via the browser's File API and never transmitted anywhere. The only network request the app makes is fetching the display font from Google Fonts on first load, which can be removed by deleting the `<link>` tags in `index.html` if you'd rather run fully offline.

## Contributing

Issues and pull requests welcome — this is intentionally a small, dependency-free tool, so the bar for new features is "does this stay simple enough to run by double-clicking an HTML file."

## Disclaimer

This is a generic, subject-agnostic quiz engine. It is not created by, affiliated with, endorsed by, or sponsored by any certification body, publisher, or educational organization (including the National Academy of Sports Medicine / NASM, despite this repo's earlier name). Any such names are trademarks of their respective owners and are not used to imply association. The app itself contains no copyrighted exam content — it only renders quiz files you supply. Any quiz content you create or load is your own responsibility; do not use this tool to reproduce or distribute copyrighted exam material. This software is provided "as is," without warranty of any kind, and is not a substitute for official study materials from any certifying organization.

## License

MIT — see [LICENSE](./LICENSE).
