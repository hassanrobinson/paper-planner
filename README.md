# Teacher Paper Planner

Three deliberately simple printable templates, built with **Svelte, HTML, and CSS**:

- **Daily Schedule:** time blocks, activities, and notes.
- **Weekly Overview:** when, event/deadline, and next action.
- **Reminders:** one running checklist with a when column.

## Run locally

Requires Node.js 20.19+ or 22+.

```bash
npm install
npm run dev
```

Open the local URL shown by Vite. To make a production build:

```bash
npm run build
```

Deploy the `dist` folder to any static host. No backend, external fonts, accounts, or student-data APIs are used.

## How to use

- Select a template.
- Edit rows or click **+ Add row**; additional rows generate numbered pages automatically.
- Select **Left edge** or **Right edge** to leave more punch clearance on that side.
- Click **Print / Save PDF**; choose US Letter portrait, scale **100%**, and turn off browser headers/footers.
- **Clear this sheet** gives blank lines; other pages are untouched.

The example starts with fictional/generic classroom tasks and an illustrative schedule; adjust it to your actual daily timetable.

## Privacy

Data is saved with `localStorage` in the current browser for convenience. It is **not encrypted** and may be lost if browser storage is cleared. Use only district-approved devices, avoid confidential student information, and clear browser data when appropriate. Deployment must avoid adding third-party scripts, analytics, or data-collection services if claiming local-only processing.

## Notes

Handwriting row sizes are fixed rather than shrinking as the list grows. Each additional page repeats the heading and includes `Page N of M`. Daily pages have nine schedule rows per page, weekly pages seven entries, and reminders pages twelve entries. Only the last daily page shows the notes box.
