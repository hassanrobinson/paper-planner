# Paper Planner

Simple printable planner sheets for teachers and students, built with **Svelte, HTML, and CSS**. Teachers and students use the same sheets.

- **This Week:** a checklist for each day of this week.
- **Reminders:** one running checklist for things that are not finished or have no day.
- **Next Week:** the same layout as This Week, so students can write down upcoming events the moment they come up.
- **Daily Schedule:** time blocks and activities, with circles for the letter day and a Today I need to block for the day's tasks.

## Run locally

Requires Node.js 20.19+ or 22+.

```bash
pnpm install
pnpm dev
```

Open the local URL shown by Vite. To make a production build:

```bash
pnpm build
```

Deploy the `dist` folder to any static host. The repo deploys to GitHub Pages automatically when `main` is pushed. No backend, accounts, or student-data APIs are used.

## How to use

- Choose **Planning packet**, **Daily packet**, or **Single sheets**.
- The Planning packet prints This Week, Reminders and Next Week. These stay in the binder in that order all week.
- The Daily packet prints one Daily Schedule page for each day, with the day name on the page. Clip them together with today on top. At dismissal, move anything not finished to Reminders and take the page off.
- Use **Single sheets** to reprint one page.
- Enter the daily schedule and the special for each letter day in the sidebar.
- Choose **Left edge** or **Right edge** to leave more room for the hole punch on that side.
- Click **Print / Save PDF**. Choose US Letter portrait, scale **100%**, and turn off browser headers and footers.
- The **Examples** options fill the sheets with sample Teacher or Student entries.

## Privacy

Schedules and specials are saved with `localStorage` in the current browser. They are **not encrypted** and may be lost if browser storage is cleared. Each browser keeps its own copy. Use only district-approved devices, avoid confidential student information, and clear browser data when appropriate. Do not add third-party scripts, analytics, or data-collection services.
