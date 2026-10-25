# Copilot instructions for Space Quiz

## Project shape

This is a dependency-free, single-page browser quiz. The complete application lives in `index.html`:

- The `<style>` block contains all layout, theme, component, responsive, focus, and animation rules.
- The body markup contains the quiz shell, progress/score UI, answer controls, feedback area, and results view.
- The `<script>` block owns the question data and the quiz state machine.

There is no package manifest, build system, server, test runner, or lint configuration. The file can be opened directly with a `file:///` URL in a browser. For changes that affect UI behavior, use the integrated browser preview and exercise the quiz rather than relying only on static inspection.

## Architecture and runtime flow

The quiz is driven by the `questions` array and three mutable state values:

- `current` is the zero-based question index.
- `score` is the number of correct answers.
- `answered` prevents a question from being answered more than once.

`renderQuestion()` derives all question-specific UI from `current`, updates the progress bar and labels, clears feedback, and recreates the answer buttons. Each answer button calls `choose(index)`. `choose()` disables all options, marks the correct option, updates the score or wrong-answer feedback, and reveals the next-question button. The next button advances `current`, or calls `showResults()` after the final question. The restart button resets state and renders the first question.

The quiz and results screens are mutually exclusive through the `.hidden` class. Keep that behavior intact when changing the results flow. Element IDs are the JavaScript interface between the markup and state logic, so update all corresponding reads/writes if an ID changes.

## UI and CSS conventions

- Preserve the narrow centered layout and the mission-control visual language: layered surfaces, grid/instrumentation treatment, purple primary accents, and green/red status feedback.
- Use the existing CSS custom properties in `:root` for shared colors and shadows. Add new tokens there rather than scattering replacement colors through selectors.
- Maintain both themes. The light theme is the default token set; dark-theme overrides belong in `@media (prefers-color-scheme: dark)`.
- Keep controls as native `<button>` elements. Preserve visible `:focus-visible` outlines and disabled states when styling answer or navigation controls.
- Keep reduced-motion support in `@media (prefers-reduced-motion: reduce)` for any new animation or transition.
- Use the existing `.correct`, `.wrong`, `.feedback.correct`, and `.feedback.wrong` state classes so visual feedback remains consistent.

## Content and behavior conventions

- Questions use objects shaped as `{ q, a, correct }`, where `correct` is the zero-based index into `a`.
- Keep the displayed question count, percentage, progressbar `aria-valuenow`, score, and results score synchronized with the same `questions.length` source.
- Dynamically generated answer buttons must retain their `data-index` values and accessible text.
- Feedback is exposed through the existing `role="status"` element; keep success and correction messages text-based and understandable without color or animation.
- Avoid adding dependencies or splitting the app into additional runtime files unless the project scope explicitly changes.

## Validation

No automated build, test, or lint commands exist. Validate changes by opening or refreshing `index.html` in a browser and checking:

1. Keyboard focus reaches each answer and navigation button, with a visible focus indicator.
2. A correct answer increments the score and shows correct feedback.
3. An incorrect answer leaves the score unchanged, marks the wrong answer, highlights the correct answer, and disables the options.
4. Advancing through all 10 questions shows the results screen and the correct final score/reaction.
5. Restart returns to question 1 with score 0.
6. The layout and contrast remain usable in both light and dark `prefers-color-scheme` modes.

For a quick browser smoke test, use the local file URL:

```text
file:///C:/dev/space-quiz/index.html
```
