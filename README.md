# Switching to Modern Standard Arabic: an in-car voice assistant prototype

**README prose word count: 824** (headings, tables, links and code not counted)

**Live prototype:** https://rashaalqahtani.github.io/msa-switch-prototype/ (open in Chrome; allow the microphone)

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
| Assistant replies, the dialect drift, the slip, the jump to the "next day", and her scripted lines (her opening questions and the reminder “I told you, MSA!”) | Simulated (written in advance) |
| Fallback buttons | Placeholder: they simulate her speech when the microphone cannot work |
| Replies written by a language model in her chosen style | Planned |
| Automatic detection of dialect drift before a reply is spoken | Planned |
| A control to make the assistant forget her saved choices | Planned |

The system diagram is in `system-diagram.png`.

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

1. **Comparison moved out of the drive, and made blind.** Comparing replies while driving is unsafe, so it became a separate tool used while parked; style names are shown only after each choice so they do not bias it.
2. **Decline turn with three options** (try MSA again, continue, English). My original problem was an assistant that claimed to use MSA when it did not; this turn states the limit instead.
3. **The next day follows her choice.** Testing showed it always started in MSA, even after English. Now, after "Try MSA again" it continues in MSA without asking; otherwise it asks, with buttons that match what happened ("Continue in English", "Start in MSA"). Checked on every next-day path.
4. **Apology as default, worded clearly.** Both groups in Alghamdi et al. (2025) preferred apologies. «عذرًا» can mean "excuse me", so it became «أعتذر عن ذلك».
5. **No promises it cannot keep.** The confirmation reply said it would use MSA "from now on", then slipped. Removed; replies now say "if I slip again, please tell me".
6. **Style change by one spoken sentence**, mentioned once as its own short turn after the answer. Its example is the opposite of the style she just heard (after an apology: "don't apologise").
7. **Every drive answer can be spoken**; buttons are a fallback. "Continue" was ambiguous after the repair, so the option became "Keep this style" («نفس الطريقة»).
8. **Honest labels.** Scripted parts (the drift, her opening questions and reminder, the next day) and the placeholder buttons are labelled; a "What is real?" panel was added.
9. **Feminine Arabic forms** throughout, because the driver is a woman.
10. **Play always speaks** in the research comparison, even in Fast mode, because pressing it is a request to hear the reply.
11. **Found by testing with a real microphone** (commit d9edeae). A repeated speech result made buttons disappear, so each question accepts one answer; every step got the same controls; Safari stopped listening, so the page now recommends Chrome; a new style is now used in the same drive.
12. **Logic and consistency checks.** Re-reading every reply and checking the report, code and figures against each other led to fixes such as: the drift now answers her question in dialect, and the Decline choice is read back from storage.

All 25 drive paths were run automatically, and the expected lines were checked on the live site with voice answers simulated.

## AI assistance

- **My own work:** I designed the interaction, the interface (the car scene, the phone screens and every voice step) and the system diagram (its components, flow, labels and layout). I tested each version and decided every change (examples: the next-day bug, the "from now on" promise, the feminine forms, moving the comparison out of the drive, the default style).
- **Claude (Anthropic)** wrote the code from my instructions.
- **Claude** also helped me turn my own notes into academic English for the report. I designed the storyboards (which moments to show and what each frame says) and gave Claude the content; Claude captured the screenshots from the live prototype and arranged them as I specified.
- **Gemini (Google)** was used only as a checklist: I asked it whether I had followed the steps the brief requires or had missed anything.
- I checked every Arabic line, the labels for implemented, simulated and planned parts, and the claims about the literature myself.

## Additional files

- `system-diagram.png`: system diagram used in the report.
- No ZIP file is needed.
