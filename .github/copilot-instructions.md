# Orbit space quiz

## Running and validation

Open `index.html` directly in a browser using a `file://` URL. On Windows, run `Start-Process .\index.html` from the project directory. For the integrated browser, resolve the current workspace's absolute path rather than assuming a previous session's location.

There are no build, test, or lint commands configured. Validation is browser-based:

- For a single-question smoke check, reload the page and choose Yuri Gagarin on question 1: score becomes 1, progress becomes 10%, green feedback appears, all answers lock, and focus moves to Next. Reload and choose Neil Armstrong: score stays 0, red shake feedback appears, and the correct answer is highlighted.
- For an end-to-end check, complete all 10 questions with zero, five, and ten correct answers. Verify score, accuracy, emoji, explorer rank, completed progress, and replay reset. Also check the rank boundaries at four/five and seven/eight correct answers.
- Check native Tab/Shift+Tab navigation and Enter/Space activation, both color schemes, reduced motion, and a 320px viewport without horizontal overflow. DOM `.click()` and `.focus()` checks do not verify actual keyboard events or screen-reader announcements; report those limitations explicitly.

## Architecture and behavior

The application is a self-contained `index.html`: CSS in the head, static page sections in the body, and plain JavaScript at the end. Keep runtime code, question data, fonts, and artwork local to this file; it must work without a server, dependencies, or network requests. Artwork uses inline SVG and a CSS data-URI mask.

The `questions` array defines ordered question objects with `category`, `question`, four `answers`, a zero-based `correct` index, and an explanatory `fact`. State is held in `current`, `score`, and `answered`, and resets on page reload.

`renderQuestion()` creates answer buttons and clears feedback. `selectAnswer()` allows one attempt, awards one point only for a correct answer, locks all answers, reveals feedback, and updates progress. Next advances explicitly; there is no timer or automatic advance. `showResults()` swaps the quiz for results. Replay resets state, progress, and focus.

Progress measures questions answered, not the current question number. Results use Commander for scores 8-10, Astronaut for 5-7, and Cadet for 0-4. Ten-question assumptions also exist in static labels, progress ARIA bounds, ten-segment CSS, accuracy calculation, and result thresholds; question-count changes must update these surfaces together.

## UI and accessibility conventions

- Preserve the centered 540px maximum column, system sans-serif typography, and 14-16px body scale. The mission-control panel contains a subtle hex mask, theme-colored radial glows, tabular numeric readouts, and segmented progress.
- Theme colors come from `:root` custom properties with `prefers-color-scheme: dark` overrides. Dark mode also overrides button foreground colors. Check contrast in both themes when changing tokens; measure settled colors rather than intermediate transitions.
- Feedback combines color, text, and check/cross symbols: green `correct-pop` for correct answers and red `shake` for wrong answers. All animations and transitions are disabled under `prefers-reduced-motion: reduce`.
- Answers and navigation use native buttons. After answering, focus moves to Next; advancing or replaying focuses the question heading; results focus their heading. Headings use `tabindex="-1"` for programmatic focus.
- Keep `#feedback` mounted and unhidden as a polite, atomic status region. Clear it with `replaceChildren()` and collapse its empty appearance with `.feedback:empty`; do not hide the live region before inserting feedback.
- Keep progress width, `aria-valuenow`, `aria-valuetext`, mission status, and score synchronized through `updateProgress()`. Decorative layers must not intercept pointer events; SVG and decorative answer letters are hidden from assistive technology.
- The `$` helper resolves element IDs. Populate question text and explanations with `textContent` and DOM nodes; existing `innerHTML` use is limited to application-owned navigation and score markup.
