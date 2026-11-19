# Switching to Modern Standard Arabic: an in-car voice assistant prototype

**README prose word count: 1592** (headings, tables, links and code not counted)

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

1. **Comparison moved out of the drive.** At first the six comparisons happened during the drive. Asking a driver to compare replies is unsafe and slow, so the drive became the product and the comparison became a separate research tool used while parked.
2. **Labels hidden while choosing, shown after each choice**, so style names such as "Explanation" do not bias the choice.
3. **Decline turn with three options** (try MSA again, continue, switch to English). My original problem was an assistant that claimed to use MSA when it did not; the Decline turn states the limit honestly instead.
4. **Next day follows her actual choice.** While testing I found the next day always started in MSA even after choosing English. Fixed, and she now chooses the language by voice.
5. **No promises the assistant cannot keep.** The confirmation reply said it would use MSA "from now on", then the assistant slipped. Removed; later replies say "if I slip again, please tell me".
6. **Default style changed from Confirmation to Apology**, because both groups in Alghamdi et al. (2025) preferred apologies.
7. **Style change by one spoken sentence** ("don't apologise") instead of a long comparison screen, because a driver needs zero set-up. The assistant mentions this option once only.
8. **Every answer can be spoken**, not only the first request; buttons are a fallback.
9. **Honest labels**: the drift, the next day and the buttons are labelled as simulated or placeholder; a "What is real?" panel and a reviewer guide were added.
10. **Feminine Arabic forms** throughout, because the driver is a woman.
11. **Found by testing the live site with a real microphone** (commit d9edeae): the end buttons disappeared when the browser delivered the same spoken answer twice, so each question now accepts one answer only; the first voice step looked different from the others, so every step now has the same controls and one Arabic/English switch; Safari stopped listening, so the page now recommends Chrome.
12. **The chosen style is shown and used in the same drive** (commit d9edeae). At first a style change only took effect after a replay; now the assistant gives an example of the new style at once and uses it the next time it returns to MSA.
13. **The hint matches what she just heard.** While reviewing the repair I noticed that after a Confirmation the assistant still said “for example, say: don’t apologise”, although it had not apologised, and the hint was attached to the end of the answer about jobs. The hint is now its own short turn after the answer, and its example is the opposite of the style she just heard (Apology → “don’t apologise”, Just switch → “apologise”, Confirmation → “tell me what happened”, Explanation → “confirm you understood”). Checked for all four styles.
14. **A clearer way to keep the style.** After the repair the main button said “Continue the drive” and the spoken answer was “نكمل”. While testing I found this unclear: it could mean keeping the style or ignoring the hint, and “نكمل” also means “continue as it is” in the Decline turn. The button is now “Keep this style” and she can say “نفس الطريقة” or “خلّه كذا”. Checked by voice: these phrases keep the style, and “نكمل” is no longer an answer after the repair.
15. **Play always speaks in the research comparison.** While trying the comparison I pressed ▶ Play and heard nothing, because Fast mode (meant for reviewers) turns all speech off. Pressing Play is an explicit request to hear a reply, so Play now speaks even in Fast mode, while the drive itself stays silent in Fast mode. Checked with Fast mode on.
16. **A clearer apology.** The Apology opening was “عذرًا على ذلك”. In Arabic “عذرًا” is often used to mean “excuse me”, so it may not be heard as a real apology, which matters when participants compare the styles. It is now “أعتذر عن ذلك” (“I apologise for that”), a first-person apology. Checked in the repair, the retry after the Decline turn and the research comparison.
17. **The next day keeps a clear choice without asking again.** If she had chosen “Try MSA again” in the Decline turn, the next day still asked “MSA or English?”. While testing I found this contradicted the claim that it remembers her choice, and repeated the annoyance of the original problem. Now, after “Try MSA again”, it simply continues in MSA and says so; it still asks after “Continue as it is” or “Switch to English”, where yesterday’s choice was not clear. Checked on all next-day paths.
18. **Next-day buttons say what really happens.** After she had switched to English, the next day offered “Continue in MSA”, although the assistant was speaking English, so nothing in MSA could be continued. The buttons now follow yesterday’s choice: after English, “Continue in English” (also by saying “continue”) or “Try MSA again”; after “Continue as it is”, “Start in MSA” or “Start in English”. The replies match (“I will go back to MSA”, “I will start in MSA”). Checked on every next-day path.
19. **Line-by-line logic check.** I re-read every reply in its context and fixed four mismatches: the drift opened with a greeting (“how are you?”) in the middle of the conversation and did not answer her question, so it now gives the same answer about jobs in Egyptian dialect, and only the variety differs from the MSA repair; “Just switch” said “I will return to MSA” when she asked it to try again, which is an opening, so it now only says “if I slip again, tell me”; naming the style she already had was reported as a change, so it now says the style is kept; and the caption said the comparison happens “before driving” even when opened after a drive. All 25 paths were run again.
20. **Independent consistency check.** A separate review compared the report, the code, this README and the figures. It found: her reminder (“I told you, MSA!”) and her opening questions are scripted, but this was not stated everywhere, so they are now listed as simulated in the report, the “What is real?” panel, this table and the diagram; her Decline choice was saved but never read back, so the next day now reads it from browser storage; after the Decline turn, “Try MSA again” said “in my previous reply… I will return to MSA” although the previous reply was already in MSA, so it now says “a moment ago… I will stay in MSA”; “Continue the drive” after a style change could only be tapped, so it now also accepts “نكمل” by voice; the planned forget control was added to the panel and the diagram. All 25 paths were run again.

All 25 drive paths (5 choices after the repair × 5 endings: “Try MSA again”, or “Continue as it is” or “Switch to English” followed by MSA or English the next day) were run automatically, and a 47-point checklist (every expected Arabic line, the research comparison and the page controls) was checked on the live site, with voice answers simulated by a mocked recogniser.

## AI assistance

- **My own work:** I designed the interaction, the interface (the car scene, the phone screens and every voice step) and the system diagram (its components, flow, labels and layout). I tested each version and decided every change (examples: the next-day bug, the "from now on" promise, the feminine forms, moving the comparison out of the drive, the default style).
- **Claude (Anthropic)** wrote the code from my instructions.
- **Claude** also helped me turn my own notes into academic English for the report. I designed the storyboards (which moments to show and what each frame says) and gave Claude the content; Claude captured the screenshots from the live prototype and arranged them as I specified.
- **Gemini (Google)** was used only as a checklist: I asked it whether I had followed the steps the brief requires or had missed anything.
- I checked every Arabic line, the labels for implemented, simulated and planned parts, and the claims about the literature myself.

## Additional files

- `system-diagram.png`: system diagram used in the report.
- No ZIP file is needed.
