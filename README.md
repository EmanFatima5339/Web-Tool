# Pareto & Knee-Point Analyser

A fully browser-based optimisation tool that identifies the **Pareto frontier** and **optimal knee point** from any two-column CSV dataset — no server required.

## Live Demo

> Host `index.html` on GitHub Pages, Netlify Drop, or any static host.
> **Netlify Drop:** drag the `index.html` file to [app.netlify.com/drop](https://app.netlify.com/drop) — you get a live URL in seconds.

---

## What it does

| Feature | Description |
|---|---|
| CSV Upload | Drag-drop or click-to-upload any two-column CSV (comma / semicolon / tab delimited) |
| Pareto Frontier | Highlights non-dominated points in the objective space |
| Knee-Point Detection | Applies the **Kneedle algorithm** (max perpendicular distance from the chord) to identify the optimal trade-off |
| Three charts | Scatter + frontier · Kneedle distance bars · Normalised Pareto front with chord |
| Data table | All rows labelled as *dominated*, *pareto*, or *★ knee* |

---

## Algorithm

### Pareto Dominance (from SOCAR research)
Point `i` is Pareto-optimal if **no other point** simultaneously has lower X (cost) and higher Y (benefit):

```python
def pareto_mask(px, py, maximise_y=True):
    for i in range(n):
        for j in range(n):
            if lower_x[j] and better_y[j]:  # dominates i
                mark i as dominated
```

### Kneedle / Geometric Knee
The knee is the point with **maximum perpendicular distance** from the straight chord connecting the first and last Pareto-front points in normalised coordinates:

```python
def geometric_knee(x_vals, y_vals):
    xn, yn = minmax_norm(x_vals), minmax_norm(y_vals)
    chord_vector = last_point - first_point
    distances = [perpendicular_distance(p, chord) for p in points]
    return argmax(distances)
```

This is identical to the method in the SOCAR Well No. 1220 manuscript (Section 6.3–6.4).

---

## How to use

1. Open `index.html` in any modern browser (Chrome / Firefox / Edge / Safari)
2. Upload a CSV with two numeric columns, e.g.:

```
cost,quality
10,2
20,5
30,9
40,12
50,13
60,13.1
```

3. Select X-axis (cost/input) and Y-axis (benefit/output) columns
4. Choose whether Y should be maximised or minimised
5. Click **Run Analysis**

The tool runs entirely in the browser using [Pyodide](https://pyodide.org/) (Python compiled to WebAssembly) — no data leaves your machine.

---

## Tech stack

| Component | Technology |
|---|---|
| Runtime | [Pyodide 0.25](https://pyodide.org/) — Python in WebAssembly |
| Charts | [Chart.js 4.4](https://www.chartjs.org/) |
| Fonts | Google Fonts (Space Mono + DM Sans) |
| Hosting | Any static host (GitHub Pages / Netlify / Replit) |

Zero build step. Single HTML file.

---

## Deploy in 30 seconds (Netlify Drop)

1. Go to **[app.netlify.com/drop](https://app.netlify.com/drop)**
2. Drag `index.html` onto the page
3. Copy the live URL and share it

---

## Repository structure

```
├── index.html       ← the entire application
├── sample_data.csv  ← example dataset to test with
└── README.md
```

---

## Sample data

`sample_data.csv` contains a synthetic trade-off curve for testing.

---

*Based on the Kneedle algorithm and Pareto analysis from SOCAR Well No. 1220 research (Sections 6.2–6.4).*
