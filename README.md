# Paper Planner

Simple printable planner sheets for teachers and students, built with **Svelte, HTML, and CSS**. Teachers and students use the same sheets.

- **Current Week Overview:** a checklist for each day of this week.
- **Next Week Overview:** the same layout for next week, so upcoming events show up early.
- **Daily Schedule:** time blocks and activities, with circles for the day of the week and the letter day.
- **Reminders:** one running checklist with a When column.

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

- Choose **Weekly packet** or **Single sheets**.
- The weekly packet prints one-sided: the Current Week Overview, the Next Week Overview, then a Daily Schedule page and a Reminders page for each day.
- Use **Single sheets** to reprint one page.
- Enter the daily schedule and the special for each letter day in the sidebar.
- Choose **Left edge** or **Right edge** to leave more room for the hole punch on that side.
- Click **Print / Save PDF**. Choose US Letter portrait, scale **100%**, and turn off browser headers and footers.
- The **Examples** options fill the sheets with sample Teacher or Student entries.

## Privacy

Schedules and specials are saved with `localStorage` in the current browser. They are **not encrypted** and may be lost if browser storage is cleared. Each browser keeps its own copy. Use only district-approved devices, avoid confidential student information, and clear browser data when appropriate. Do not add third-party scripts, analytics, or data-collection services.
