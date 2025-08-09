<!doctype html>
<html lang="uk">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Terraria — Прехардмод: предмети по класах і боси</title>
  <style>
    :root{--bg:#0f1724;--card:#0b1220;--accent:#ffcc00;--muted:#94a3b8;--glass:rgba(255,255,255,0.03)}
    *{box-sizing:border-box}
    body{font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto; margin:0; background:linear-gradient(180deg,#071027 0%, #0b1220 100%); color:#e6eef8}
    header{padding:18px 20px;display:flex;gap:16px;align-items:center;border-bottom:1px solid rgba(255,255,255,0.03)}
    h1{font-size:18px;margin:0}
    .controls{display:flex;gap:8px;align-items:center;margin-left:auto}
    .search{background:var(--glass);border:1px solid rgba(255,255,255,0.04);padding:8px 10px;border-radius:8px;color:var(--muted)}
    .btn{background:transparent;border:1px solid rgba(255,255,255,0.06);padding:8px 10px;border-radius:8px;color:inherit;cursor:pointer}
    .filters{padding:12px 20px;display:flex;gap:8px;flex-wrap:wrap}
    .filter-btn{background:transparent;border:1px solid rgba(255,255,255,0.04);padding:6px 10px;border-radius:999px;cursor:pointer}
    .filter-btn.active{background:linear-gradient(90deg,#15324a,#1f7896);border-color:rgba(255,255,255,0.08)}
    main{padding:18px 20px}
    .grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(260px,1fr));gap:12px}
    .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border:1px solid rgba(255,255,255,0.03);padding:12px;border-radius:12px}
    .card h3{margin:0 0 6px 0;font-size:15px}
    .meta{font-size:13px;color:var(--muted);margin-bottom:8px}
    .tags{display:flex;gap:6px;flex-wrap:wrap}
    .tag{background:#081226;padding:4px 8px;border-radius:999px;font-size:12px;border:1px solid rgba(255,255,255,0.02)}
    .section-title{color:var(--accent);margin:12px 0 6px 0}
    footer{padding:18px 20px;color:var(--muted);font-size:13px;border-top:1px solid rgba(255,255,255,0.03)}
    .top-row{display:flex;gap:12px;align-items:center}
    .controls small{display:block;color:var(--muted);font-size:12px}
    .boss-list{display:flex;flex-direction:column;gap:8px}
    .boss-item{background:rgba(255,255,255,0.02);padding:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.03)}
    .actions{display:flex;gap:8px}
    pre{white-space:pre-wrap;word-break:break-word}
  </style>
</head>
<body>
  <header>
    <div>
      <h1>Terraria — Прехардмод: предмети по класах і боси</h1>
      <small style="color:var(--muted)">Швидкий локальний перегляд як в вікі — фільтруй, шукай, експортуй</small>
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
      <aside style="width:320px;margin-left:18px">
        <h2 class="section-title">Боси Прехардмоду</h2>
        <div id="bosses" class="boss-list"></div>
      </aside>
    </div>
  </main>

  <footer>
    Підказка: натисни на клас щоб відфільтрувати. Можна доповнити картинки та подробиці з Terraria Wiki.
  </footer>

  <script>
    // Дані — можна доповнювати
    const DATA = {
      classes: {
        Melee: {
          weapons: [
            {name: "Night's Edge", source: "Крафт"},
            {name: "Muramasa", source: "Dungeon"},
            {name: "Enchanted Sword", source: "Сундук в підземеллі"},
            {name: "Fiery Greatsword", source: "Крафт (Hellstone)"},
            {name: "Blade of Grass", source: "Jungle"},
            {name: "Obsidian Swordfish", source: "Fishing - Jungle"}
          ],
          armor: ["Shadow armor","Molten armor"],
          accessories: ["Obsidian Shield","Magma Stone"]
        },
        Ranged: {
          weapons: [
            {name: "Megashark", source: "Крафт (Shark Teeth + Guns)"},
            {name: "Clockwork Assault Rifle", source: "Event / Wall of Flesh (drops)"},
            {name: "Phoenix Blaster", source: "Gun Parts + Handgun"},
            {name: "Demon Bow", source: "Craft"}
          ],
          armor: ["Necro armor","Meteor armor"],
          accessories: ["Magic Quiver","Ranger Emblem"]
        },
        Magic: {
          weapons: [
            {name: "Water Bolt", source: "Dungeon"},
            {name: "Flamelash", source: "Craft"},
            {name: "Meteor Staff", source: "Meteorite"},
            {name: "Book of Skulls", source: "Skeletron drops"}
          ],
          armor: ["Necro armor","Meteor armor"],
          accessories: ["Mana Flower","Celestial Emblem"]
        },
        Summoner: {
          weapons: [
            {name: "Slime Staff", source: "Slimes"},
            {name: "Hornet Staff", source: "Queen Bee"},
            {name: "Imp Staff", source: "Imps (Underworld)"}
          ],
          armor: ["Bee armor","Spider armor"],
          accessories: ["Pygmy Necklace","Hercules Beetle"]
        }
      },
      bosses: [
        {name: "King Slime", summon: "Slime Crown / Slime Rain chance"},
        {name: "Eye of Cthulhu", summon: "Suspicious Looking Eye / Night conditions"},
        {name: "Eater of Worlds", summon: "Worm Food or break Shadow Orbs"},
        {name: "Brain of Cthulhu", summon: "Bloody Spine or break Crimson Hearts"},
        {name: "Queen Bee", summon: "Abeemination or break larva in Hive"},
        {name: "Skeletron", summon: "Talk to Old Man at dungeon entrance at night"},
        {name: "Wall of Flesh", summon: "Guide Voodoo Doll in Underworld lava"}
      ]
    };

    // DOM refs
    const itemsGrid = document.getElementById('itemsGrid');
    const bossesDiv = document.getElementById('bosses');
    const searchInput = document.getElementById('search');
    const filterBtns = Array.from(document.querySelectorAll('.filter-btn'));
    let activeFilter = 'All';

    function renderItems(filter = 'All', query = ''){
      itemsGrid.innerHTML = '';
      const classes = DATA.classes;
      const nodes = [];
      Object.keys(classes).forEach(clsName => {
        if(filter !== 'All' && filter !== clsName) return;
        const cls = classes[clsName];
        // weapons
        cls.weapons.forEach(w => {
          if(query && !w.name.toLowerCase().includes(query.toLowerCase())) return;
          const card = createCard(w.name, clsName, w.source, 'Weapon');
          nodes.push(card);
        });
        // armor
        (cls.armor || []).forEach(a => {
          if(query && !a.toLowerCase().includes(query.toLowerCase())) return;
          nodes.push(createCard(a, clsName, 'Armor set item', 'Armor'));
        });
        // accessories
        (cls.accessories || []).forEach(ac => {
          if(query && !ac.toLowerCase().includes(query.toLowerCase())) return;
          nodes.push(createCard(ac, clsName, 'Accessory', 'Accessory'));
        });
      });

      if(nodes.length === 0){
        itemsGrid.innerHTML = '<div style="color:var(--muted);padding:12px">Нічого не знайдено.</div>';
        return;
      }
      nodes.forEach(n => itemsGrid.appendChild(n));
    }

    function createCard(title, cls, source, type=''){
      const el = document.createElement('div');
      el.className = 'card';
      el.innerHTML = `<h3>${escapeHtml(title)}</h3>
                      <div class="meta">${escapeHtml(source)} • <strong>${escapeHtml(cls)}</strong></div>
                      <div class="tags"><span class="tag">${escapeHtml(type)}</span></div>`;
      return el;
    }

    function renderBosses(){
      bossesDiv.innerHTML = '';
      DATA.bosses.forEach(b => {
        const el = document.createElement('div');
        el.className = 'boss-item';
        el.innerHTML = `<strong>${escapeHtml(b.name)}</strong><div class="meta">${escapeHtml(b.summon)}</div>`;
        bossesDiv.appendChild(el);
      });
    }

    // utils
    function escapeHtml(s){
      return (s+'').replace(/[&<>"]/g, c=>({ '&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;' }[c]));
    }

    // events
    filterBtns.forEach(b => b.addEventListener('click', ()=>{
      filterBtns.forEach(x=>x.classList.remove('active'));
      b.classList.add('active');
      activeFilter = b.dataset.class;
      if(activeFilter === 'Bosses'){
        // show bosses only — clear items grid and highlight
        itemsGrid.innerHTML = '<div style="color:var(--muted);padding:12px">Секція босів праворуч. Натисни "Усі" щоб повернутися.</div>';
      } else {
        renderItems(activeFilter, searchInput.value.trim());
      }
    }));

    searchInput.addEventListener('input', ()=>{
      const q = searchInput.value.trim();
      if(activeFilter === 'Bosses') return; // search doesn't affect bosses here
      renderItems(activeFilter, q);
    });

    document.getElementById('exportJson').addEventListener('click', ()=>{
      const blob = new Blob([JSON.stringify(DATA, null, 2)], {type:'application/json'});
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url; a.download = 'terraria_prehardmode.json';
      document.body.appendChild(a); a.click(); a.remove(); URL.revokeObjectURL(url);
    });

    // initial render
    renderItems();
    renderBosses();
  </script>
</body>
</html>
