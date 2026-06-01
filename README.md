# Spelling Practice App — Build Prompt

Build a spelling practice web app as a single `index.html` file.

## Core features

A list of vocabulary words, each displayed as a card with:

- A large speaker button (🔊) that reads the word aloud using the Web Speech API (`SpeechSynthesisUtterance`, rate 0.85)
- The word broken into syllables displayed with bullet dots between them (e.g. `ren • ais • sance`)
- A small speaker button next to the syllables that reads each syllable aloud with pauses
- The word's definition in italics with a small speaker button to read it aloud
- A "How to remember it" spelling tip section with a small speaker button to read it aloud
- A text input where the user types the word, a Check button (also triggered by Enter), and a Clear button
- Green feedback on correct spelling, red feedback with the user's attempt shown on incorrect

A floating "+ Add Word" button (fixed, bottom-right) that opens a modal form with:

- A Word field with a "Look up" button that fetches from `https://api.dictionaryapi.dev/api/v2/entries/en/{word}` — auto-fills the definition (prefixed with part of speech) and syllables (from phonetic dot-notation if available, otherwise a basic vowel-pattern syllable splitter)
- A Syllables field (comma-separated, editable)
- A Definition textarea
- A "How to remember it" textarea
- A Save Word button and a Cancel button (all modal buttons must have `type="button"`)
- Inline error if word or syllables are empty

## Storage — Google Apps Script + Google Sheets

Custom words are stored in and loaded from a **Google Apps Script web app** (URL configured in a `const SCRIPT_URL = ''` at the top of the script).

The Apps Script (`Code.gs`, separate file) handles:

- `GET ?action=list` → returns array of word objects as JSON
- `POST {action:"add", word, syllables (pipe-separated string), definition, tip}` → appends a row
- `POST {action:"delete", word}` → removes matching rows

Sheet columns: `word | syllables | definition | tip`, first row frozen as header, sheet named "Words", auto-created if missing. Responds with `ContentService` JSON. Deploy as **Execute as Me / Anyone**.

## Additional behavior

- Custom word cards show an `×` delete button in the header; clicking it confirms then calls the delete endpoint and re-renders
- A small `#pageStatus` paragraph shows "Loading…" while fetching and clears when done; if `SCRIPT_URL` is empty the word list shows a configure prompt instead

## Built-in words

Two hardcoded words always shown (not deletable): **renaissance** and **Michelangelo**, each with full syllables, definition, and spelling tip.

## Design

Clean serif (`Georgia`) layout on a warm off-white (`#f9f7f4`) background, blue accent (`#4a6fa5`), card-based, responsive. Modal with backdrop. No external CSS frameworks or JS libraries.
