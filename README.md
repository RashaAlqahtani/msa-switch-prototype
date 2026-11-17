# Switching to Modern Standard Arabic: an in-car voice assistant prototype

**README prose word count: 667** (headings, tables, links and code not counted)

**Live prototype:** LIVE_LINK (open in Chrome; allow the microphone)

A driver talks hands-free to a voice assistant on a phone mount. The assistant drifts into Egyptian dialect; she asks for Modern Standard Arabic (MSA) by voice; it switches back with a short acknowledgement, admits when it cannot keep to MSA, and remembers her choices.

## Project status

| Part | Status |
|---|---|
| Voice input (speech-to-text, Arabic or English) | Implemented |
| Spoken replies (text-to-speech, Arabic and English voices) | Implemented |
| Understanding her request (keyword rules: MSA request, style change, Decline choice, language) | Implemented |
| Dialogue flow: drift → request → repair → slip → Decline turn → next day | Implemented |
| Changing the acknowledgement style by voice, saved in the browser | Implemented |
| Research comparison: 6 blind pairs of the 4 styles, JSON export | Implemented |
| Assistant replies, the dialect drift, the slip and the jump to the "next day" | Simulated (written in advance) |
| Fallback buttons | Placeholder: they simulate her speech when the microphone cannot work |
| Replies written by a language model in her chosen style | Planned |
| Automatic detection of dialect drift before a reply is spoken | Planned |

The system diagram is in `figures/system-diagram.png`.

## Technology stack

- **HTML, CSS and JavaScript** in one file (`index.html`), with no build step and no server code, so it can run from GitHub Pages.
- **Web Speech API** (built into the browser): `SpeechRecognition` turns her voice into text; `speechSynthesis` reads replies aloud. Both are AI models supplied by the browser and operating system; they were not trained or changed in this project.
- **localStorage** stores her preferred style and her last Decline choice on the device.
- No API keys are used or stored. A language model is planned but not connected.

## Running or inspecting the work

1. Open the live link in **Chrome** and allow the microphone. Speech input needs an https page; it does not work when `index.html` is opened as a local file.
2. Without a microphone, everything can still be tried with the fallback buttons.
3. The page has a **Reviewer guide (about 2 minutes)** and a **What is real?** panel at the top. Tick **Fast review** to skip voices and pauses, or add `?fast=1` to the link.
4. To run locally: download the repository and open `index.html` in a browser (buttons only), or serve the folder over https.

## Development record

Main changes, why they were made and what was checked:

1. **Comparison moved out of the drive.** At first the six comparisons happened during the drive. Asking a driver to compare replies is unsafe and slow, so the drive became the product and the comparison became a separate research tool used while parked.
2. **Labels hidden while choosing, shown after each choice**, so style names such as "Explanation" do not bias the choice.
3. **Decline turn with three options** (try MSA again, continue, switch to English). My original problem was an assistant that claimed to use MSA when it did not; the Decline turn states the limit honestly instead.
4. **Next day follows her actual choice.** While testing I found the next day always started in MSA even after choosing English. Fixed, and she now chooses the language by voice.
5. **No promises the assistant cannot keep.** The confirmation reply said it would use MSA "from now on", then the assistant slipped. Removed; later replies say "if I slip, tell me".
6. **Default style changed from Confirmation to Apology**, because both groups in Alghamdi et al. (2025) preferred apologies.
7. **Style change by one spoken sentence** ("don't apologise") instead of a long comparison screen, because a driver needs zero set-up. The assistant mentions this option once only.
8. **Every answer can be spoken**, not only the first request; buttons are a fallback.
9. **Honest labels**: the drift, the next day and the buttons are labelled as simulated or placeholder; a "What is real?" panel and a reviewer guide were added.
10. **Feminine Arabic forms** throughout, because the driver is a woman.

All six drive paths (3 Decline choices × 2 next-day choices), voice answers (with a mocked recogniser) and the research comparison were clicked through on a local server with no errors.

## AI assistance

- **Claude (Anthropic)** wrote most of the code and drew the system diagram from my design decisions. I tested each version, reported problems and decided the changes (examples: the next-day bug, the "from now on" promise, the feminine forms, moving the comparison out of the drive, the default style).
- **Gemini (Google)** was asked to review the prototype against the brief and rubric. I did not follow all of its advice: it wrongly said the comparison still happened while driving and misreported the evidence for the default style, so I checked its points against my Assignment 1 sources.
- I checked every Arabic line, the labels for implemented, simulated and planned parts, and the claims about the literature myself.

## Additional files

- `figures/system-diagram.png`: system diagram used in the report.
- No ZIP file is needed.
