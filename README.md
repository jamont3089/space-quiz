# Orbit - Space Exploration Quiz

A self-contained space exploration quiz with 10 questions, animated feedback, and a mission-control-inspired progress display.

## Run

Open `index.html` directly in a browser. No server, dependencies, installation, or build step is required.

On Windows, from the project directory:

```powershell
Start-Process .\index.html
```

## Live site

The quiz is deployed to GitHub Pages at https://jamont3089.github.io/space-quiz/.

Every push to `main` runs [`.github/workflows/deploy-pages.yml`](.github/workflows/deploy-pages.yml), which publishes `index.html`. The workflow can also be run manually from the Actions tab.

## Features

- Ten multiple-choice questions with explanations and one attempt per question.
- Live score and a ten-segment progress bar.
- Green animated feedback for correct answers and a red shake for incorrect answers.
- Results with accuracy, an emoji reaction, and an explorer rank.
- Replay to start a fresh quiz.
- A narrow, responsive layout with system light/dark themes.
- Native buttons, focus management, accessible progress descriptions, and reduced-motion support.
- Inline SVG artwork and a subtle hex-grid mission-control panel.

## Implementation

All application markup, CSS, JavaScript, and question data live in `index.html`. Quiz state is kept in memory and resets on reload.

Question objects contain a category, prompt, four answers, a zero-based correct-answer index, and an explanatory fact. Results award the Cadet rank for 0-4 correct answers, Astronaut for 5-7, and Commander for 8-10.

## Validation

Validation is browser-based. For a quick check, select Yuri Gagarin on question 1 and confirm one point, 10% progress, and green feedback. Reload and select Neil Armstrong to check incorrect feedback and an unchanged score.

Complete the quiz to check results and replay. Check keyboard navigation with Tab, Shift+Tab, Enter, and Space, plus light/dark themes, reduced motion, and narrow viewports.

Project-specific guidance and more detailed smoke checks are in [the Copilot instructions](.github/copilot-instructions.md).
