<!doctype html>
<html lang="uk">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Terraria — галерея мечів</title>
  <meta name="description" content="Галерея мечів з Terraria: фільтр, пошук, легка підстановка нових картинок. Готово до розгортання на GitHub Pages." />
  <style>
    :root {
      --bg: #0b1020;
      --card: #141a2a;
      --muted: #96a2c2;
      --text: #e7edff;
      --accent: #6ea8ff;
      --radius: 16px;
    }
    * { box-sizing: border-box; }
    html, body { height: 100%; }
    body {
      margin: 0; font: 16px/1.6 system-ui, -apple-system, Segoe UI, Roboto, Ubuntu, Cantarell, 'Noto Sans', 'Helvetica Neue', Arial, 'Apple Color Emoji', 'Segoe UI Emoji';
      background: radial-gradient(1000px 600px at 20% -10%, #1a2240, transparent),
                  radial-gradient(800px 500px at 90% 10%, #1d274b, transparent),
                  var(--bg);
      color: var(--text);
    }
    header {
      position: sticky; top: 0; z-index: 10; backdrop-filter: blur(6px);
      background: color-mix(in oklab, var(--bg), transparent 40%);
      border-bottom: 1px solid color-mix(in oklab, var(--muted), transparent 60%);
    }
    .wrap { max-width: 1100px; margin: 0 auto; padding: 16px; }
    h1 { margin: 0 0 8px; font-size: clamp(1.2rem, 2.3vw + 0.8rem, 2rem); }
    .controls { display: grid; gap: 8px; grid-template-columns: 1fr; }
    .row { display: grid; gap: 8px; grid-template-columns: 1fr; }
    @media (min-width: 700px) { .controls { grid-template-columns: 1fr auto auto; } .row { grid-template-columns: 1fr 1fr 1fr; } }
    input[type="search"], select {
      width: 100%; padding: 12px 14px; border-radius: var(--radius);
      border: 1px solid color-mix(in oklab, var(--muted), transparent 60%);
      background: var(--card); color: var(--text); outline: none;
    }
    .grid { display: grid; gap: 14px; grid-template-columns: repeat(2, minmax(0, 1fr)); }
    @media (min-width: 560px) { .grid { grid-template-columns: repeat(3, minmax(0, 1fr)); } }
    @media (min-width: 900px) { .grid { grid-template-columns: repeat(4, minmax(0, 1fr)); } }
    .card {
      background: linear-gradient(180deg, color-mix(in oklab, var(--card), white 2%), var(--card));
      border: 1px solid color-mix(in oklab, var(--muted), transparent 70%);
      border-radius: var(--radius); padding: 12px; display: grid; gap: 10px;
      transition: transform .2s ease, box-shadow .2s ease, border-color .2s ease;
    }
    .card:hover { transform: translateY(-3px); border-color: color-mix(in oklab, var(--accent), transparent 60%);
      box-shadow: 0 10px 28px color-mix(in oklab, black, var(--accent) 12%);
    }
    .thumb { display: grid; place-items: center; aspect-ratio: 1/1; border-radius: 12px; background: #0f1222; }
    img { image-rendering: pixelated; width: 64px; height: 64px; }
    .meta { display: flex; align-items: center; justify-content: space-between; gap: 10px; }
    .tag { font-size: 12px; padding: 2px 8px; border-radius: 999px; border: 1px solid color-mix(in oklab, var(--muted), transparent 60%); color: var(--muted); }
    .name { font-weight: 700; letter-spacing: .2px; }
    footer { opacity: .8; font-size: 13px; margin: 24px 0; }
    a { color: var(--accent); text-decoration: none; }
    .empty { text-align: center; opacity: .7; padding: 40px 6px; }
  </style>
</head>
<body>
  <header>
    <div class="wrap">
      <h1>Terraria — мечі (галерея)</h1>
      <div class="controls" role="search">
        <input id="q" type="search" placeholder="Пошук: назва…" autocomplete="off" />
        <select id="era">
          <option value="all">Усі епохи</option>
          <option value="pre">Pre-Hardmode</option>
          <option value="hard">Hardmode</option>
          <option value="post">Post-Moon Lord</option>
        </select>
        <select id="type">
          <option value="all">Усі типи</option>
          <option value="broadsword">Бродсворд</option>
          <option value="shortsword">Шортсворд</option>
          <option value="special">Особливий / інше</option>
        </select>
      </div>
    </div>
  </header>

  <main class="wrap">
    <div id="grid" class="grid" aria-live="polite"></div>
    <p id="empty" class="empty" hidden>Нічого не знайдено. Змініть фільтри або запит.</p>

    <footer>
      <p>Зображення і назви — © Re-Logic / відповідні власники, використані як ілюстрації. Джерела: офіційна вікі (<a href="https://terraria.wiki.gg/wiki/Swords" target="_blank" rel="noopener">terraria.wiki.gg</a>) та Fandom вікі. Ви можете безпечно розгорнути це як GitHub Pages (лише статична HTML-сторінка).</p>
    </footer>
  </main>

  <script>
    const SWORDS = [
      { name: 'Copper Shortsword', img: 'https://static.wikia.nocookie.net/terraria_gamepedia/images/8/8b/Copper_Shortsword.png/revision/latest?cb=20200516210051', era: 'pre', type: 'shortsword', src: 'https://terraria.fandom.com/wiki/Copper_Shortsword' },
      { name: 'Starfury', img: 'https://static.wikia.nocookie.net/terraria_gamepedia/images/2/2d/Starfury.png/revision/latest?cb=20221121015812', era: 'pre', type: 'broadsword', src: 'https://terraria.fandom.com/wiki/Starfury' },
      { name: 'Terra Blade', img: 'https://static.wikia.nocookie.net/terraria_gamepedia/images/2/2d/Terra_Blade.png/revision/latest?cb=20200917120500', era: 'hard', type: 'broadsword', src: 'https://terraria.fandom.com/wiki/Terra_Blade' },
      { name: 'Night\'s Edge', img: 'https://static.wikia.nocookie.net/terraria_gamepedia/images/0/0a/Night%27s_Edge.png/revision/latest?cb=20221009094608', era: 'pre', type: 'broadsword', src: 'https://terraria.fandom.com/wiki/Night%27s_Edge' },
      { name: 'Meowmere', img: 'https://static.wikia.nocookie.net/terraria_gamepedia/images/5/5b/Meowmere.png/revision/latest?cb=20231117191541', era: 'post', type: 'broadsword', src: 'https://terraria.fandom.com/wiki/Meowmere' },
      { name: 'Zenith', img: 'https://static.wikia.nocookie.net/terraria_gamepedia/images/0/0f/Zenith.png/revision/latest?cb=20200604044113', era: 'post', type: 'special', src: 'https://terraria.fandom.com/wiki/Zenith' },
    ];
    const FALLBACK_SVG = `data:image/svg+xml;utf8,
      <svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 128 128'>
        <rect width='100%' height='100%' fill='%230f1222'/>
        <g fill='%2396a2c2' font-family='monospace' font-size='12'>
          <text x='12' y='68'>no image</text>
        </g>
      </svg>`;

    const grid = document.getElementById('grid');
    const empty = document.getElementById('empty');
    const q = document.getElementById('q');
    const era = document.getElementById('era');
    const type = document.getElementById('type');

    function render(list) {
      grid.innerHTML = '';
      if (!list.length) { empty.hidden = false; return; }
      empty.hidden = true;
      for (const s of list) {
        const card = document.createElement('a');
        card.className = 'card';
        card.href = s.src || '#';
        card.target = s.src ? '_blank' : '';
        card.rel = s.src ? 'noopener' : '';
        card.setAttribute('data-era', s.era);
        card.setAttribute('data-type', s.type);
        card.setAttribute('aria-label', s.name);

        card.innerHTML = `
          <div class="thumb"><img loading="lazy" alt="${s.name}" src="${s.img}" onerror="this.src='${FALLBACK_SVG}'"/></div>
          <div class="meta">
            <span class="name">${s.name}</span>
            <span class="tag">${label(s.era)}</span>
          </div>
        `;
        grid.appendChild(card);
      }
    }

    function label(x) { return x === 'pre' ? 'Pre-Hardmode' : x === 'hard' ? 'Hardmode' : x === 'post' ? 'Post-ML' : x; }

    function applyFilters() {
      const term = q.value.trim().toLowerCase();
      const e = era.value; const t = type.value;
      const out = SWORDS.filter(s =>
        (e === 'all' || s.era === e) &&
        (t === 'all' || s.type === t) &&
        (!term || s.name.toLowerCase().includes(term))
      );
      render(out);
    }

    q.addEventListener('input', applyFilters);
    era.addEventListener('change', applyFilters);
    type.addEventListener('change', applyFilters);

    // Початковий рендер
    render(SWORDS);
  </script>
</body>
</html>
