<html lang="uk">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Terraria — Прехардмод: предмети по класах і боси</title>
  <meta name="description" content="Локальний перегляд прехардмодних предметів Terraria по класах і босів. Готово для GitHub Pages." />
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <header class="site-header">
    <div>
      <h1>Terraria — Прехардмод: предмети по класах і боси</h1>
      <p class="muted">Локальний перегляд — фільтри, пошук, експорт JSON</p>
    </div>
    <div class="controls">
      <input id="search" class="search" type="search" placeholder="Пошук (назва предмета)" aria-label="Пошук" />
      <button id="exportJson" class="btn">Експорт JSON</button>
    </div>
  </header>

  <nav class="filters" id="filters" aria-label="Фільтри класів">
    <button data-class="All" class="filter-btn active">Усі</button>
    <button data-class="Melee" class="filter-btn">Воїн (Melee)</button>
    <button data-class="Ranged" class="filter-btn">Стрілець (Ranged)</button>
    <button data-class="Magic" class="filter-btn">Маг (Magic)</button>
    <button data-class="Summoner" class="filter-btn">Призивач (Summoner)</button>
    <button data-class="Bosses" class="filter-btn">Боси</button>
  </nav>

  <main class="container">
    <section class="items-section" aria-labelledby="items-title">
      <h2 id="items-title">Предмети</h2>
      <div id="itemsGrid" class="grid" role="list" aria-label="Список предметів"></div>
    </section>

    <aside class="sidebar" aria-labelledby="bosses-title">
      <h2 id="bosses-title">Боси Прехардмоду</h2>
      <div id="bosses" class="boss-list" role="list" aria-label="Список босів"></div>
    </aside>
  </main>

  <footer class="site-footer">
  </footer>

  <script src="script.js"></script>
</body>
</html>
