# FINAL PACKAGE — Panchayat/Village Weather Downscaling (Gopalpur, Odisha pilot)

Everything from this project, in one place. Read this file first.

---

## 1. The ONE file to open for your SIH demo

```
odisha_pilot/gopalpur_windy_style_map.html
```

Double-click it — opens in any browser, no install, no login, works
offline. This is the flagship demo:

- Dark, Windy.com-style animated map of the real villages around
  Gopalpur / IISER Berhampur, Ganjam district, Odisha.
- Top banner shows the single **block-level forecast** (what IMD gives
  today) with a "downscaled to 9 villages below" arrow — makes the
  before/after story obvious to judges in one glance.
- Three switchable layers: **Wind / Rainfall / Temperature** — click a
  button top-right, colors update instantly.
- Click any village dot for its full numbers.
- Live IST clock, bottom-right, for a "real system" feel.
- Small amber badge honestly marks this as an illustrative demo, not a
  live IMD feed — keep it visible; judges will ask, and answering
  "this is a working prototype with representative data, ready for real
  IMD feeds" is a stronger answer than pretending it's live.

**The one-sentence pitch to say out loud:** *"IMD gives us one forecast
for the whole block. We use elevation, distance from the coast, and
local history to split that one number into a different, more accurate
forecast for every village inside it — this is what that looks like."*

---

## 2. The other maps (same idea, different depth)

| File | What it shows |
|---|---|
| `odisha_pilot/gopalpur_village_weather_map.html` | Simpler, lighter version of the same 9 villages — a fallback if the dark animated version is too much for a slower laptop/projector |
| `data/output/panchayat_weather_map.html` | A different pilot block (synthetic, near Patna, Bihar) — shows the same downscaling idea but with numbers that came from an actual trained machine-learning model, not hand-set rules |
| `data/output/panchayat_map.png` | Static image version of the Bihar map, for slides that can't embed HTML |

---

## 3. The working system behind the demo (for technical judges / your own dev work)

The `src/` folder is a complete, tested, end-to-end pipeline — not just
the map. It was built and run in this order, and each stage's output is
included:

1. `src/make_sample_data.py` — builds a realistic 3-year practice dataset
2. `src/baseline.py` — the "just copy the block value" baseline every
   real model has to beat
3. `src/train.py` — trains the actual downscaling model (XGBoost:
   a two-stage rain model + separate temperature/humidity models)
4. `src/evaluate.py` — the honest scorecard, baseline vs model
5. `src/predict.py` — turns a new block forecast into village forecasts
6. `src/maps.py` — draws the result on a map
7. `src/advisory.py` — turns weather into plain farming advice
8. `app/api.py` + `app/dashboard.py` — a small web service + dashboard

**The honest result** (`data/output/evaluation_summary.csv`):
temperature downscaling improved ~70-75% over the naive baseline;
rainfall and humidity did not beat the baseline on this practice
dataset. Good to know before promising judges the model beats IMD on
everything — say what it's proven to do well (temperature) and what
still needs real data to prove out (rainfall/humidity).

---

## 4. What's real vs illustrative — say this plainly if asked

- **Village locations**: real (pulled from actual map data for Ralaba,
  Korapalli, Gopalpur, Tulu, Indrakhi, Markandi, Golanthara,
  Biswanathpur, Bahadur Pentho).
- **Weather numbers on the Odisha map**: illustrative, following one
  simple physical rule (closer to sea = cooler & more humid; further
  inland = hotter & drier).
- **Weather numbers on the Bihar map**: from an actually trained
  machine-learning model, but trained on invented practice data, not
  real IMD records.
- **Real IMD data** (SANKALP block forecasts, gridded rainfall/temperature)
  needs portal registration/access this environment doesn't have — the
  natural next step once you're past the pitch stage.
- **Real Panchayat boundary polygons** ARE freely available (see
  `README.md` → "Swapping in real data" for the exact source) — this is
  the easiest real-data upgrade to make first.

---

## 5. Suggested demo flow for SIH (3-4 minutes)

1. Open `gopalpur_windy_style_map.html`. Let the animation run a few
   seconds — it looks alive immediately.
2. Point at the top banner: "This one number is what farmers get today."
3. Click 2 villages — one coastal (Korapalli), one inland (Golanthara).
   Read out how different their actual conditions are.
4. Switch to the Temperature layer — the coast-to-inland color gradient
   is the clearest visual proof of the idea.
5. Say the scaling path out loud: "This is one block, 9 villages. The
   same pipeline — shown working end-to-end in the code — scales to
   every block in Odisha once we're connected to IMD's real data feed."
6. If a judge asks for numbers: open `evaluation_summary.csv` — real
   MAE/RMSE comparison, baseline vs model, no invented statistics.

---

## Full file list

```
FINAL_README.md                                <- you are here
START_HERE.md                                  <- earlier plain-language file index
README.md                                      <- full technical setup instructions

odisha_pilot/
  gopalpur_windy_style_map.html                <- FLAGSHIP DEMO
  gopalpur_village_weather_map.html            <- simpler fallback version
  build_odisha_map.py                          <- code that generated it

src/                                            <- full working pipeline (8 scripts)
app/                                            <- API + dashboard
models/                                         <- trained model files (.pkl)
data/processed/                                 <- training data, panchayat boundaries
data/output/                                    <- forecasts, maps, evaluation results
requirements.txt                                <- Python packages needed to run src/
```
