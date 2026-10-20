# Paper Planner: project rules

Printable classroom sheets for students with limited working memory. Clarity and simplicity come first.

## Design rules

- **No all caps anywhere.** Not in the app UI and not on the printed sheets. Use normal sentence or title case. Never use `text-transform: uppercase` and never type labels in all capitals (the single letters A, B, C, D for letter days are fine). Students find all-caps text harder to read.
- **Keep headers simple.** Name and Date at the top right, plus one strip of circle options (day of week, letter day) under the title. No number badges and no stacked step lists.
- **Nothing hardcoded for one class.** Schedules, specials and names are entered in the sidebar and saved in the browser (`localStorage`). The Teacher and Student examples are the only built-in sample data.
- **One sheet works any day.** Do not print letter-day-specific sheets. Students circle the day and letter day on the page.
- **Print fit.** Each sheet is US Letter portrait. After any layout change, check that the content still fits above the bottom margin.
- **Weekly packet prints one-sided.** One print job: the Current Week Overview, the Next Week Overview, then a Daily Schedule page and a Reminders page for each day. Every page is its own sheet with the same punch margin, so nothing is hidden on a back side and students never flip a sheet. Single-sheet mode is for replacing one page.
