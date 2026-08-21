---
name: New Day
description: Adds today's UTC date to the Daily Updates navigation and a matching dialog confirming the daily update ran.
on:
  schedule: daily
  workflow_dispatch:
engine: copilot
permissions:
  contents: read
  copilot-requests: write
tools:
  edit:
safe-outputs:
  create-pull-request:
    allowed-files:
      - "index.html"
    max: 1
---

# New Day

Update `index.html` to record that today's daily update ran, using the **UTC date of this workflow run**.

## Steps

1. Determine today's date in UTC (the date this workflow run started).
2. Format the date the same way the existing entries in the "Daily Updates" navigation are worded (e.g. `1st of August` — ordinal day of month, "of", full month name).
3. Derive the ID slug the same way the existing entries do (e.g. `august-1` — lowercase month name, hyphen, day number without ordinal suffix or leading zero).
4. Open `index.html` and check the `daily-updates-list` navigation and the `<dialog>` elements for an entry whose ID slug or displayed date already matches today's UTC date.
   - **If a matching date, navigation control, or dialog already exists, make no change at all** — do not edit the file, and do not create a duplicate entry.
5. If today's date is **not** already present, make the following additive changes to `index.html`, preserving every existing daily update entry exactly as-is:
   - Add a new `<li>` inside `.daily-updates-list` following the exact structure of the existing entry: a `button.daily-update-trigger` with `type="button"`, `aria-haspopup="dialog"`, `aria-controls="<slug>-dialog"`, and `data-dialog-trigger`, containing a `<span>` with today's formatted date text and the existing `<span aria-hidden="true">&#8594;</span>` arrow.
   - Add a new `<dialog>` element following the exact structure, attributes, and styling classes of the existing `august-1-dialog` (`class="daily-update-dialog"`, `id="<slug>-dialog"`, `aria-labelledby="<slug>-question"`, `aria-describedby="<slug>-answer"`), including the same header pattern (`Daily Update / <formatted date>` label and close button/form) and the same `<article class="daily-update-dialog-content">` structure.
   - The dialog heading (`id="<slug>-question"`) and body (`id="<slug>-answer"`) must confirm that the daily update workflow ran successfully for today's UTC date — keep the tone and phrasing consistent with the existing dialog's content, but the message is simply confirming today's run happened (it does not need to restate the "isn't this non-deterministic?" content).
   - Do not modify `styles.css` or any styling — reuse the existing classes exactly.
   - Do not modify the `<script>` block; the existing `data-dialog-trigger` click handler already supports any number of entries.
6. Double-check the new IDs (`<slug>` for the trigger's `aria-controls`, the dialog's `id`, and the heading/body IDs) do not collide with any existing IDs in the file.
7. Open a pull request with the change to `index.html` only.
