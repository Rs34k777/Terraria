<!doctype html>
<html lang="uk">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Terraria — Прехардмод: предмети по класах і боси</title>
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <header>
    <div>
      <h1>Terraria — Прехардмод: предмети по класах і боси</h1>
      <small>Швидкий локальний перегляд як в вікі — фільтруй, шукай, експортуй</small>
    </div>
    <div class="controls">
      <input id="search" class="search" placeholder="Пошук (назва предмета)" />
      <button id="exportJson" class="btn">Експорт JSON</button>
    </div>
  </header>

  <div class="filters" id="filters">
    <button data-class="All" class="filter-btn active">Усі</button>
    <button data-class="Melee" class="filter-btn">Воїн (Melee)</button>
    <button data-class="Ranged" class="filter-btn">Стрілець (Ranged)</button>
    <button data-class="Magic" class="filter-btn">Маг (Magic)</button>
    <button data-class="Summoner" class="filter-btn">Призивач (Summoner)</button>
    <button data-class="Bosses" class="filter-btn">Боси</button>
  </div>

  <main>
    <div class="top-row">
      <div style="flex:1">
        <h2 class="section-title">Предмети</h2>
        <div id="itemsGrid" class="grid"></div>
      </div>
      <aside class="sidebar">
        <h2 class="section-title">Боси Прехардмоду</h2>
        <div id="bosses" class="boss-list"></div>
      </aside>
    </div>
  </main>

  <footer>

  </footer>

  <script src="script.js"></script>
</body>
</html>
