<script>
  let selected = $state({ daily: true, weekly: false, nextWeek: false, reminders: false });
  let exampleMode = $state('none');
  let printSide = $state('left');

  const SETUP_KEY = 'planner.dailySetup';
  const DAILY_ROWS = 11;
  const DAY_LETTERS = ['A', 'B', 'C', 'D'];
  const SPECIAL_TOKEN = '{special}';
  const loadSetup = () => {
    const empty = { base: '', specials: Object.fromEntries(DAY_LETTERS.map((d) => [d, ''])) };
    try {
      const saved = JSON.parse(localStorage.getItem(SETUP_KEY) ?? '{}');
      return { base: saved.base ?? '', specials: { ...empty.specials, ...saved.specials } };
    } catch { return empty; }
  };
  const saveSetup = () => {
    try { localStorage.setItem(SETUP_KEY, JSON.stringify(setup)); } catch {}
  };
  let setup = $state(loadSetup());
  const specialLetters = $derived(DAY_LETTERS.filter((d) => setup.specials[d].trim()));
  // Accepts "time, activity" or tab-separated text pasted from a spreadsheet.
  const parseSchedule = (text) =>
    text
      .split('\n')
      .map((line) => line.trim())
      .filter(Boolean)
      .map((line) => {
        const i = line.includes('\t') ? line.indexOf('\t') : line.indexOf(',');
        return i === -1 ? ['', line] : [line.slice(0, i).trim(), line.slice(i + 1).trim()];
      });
  const baseRowCount = $derived(parseSchedule(setup.base).length);

  const pages = {
    weekly: 'Current Week Overview',
    nextWeek: 'Next Week Overview',
    daily: 'Daily Schedule',
    reminders: 'Reminders'
  };
  const shown = $derived(Object.keys(pages).filter((key) => selected[key]));

  // Weekly packet prints one-sided: the two week overviews, then a Daily Schedule and a
  // Reminders page for each day. Every page is its own sheet.
  const PACKET_DAYS = ['Mon', 'Tue', 'Wed', 'Thur', 'Fri'];
  let mode = $state('sheets');
  let packetWeekly = $state(true);
  let packetNextWeek = $state(true);
  let packetDays = $state(Object.fromEntries(PACKET_DAYS.map((d) => [d, true])));
  const sheets = $derived(
    (mode === 'packet'
      ? [
          ...(packetWeekly ? [{ view: 'weekly' }] : []),
          ...(packetNextWeek ? [{ view: 'nextWeek' }] : []),
          ...PACKET_DAYS.filter((d) => packetDays[d]).flatMap((day) => [
            { view: 'daily', day },
            { view: 'reminders', day }
          ])
        ]
      : shown.map((view) => ({ view }))
    ).map((sheet, i) => ({ ...sheet, id: `${i}-${sheet.view}` }))
  );
  const teacherExamples = {
    weekly: {
      rows: [
        [{ t: 'Return RSVP forms', done: true }, { t: 'Copy math centers', done: true }, { t: 'Staff meeting 3:30' }, { t: 'Call Jaden’s family' }],
        [{ t: 'Family Dinner RSVP due', done: true }, { t: 'Book fair sign-up' }, { t: 'i-Ready groups' }, { t: 'Post lunch counts' }],
        [{ t: 'Science materials ready' }, { t: 'Turn forms to office' }, { t: 'Observation 10:15' }, { t: 'Update grades' }],
        [{ t: 'i-Ready makeups' }, { t: 'Counselor check-in' }, { t: 'Parent conference 2:45' }, { t: 'Order supplies' }],
        [{ t: 'Plan next week' }, { t: 'Filming: heritage talk' }, { t: 'Clean out folders' }, { t: 'Send class newsletter' }]
      ]
    },
    nextWeek: {
      rows: [
        [{ t: 'Send class newsletter' }, { t: 'Copy math centers' }, { t: 'Staff meeting 3:30' }],
        [{ t: 'Book fair sign-up' }, { t: 'i-Ready groups' }, { t: 'Post lunch counts' }],
        [{ t: 'Science materials ready' }, { t: 'Observation 10:15' }, { t: 'Update grades' }],
        [{ t: 'Chorus at heritage dinner' }, { t: 'Parent conference 2:45' }, { t: 'Order supplies' }],
        [{ t: 'Plan next week' }, { t: 'Clean out folders' }, { t: 'Send class newsletter' }]
      ]
    },
    daily: {
      date: 'Mon, Oct 19',
      rows: [
        ['7:30–7:50', 'Arrival, breakfast, morning meeting'],
        ['7:50–9:20', 'Math block'],
        ['9:20–9:50', 'Math small groups'],
        ['9:50–11:20', 'Reading and writing block'],
        ['11:50–12:30', 'Lunch and recess']
      ],
      added: [['10:15', 'Office needs RSVP count'], ['1:10', 'Presentation filming']]
    },
    reminders: {
      date: 'Oct 19',
      rows: [
        ['Check who still needs a Family Dinner RSVP slip', 'Monday'],
        ['Log family calls in PowerSchool', 'After each call'],
        ['Confirm science materials with teammate', 'Wednesday'],
        ['Ask counselor about check-in request', 'This week'],
        ['Check who missed i-Ready test', 'Thursday']
      ]
    }
  };
  const studentExamples = {
    weekly: {
      rows: [
        [{ t: 'Sign reading log', done: true }, { t: 'Gym shoes', done: true }, { t: 'Bring my instrument' }, { t: 'Spelling words' }],
        [{ t: 'Library book due', done: true }, { t: 'Band lesson 2:00' }, { t: 'Family Dinner form' }, { t: 'Practice math facts' }],
        [{ t: 'Math test' }, { t: 'Return signed form' }, { t: 'Return library book' }, { t: 'Read 20 minutes' }],
        [{ t: 'Dentist 3:30' }, { t: 'Bring water bottle' }, { t: 'Finish math page' }, { t: 'Chorus rehearsal' }],
        [{ t: 'Picture day' }, { t: 'Book order due' }, { t: 'Clean out desk' }, { t: 'Pack for weekend' }]
      ]
    },
    nextWeek: {
      rows: [
        [{ t: 'Bring my instrument' }, { t: 'Spelling words' }],
        [{ t: 'Library book due' }, { t: 'Band lesson 2:00' }],
        [{ t: 'Math test' }, { t: 'Read 20 minutes' }],
        [{ t: 'Chorus at heritage dinner' }, { t: 'Wear dress clothes' }],
        [{ t: 'Pack for weekend' }, { t: 'Bring water bottle' }]
      ]
    },
    daily: {
      date: 'Mon, Oct 19',
      rows: [
        ['8:00', 'Morning work'],
        ['8:30', 'Math'],
        ['10:00', 'Reading and writing'],
        ['11:50', 'Lunch and recess'],
        ['12:30', 'Science']
      ],
      added: [['9:30', 'Band'], ['1:30', 'Chorus'], ['2:00', 'Makeup test']]
    },
    reminders: {
      date: 'Oct 19',
      rows: [
        ['Bring my instrument for band', 'Monday'],
        ['Sign and return the Family Dinner form', 'Tuesday'],
        ['Return my library book', 'Wednesday'],
        ['Finish my math page', 'Thursday'],
        ['Practice my spelling words', 'Every night']
      ]
    }
  };
  const examples = { teacher: teacherExamples, student: studentExamples };
  const exampleNames = { teacher: 'Ms. Rivera', student: 'Maya' };
  const current = $derived(examples[exampleMode]);
  const example = (key, list = 'rows') => current?.[key][list] ?? [];
  const rows = (n) => Array.from({ length: n });
  const weekDays = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday'];
  const weekDates = { weekly: ['19', '20', '21', '22', '23'], nextWeek: ['26', '27', '28', '29', '30'] };
  const weekTitles = { weekly: 'This Week', nextWeek: 'Next Week' };
</script>

<svelte:head>
  <title>Paper Planner — Printable Sheets</title>
</svelte:head>

{#snippet nameDate(key)}
  <div class="name-date">
    <div class="field"><b>Name:</b><span class="ex">{exampleMode === 'none' ? '' : exampleNames[exampleMode]}</span></div>
    <div class="field"><b>Date:</b><span class="ex">{current ? current[key].date : ''}</span></div>
  </div>
{/snippet}

{#snippet circleStrip(showLetters = false)}
  <div class="circle-strip">
    <div class="circle-group" aria-label="Circle the day of the week">
      <b>Day:</b>
      {#each ['Mon', 'Tue', 'Wed', 'Thur', 'Fri'] as day}
        <span class="day" class:circled={current && current.daily.date.startsWith(day.slice(0, 3))}>{day}</span>
      {/each}
    </div>
    {#if showLetters}
      <div class="circle-group" aria-label="Circle the letter day">
        <b>Letter day:</b>
        {#each DAY_LETTERS as letter}
          <span class="day" class:circled={current && letter === 'A'}>{letter}</span>
        {/each}
      </div>
    {/if}
  </div>
{/snippet}

{#snippet weekSheet(key)}
    <header class="wk-hdr">
      <div class="wk-title">
        <svg class="wk-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><rect x="3" y="4" width="18" height="18" rx="2"/><path d="M16 2v4M8 2v4M3 10h18"/></svg>
        <span>{weekTitles[key]}</span>
        {#if current}<span class="wk-exlabel">Example</span>{/if}
      </div>
      <div class="wk-name"><span>Name:</span><span class="wk-name-line" class:hw={current}>{current ? exampleNames[exampleMode] : ''}</span></div>
    </header>
    <div class="wk-grid">
      {#each weekDays as day, i}
        <section class="wk-row">
          <div class="wk-cell">
            <span class="wk-day">{day}</span>
            <span class="wk-dd"><i class:hw={current}>{current ? '10' : ''}</i><b>/</b><i class:hw={current}>{current ? weekDates[key][i] : ''}</i></span>
          </div>
          <div>
            {#each rows(4) as _, j}
              {@const item = example(key)[i]?.[j]}
              <div class="wk-line">
                <span class="wk-cb">{#if item?.done}<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3.2" stroke-linecap="round" stroke-linejoin="round"><path d="m5 12.5 5 5 9-11"/></svg>{/if}</span>
                {#if item}<span class="wk-text hw">{item.t}</span>{/if}
              </div>
            {/each}
          </div>
        </section>
      {/each}
    </div>
{/snippet}

<div class="app-shell">
  <header class="toolbar">
    <div class="brand">
      <div class="brand-mark">P</div>
      <div><strong>Paper Planner</strong><span>Blank printable sheets</span></div>
    </div>
    <div class="toolbar-actions">
      <label class="punch-setting">Holes
        <select bind:value={printSide} aria-label="Hole punch margin"><option value="left">Left edge</option><option value="right">Right edge</option></select>
      </label>
      <button class="primary" onclick={() => window.print()}>Print / Save PDF</button>
    </div>
  </header>

  <div class="workspace">
    <aside class="sidebar" aria-label="Sheets to print">
      <div class="mode-toggle" role="radiogroup" aria-label="What to print">
        <label class:active={mode === 'sheets'}><input type="radio" name="mode" value="sheets" bind:group={mode} />Single sheets</label>
        <label class:active={mode === 'packet'}><input type="radio" name="mode" value="packet" bind:group={mode} />Weekly packet</label>
      </div>
      {#if mode === 'packet'}
        <label class="sheet-choice"><input type="checkbox" bind:checked={packetWeekly} />Current Week Overview</label>
        <label class="sheet-choice"><input type="checkbox" bind:checked={packetNextWeek} />Next Week Overview</label>
        <p class="side-label">Days</p>
        <div class="print-days">
          {#each PACKET_DAYS as d}
            <label><input type="checkbox" bind:checked={packetDays[d]} />{d}</label>
          {/each}
        </div>
        <p class="sidebar-note packet-note">{sheets.length} {sheets.length === 1 ? 'page' : 'pages'}, one-sided. Each day prints a Daily Schedule page and a Reminders page.</p>
      {:else}
        <p class="side-label">Sheets To Print</p>
        {#each Object.entries(pages) as [key, label]}
          <label class="sheet-choice"><input type="checkbox" bind:checked={selected[key]} />{label}</label>
        {/each}
      {/if}
      {#if selected.daily || mode === 'packet'}
        <div class="schedule-input">
          <label class="side-label" for="base-schedule">Daily schedule</label>
          <textarea id="base-schedule" rows="9" placeholder={"7:30–7:50, Arrival\n7:50–9:20, Math block\n12:30–1:05, {special}"} bind:value={setup.base} onblur={saveSetup} spellcheck="false"></textarea>
          <p class="sidebar-note">One row per line: <b>time, activity</b>. You can paste two spreadsheet columns too. Type <b>{'{special}'}</b> where the special goes; it prints all four with a bubble beside each for students to shade.{baseRowCount > DAILY_ROWS ? ` Only the first ${DAILY_ROWS} rows fit.` : ''}</p>
          <p class="side-label">Special for each letter day</p>
          <div class="specials">
            {#each DAY_LETTERS as d}
              <label><b>{d}</b><input type="text" bind:value={setup.specials[d]} onblur={saveSetup} placeholder="Gym" spellcheck="false" /></label>
            {/each}
          </div>
        </div>
      {/if}
      <fieldset class="example-choice">
        <legend class="side-label">Examples</legend>
        {#each [['none', 'None'], ['teacher', 'Teacher'], ['student', 'Student']] as [value, label]}
          <label class="sheet-choice"><input type="radio" name="examples" {value} bind:group={exampleMode} />{label}</label>
        {/each}
      </fieldset>
      <p class="sidebar-note">Every page prints on its own sheet, one side only.</p>
    </aside>

  <main class="main-area">
    <div class="print-area" class:right-punch={printSide === 'right'}>
      {#each sheets as sheet (sheet.id)}
      {@const view = sheet.view}
      <section class="sheet" aria-label={`${sheet.day ? sheet.day + ' ' : ''}${pages[view]}`}>
        {#if view === 'daily'}
          <div class="sheet-header title-row"><h2>Daily Schedule</h2>{@render nameDate('daily')}</div>
          {@render circleStrip(true)}
          <div class="table-wrap daily-table">
            <div class="table-head"><span></span><span>Time</span><span>Activity</span></div>
            {#each rows(DAILY_ROWS) as _, i}
              {@const row = current ? example('daily')[i] : parseSchedule(setup.base)[i]}
              <div class="table-row daily-row"><span class="empty-check" class:checked={current && i === 0}></span><span class:ex={current} class:typed={!current}>{row?.[0] ?? ''}</span>{#if !current && row?.[1] === SPECIAL_TOKEN}
                  <span class="special-options">
                    {#each specialLetters as d}
                      <span class="special-option"><i class="bubble"></i><b>{d}</b>{setup.specials[d].trim()}</span>
                    {:else}
                      <span>Special</span>
                    {/each}
                  </span>
                {:else}
                  <span class:ex={current} class:typed={!current}>{row?.[1] ?? ''}</span>
                {/if}
              </div>
            {/each}
          </div>
          <div class="table-wrap daily-table added-table">
            <div class="table-head"><span></span><span>Time</span><span>Additional Activity</span></div>
            {#each rows(4) as _, i}
              <div class="table-row daily-row"><span class="empty-check"></span><span class="ex">{example('daily', 'added')[i]?.[0] ?? ''}</span><span class="ex">{example('daily', 'added')[i]?.[1] ?? ''}</span></div>
            {/each}
          </div>
        {:else if view === 'weekly' || view === 'nextWeek'}
          {@render weekSheet(view)}
        {:else}
          <div class="sheet-header title-row"><h2>Reminders</h2>{@render nameDate('reminders')}</div>
          {@render circleStrip()}
          <div class="table-wrap reminders-table">
            <div class="table-head"><span></span><span>Reminder</span><span>When</span></div>
            {#each rows(16) as _, i}
              <div class="table-row reminder-row"><span class="empty-check"></span><span class="ex">{example('reminders')[i]?.[0] ?? ''}</span><span class="ex">{example('reminders')[i]?.[1] ?? ''}</span></div>
            {/each}
          </div>
        {/if}
      </section>
      {/each}
      {#if sheets.length === 0}<p class="below-note">Check at least one sheet to print.</p>{/if}
    </div>
    <p class="below-note">US Letter · Portrait · Browser print: choose 100% scale and disable browser headers/footers.</p>
  </main>
  </div>
</div>
