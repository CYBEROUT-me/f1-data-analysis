# Дашборд у Tableau Public

Файли в цій папці створює ноутбук `notebooks/analysis.ipynb` (розділ 7):

| Файл | Що містить |
|---|---|
| `f1_starts.csv` | один рядок = один старт пілота (25 518 рядків, 1950–2025) |
| `f1_seasons.csv` | підсумки сезонів: гонки, пілоти, частка сходів, різні переможці, частка перемог найсильнішої команди |
| `f1_pit_stops.csv` | піт-стопи 1994–2025 (без зупинок понад 120 с) |

Основні поля `f1_starts.csv`: `Year`, `Decade`, `Grand Prix`, `Circuit`, `Circuit Type`, `Circuit Country`,
`Latitude`, `Longitude`, `Driver`, `Nationality`, `Constructor`, `Grid`, `Finish`, `Status`, `DNF Cause`,
`Points`, `Driver Age`, `Age Group`, `Grid Group`. Поля `Win`, `Podium`, `DNF`, `Home Race` – прапорці 0/1,
тому їх середнє одразу дає частку (наприклад, `AVG(DNF)` – частка сходів).

## Структура дашборда «Формула-1: 76 сезонів у цифрах»

Обчислюване поле (Analysis → Create Calculated Field): `Races` = `COUNTD([Race Id])`.

### Аркуші

1. **Карта трас.** `Longitude` → Columns, `Latitude` → Rows (обидва Dimension), Marks: Map.
   `Circuit` → Detail, `Races` → Size, `Circuit Type` → Color. Tooltip: країна, кількість гонок.
2. **Сходи по сезонах.** `Year` (Dimension, Continuous) → Columns, `AVG(DNF)` → Rows, формат – відсотки.
   `DNF Cause` → Color, щоб побачити причини.
3. **Перемоги команд.** `Constructor` → Rows, `SUM(Win)` → Columns, сортування за спаданням,
   фільтр Top 10 за `SUM(Win)`, `Decade` → Color.
4. **Старт і перемога.** `Grid` (Dimension, фільтр 1–15) → Columns, `AVG(Win)` → Rows, формат – відсотки.
5. **KPI.** Текстова таблиця: `Races`, `COUNTD(Driver)`, `AVG(DNF)`, `AVG(Driver Age)` для поточного вибору.

### Інтерактивність (мінімум два елементи)

- Фільтр **Year** (Range of values, слайдер) → Apply to Worksheets → All Using This Data Source.
- Фільтр **Circuit Type** (чекбокси) → на всі аркуші.
- Фільтр **Constructor** (випадний список) → на всі аркуші.
- Dashboard → Actions → Add Action → Filter: клік по трасі на карті фільтрує інші аркуші.

### Компонування

Dashboard → Size: Automatic. Зверху назва й рядок KPI, ліворуч карта, праворуч графік сходів,
внизу перемоги команд і «Старт і перемога». Під назвою короткий текст: джерело (F1DB, CC BY 4.0)
і що означає кожен показник.

### Публікація

File → Save to Tableau Public As… → увійти в акаунт Tableau Public → назва «F1 1950–2025».
Посилання на опублікований дашборд додати в `README.md` проєкту та в пояснювальну записку (підрозділ 4.2),
скриншот зберегти сюди як `dashboard.png`.
