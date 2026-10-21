# Paper Planner: project rules

Printable classroom sheets for students with limited working memory. Clarity and simplicity come first.

## Design rules

- **No all caps anywhere.** Not in the app UI and not on the printed sheets. Use normal sentence or title case. Never use `text-transform: uppercase` and never type labels in all capitals (the single letters A, B, C, D for letter days are fine). Students find all-caps text harder to read.
- **Keep headers simple.** Name and Date at the top right, plus one strip of circle options (day of week, letter day) under the title. No number badges and no stacked step lists.
- **Nothing hardcoded for one class.** Schedules, specials and names are entered in the sidebar and saved in the browser (`localStorage`). The Teacher and Student examples are the only built-in sample data.
- **One sheet works any day.** Do not print letter-day-specific sheets. Students circle the letter day on the page. A single Daily Schedule sheet also has the day of the week to circle. In the Daily packet each page is printed with its day name instead, so students never have to choose.
- **Print fit.** Each sheet is US Letter portrait. After any layout change, check that the content still fits above the bottom margin. Check the Daily page worst case: 11 schedule rows plus four long specials that wrap.
- **Two packets, one-sided.** Every page is its own sheet with the same punch margin, so nothing is hidden on a back side and students never flip a sheet.
  - **Planning packet** (stays in the binder, same order all week): This Week, Reminders, Next Week. These are the write-in pages. Anything with a day goes on This Week or Next Week. Anything without a day, or not finished, goes on Reminders. Reminders is one page per week with no When column.
  - **Daily packet**: one Daily Schedule page per day, clipped together with today on top. Each page has the schedule on top and a Today I need to block below it. At dismissal, anything unfinished is written once on Reminders (never recopied to the next day) and the page comes off.
  - Single-sheet mode is for replacing one page.
