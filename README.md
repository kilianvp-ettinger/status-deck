# Status Deck

A configurable status band above the prompt in the Claude Code desktop app. It started as a plan-usage meter and now shows everything you want to keep an eye on while you work, in six looks and with every part switchable.

## What it shows

- **5-hour and weekly plan limits** with a progress bar, a pace marker (how far through the window you are) and the time until each resets, as a countdown or a clock time
- **Context window** fill with a warning near full
- **Git branch**, with a dot when there are uncommitted changes
- **Prompt cache** warm or cold and how long it stays warm (1 hour or 5 minutes)
- **Keep-warm** (off by default): while you are idle, a one-word request goes out shortly before the cache expires, for 1 to 12 hours after your last turn. Each ping reads the cached context and counts toward your plan usage, and it stops by itself if the cache was already gone. Same approach as [cache-tax](https://github.com/karanb192/cache-tax)
- Optional **session tokens** (in, out, cache) and **session cost**
- **Alert colors** when you use a limit faster than time passes, or when a limit is nearly used up

## Styles

Pick one in the settings: **Pills**, **Glass**, **Rings**, **Thin bars**, **Segments** or **Stacked**.

Also adjustable: fully rounded or rectangular pills, light-theme text, a compact layout (icon and percent only, or automatic when it would not fit), what to do when the band is too wide (wrap onto more rows or switch to compact), and an accent color for the 5-hour, weekly and context items, including any hex color.

5h, weekly and context always stay together on the first row; their bars shrink to fit before anything wraps.

## Install

Add the marketplace and install the plugin:

```text
/plugin marketplace add wellbrained/status-deck
/plugin install status-deck@status-deck
```

Then restart Claude or run `/reload-plugins`.

To try a local copy for one run instead:

```bash
claude --plugin-dir "<path to your clone>"
```

This mod used to be called **Usage Meter Pills** (`usage-meter-pills`). If you still have that plugin installed, remove it so you do not get two bands. Settings are stored per plugin, so Status Deck starts with the defaults.

## Settings

Type `/status-deck-options`, or press the gear next to the band. The settings are grouped into cards (Style, Usage limits, Context window, Extras) that open and close, and the open state is remembered. Every option has a live sample on the right.

Everything applies right away. Color changes can be saved or cancelled; every other option is kept immediately.

## Updates

A GitHub Action raises the patch version on every push to `main` that changes the plugin (docs, tests and workflow-only pushes are skipped), so installed copies can update to each release:

```text
/plugin marketplace update status-deck
/plugin update status-deck@status-deck
```

## Notes

- The desktop app draws the band as an image; the terminal gets a plain text line.
- The rounded frame around the band belongs to the app and cannot be styled from a plugin.
- Information the plugin API does not expose, such as free plan resets, is not shown.

## Development

```bash
claude plugin validate .
claude plugin test .
```

The code is one hooks module, [`hooks/register.js`](hooks/register.js). The settings pane is built from one list near the top of that file, so a new option, style or color needs an entry there and a sample in `previewFor`.

## Credits

The prompt cache indicator and keep-warm come from a contribution by [@jumoog](https://github.com/jumoog), who also added the version-bump workflow.
