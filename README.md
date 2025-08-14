<html lang="ru">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Путешествие на Пхукет — Маршрут и фотографии</title>
  <meta name="description" content="Одностраничный сайт маршрута: Самара → Санкт‑Петербург → Москва → Пекин → Пхукет и обратно." />
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    body { font-family: Inter, system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif; }
    .card { border-radius: 1.25rem; box-shadow: 0 10px 30px rgba(2,6,23,.08); }
    .pill { border-radius: 9999px; padding: .25rem .75rem; background: #eef2ff; color: #3730a3; font-weight: 600; font-size: .85rem; }
    .grid-photo img { object-fit: cover; width: 100%; height: 100%; display: block; }
  </style>

  <!-- Leaflet map library -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" crossorigin=""/>
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" crossorigin=""></script>

</head>
<body class="bg-gradient-to-b from-white via-slate-50 to-slate-100 text-slate-900">
  <header class="mx-auto max-w-6xl px-6 pt-10 pb-6">
    <div class="flex items-center justify-between">
      <div class="pill">Маршрут: Самара → СПб → Москва → Пекин → Пхукет</div>
      <a href="#gallery" class="text-sm font-semibold hover:underline">Фотогалерея</a>
    </div>
    <h1 class="mt-6 text-4xl md:text-6xl font-extrabold tracking-tight">Путешествие на Пхукет</h1>
    <p class="mt-3 text-lg md:text-xl text-slate-600">15 октября — 2 ноября · длинный, но увлекательный путь к солнцу и морю.</p>
  </header>

  <main class="mx-auto max-w-6xl px-6 pb-16">
    <!-- Расписание -->
    <section class="card bg-white p-6 md:p-8">
      <h2 class="text-2xl md:text-3xl font-bold">Расписание перелётов</h2>
      <ol class="mt-4 space-y-4">
        <li class="border-l-4 border-indigo-500 pl-4">
          <div class="font-semibold">Самара → Санкт‑Петербург</div>
          <div class="text-slate-600">15 октября, 19:15 - 15 октября, 20:35 — Начало нашего большого путешествия🎉</div>
        </li>
        <li class="border-l-4 border-indigo-500 pl-4">
          <div class="font-semibold">Санкт‑Петербург → Москва</div>
          <div class="text-slate-600">20 октября, 07:00 - 20 октября, 14:00</div>
        </li>
        <li class="border-l-4 border-indigo-500 pl-4">
          <div class="font-semibold">Москва → Пекин</div>
          <div class="text-slate-600">20 октября, 20:45 - 21 октября, 08:55</div>
        </li>
        <li class="border-l-4 border-indigo-500 pl-4">
          <div class="font-semibold">Пекин → Пхукет</div>
          <div class="text-slate-600">21 октября, 12:45 - 21 октября, 17:45</div>
        </li>
        <li class="border-l-4 border-emerald-500 pl-4">
          <div class="font-semibold">Пхукет → Пекин</div>
          <div class="text-slate-600">31 октября, 18:25 - 1 ноября, 00:50</div>
        </li>
        <li class="border-l-4 border-emerald-500 pl-4">
          <div class="font-semibold">Пекин → Москва</div>
          <div class="text-slate-600">1 ноября, 14:15 - 1 ноября, 17:40</div>
        </li>
        <li class="border-l-4 border-rose-500 pl-4">
          <div class="font-semibold">Москва → Самара</div>
          <div class="text-slate-600">2 ноября, 15:55 - 2 ноября, 18:50</div>
        </li>
      </ol>
    </section>

    <!-- Карта маршрута -->
    <section id="map" class="mt-10 card bg-white p-6 md:p-8">
      <h2 class="text-2xl md:text-3xl font-bold mb-3">Карта маршрута</h2>
      <div id="leaflet-map" style="height: 480px; border-radius: 1rem; overflow: hidden;"></div>
      <p class="mt-3 text-sm text-slate-600">Кликните по маркерам, чтобы увидеть дату и заметку. Линия отражает последовательность перелётов.</p>
    </section>

    <script>
      // Точки маршрута (включая обратный путь и возвращение в Самару)
      const ROUTE_POINTS = [
        {name: "Самара", lat: 53.195, lng: 50.101, date: "15 октября, 19:15", note: "Вылет в Санкт‑Петербург"},
        {name: "Санкт‑Петербург", lat: 59.9343, lng: 30.3351, date: "21 октября, 07:00", note: "Перелёт в Москву"},
        {name: "Москва", lat: 55.7558, lng: 37.6173, date: "21 октября, 21:45", note: "Перелёт в Пекин"},
        {name: "Пекин", lat: 39.9042, lng: 116.4074, date: "22 октября, 08:45", note: "Перелёт на Пхукет"},
        {name: "Пхукет", lat: 7.8804, lng: 98.3923, date: "31 октября, 15:45", note: "Обратный перелёт в Пекин"},
        {name: "Пекин", lat: 39.9042, lng: 116.4074, date: "1 ноября, 10:45", note: "Перелёт в Москву"},
        {name: "Москва", lat: 55.7558, lng: 37.6173, date: "2 ноября, 15:30", note: "Возвращение в Самару"},
        {name: "Самара", lat: 53.195, lng: 50.101, date: "—", note: "Дом"}
      ];

      const map = L.map('leaflet-map');
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; OpenStreetMap contributors'
      }).addTo(map);

      const latlngs = ROUTE_POINTS.map(p => [p.lat, p.lng]);
      const poly = L.polyline(latlngs, {weight: 4, opacity: 0.9}).addTo(map);

      const group = L.featureGroup();
      ROUTE_POINTS.forEach((p, i) => {
        const marker = L.marker([p.lat, p.lng]).bindPopup(`<b>${i+1}. ${p.name}</b><br>${p.date}<br>${p.note}`);
        group.addLayer(marker);
      });
      group.addTo(map);

      map.fitBounds(poly.getBounds(), {padding: [30, 30]});
    </script>

    <!-- Фотогалерея -->
    <section id="gallery" class="mt-10">
      <h2 class="text-2xl md:text-3xl font-bold mb-4">Города путешествия</h2>
      <div class="grid md:grid-cols-2 gap-6">
        <!-- Самара -->
        <figure class="card overflow-hidden bg-white">
          <div class="grid-photo h-64 md:h-80"><img src="https://upload.wikimedia.org/wikipedia/commons/b/b1/Samara_-_View_from_Volga_%282008-07-13%29.jpg" alt="Самара — вид с Волги"></div>
          <figcaption class="p-4 text-sm text-slate-600">
            Самара — вид с Волги.</a>.
          </figcaption>
        </figure>
        <!-- Санкт-Петербург -->
        <figure class="card overflow-hidden bg-white">
          <div class="grid-photo h-64 md:h-80"><img src="https://upload.wikimedia.org/wikipedia/commons/1/10/Saint_Petersburg%2C_Russia%2C_Neva_River%2C_Panoramic_view.jpg" alt="Санкт‑Петербург — Нева, панорама"></div>
          <figcaption class="p-4 text-sm text-slate-600">
            Санкт‑Петербург, Нева. Город на Неве с величественной архитектурой, дворцами и белыми ночами.</a>.
          </figcaption>
        </figure>
        <!-- Москва -->
        <figure class="card overflow-hidden bg-white">
          <div class="grid-photo h-64 md:h-80"><img src="https://upload.wikimedia.org/wikipedia/commons/5/51/Moscow_City_2019.jpg" alt="Москва‑Сити — деловой центр"></div>
          <figcaption class="p-4 text-sm text-slate-600">
            Москва‑Сити. Столица России с Красной площадью, Кремлём и современными небоскрёбами.</a>.
          </figcaption>
        </figure>
        <!-- Пекин -->
        <figure class="card overflow-hidden bg-white">
          <div class="grid-photo h-64 md:h-80"><img src="https://upload.wikimedia.org/wikipedia/commons/8/8a/Skyline_of_Beijing_CBD_from_the_southeast_%2820210907094201%29.jpg" alt="Пекин — CBD, панорама"></div>
          <figcaption class="p-4 text-sm text-slate-600">
            Пекин, CBD. Столица Китая с Запретным городом, Великой китайской стеной и современными районами.</a>.
          </figcaption>
        </figure>
        <!-- Пхукет -->
        <figure class="card overflow-hidden bg-white md:col-span-2">
          <div class="grid-photo h-72 md:h-[28rem]"><img src="https://upload.wikimedia.org/wikipedia/commons/b/bd/Patong_Beach_in_Phuket.jpg" alt="Патонг-Бич, Пхукет"></div>
          <figcaption class="p-4 text-sm text-slate-600">
            Патонг‑Бич, Пхукет. Тайский остров с пляжами, рынками и тёплым морем.</a>.
          </figcaption>
        </figure>
      </div>
    </section>

    <section class="mt-10 text-sm text-slate-500">
      <p>Контакты для связи: <a class="underline" href="mailto:alekseenko_r@mail.ru">alekseenko_r@mail.ru</a> · <a class="underline" href="tel:+79277521934">+7 927 752‑19‑34</a></p>
    </section>
  </main>

  <footer class="mt-8 pb-12 text-center text-xs text-slate-500">
    Сайт создан для того чтобы Максим не купил у начальницы экскурсию по ШАНХАЮ, маршрут Самара → Санкт‑Петербург → Москва → Пекин → Пхукет (и обратно).
  </footer>
</body>
</html>
