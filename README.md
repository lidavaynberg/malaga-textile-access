# Textile Collection Access in Málaga

> Is Málaga's textile collection system accessible enough — and if not, what should change?

A data project mapping access to textile recycling points across the city of Málaga, Spain, in the context of the EU Waste Framework Directive that obliged all Spanish municipalities to provide separate textile collection from January 1, 2025.

**Final project for the course *Practical Data Science: Tools for Social Change*.**

---

## The Problem

The textile industry is one of the most resource-intensive sectors on the planet — responsible for an estimated 10% of global CO₂ emissions, large freshwater use, and microplastic pollution of oceans. The rise of fast fashion has roughly doubled global clothing production over the past two decades while halving the time each garment is worn, turning textiles into one of the fastest-growing waste streams. What happens to clothes after we stop wearing them is no longer a niche question — it is a basic test of whether modern consumption can be made circular.

In 2024, Spain generated an estimated 922,068 tonnes of post-consumer textile waste — but only 12.9% was collected separately (118,952 tonnes, or 2.45 kg per capita) ([Moda re-, 2025][moda-re-2025]). The remaining ~87% went to mixed waste streams, ending up in landfill or incineration.

To close this gap, the revised EU Waste Framework Directive obliged every member state to set up separate textile collection by January 1, 2025 ([Directive (EU) 2018/851][eu-directive]). In the run-up to that deadline, Spanish municipalities scaled up infrastructure rapidly: the number of dedicated textile containers grew from 21,476 in 2021 to 29,692 in 2024 (+38.3%), and public tenders for textile waste services tripled — from 18 in 2022 to 58 in 2024.

More than a year after the deadline, the infrastructure is in place. Looking at what was built, this project asks:

**Is the resulting system accessible enough for residents to actually use it?**

Public reporting at national and regional level tracks tonnes collected — not how reachable the system is from a citizen's doorstep. The same national report acknowledges that "the inequality observed in tonnes collected is also reproduced in collection infrastructure," but documents this only at the regional (autonomous community) level. No published study has examined whether containers within a single Spanish city reach all residents within a reasonable walking distance, or whether access is distributed evenly across neighborhoods.

This project investigates accessibility for Málaga from two angles: **physical reach** (how much of the population lives close to a collection point) and **equity** (whether that reach varies systematically with neighborhood income). Andalucía as a whole collects only 2.01 kg per capita per year — 18% below the national average — and if a low collection rate coexists with high physical accessibility, the bottleneck likely lies elsewhere: in communication, behavior, or in the fit of the container-based model itself.

---

## The Question

Concretely, this project asks:

1. **Coverage** — what share of Málaga's population lives within walking distance (≈300 m) of any textile collection point?
2. **Equity of access** — is that coverage distributed evenly across neighborhoods, or does it vary systematically with income?
3. **System fit** — given the level of access observed, is the container-based model on its own enough to reach the EU's separate-collection targets, or are complementary approaches needed (awareness campaigns, door-to-door schemes, on-demand pickup, in-store collection)?
4. **Action** — where, and through which type of intervention, can access be improved most effectively?

---

## Data

| Source | What it provides | Format |
|---|---|---|
| Limpieza de Málaga, S.A.M. (LIMASAM) | Active street-container locations in Málaga (March 2026 extract). Covers both operators currently holding the municipal contract — East West Productos Textiles S.L. and Fundación Pueblo para Pueblo (Humana) — combined in a single dataset, without operator-level distinction. | CSV |
| Ayuntamiento de Málaga — open data portal | Originally intended source for municipal textile containers. Found to be outdated and incomplete; superseded by the LIMASAM extract above (see *Notes on data sources* below). | CSV, GeoJSON |
| Local private operators (non-tendered) | None identified in Málaga so far. The street-container network appears to be fully covered by the two operators awarded the municipal tender. To be confirmed. | — |
| Brand store collection points | Inditex, H&M, Decathlon, Mango pilot scheme (April 2025) | Scraped from brand websites |
| Charity collection points | Cáritas/Moda re-, Humana, Roba Amiga | Scraped + geocoded |
| Ayuntamiento de Málaga — cartography | District and neighborhood (`barrio`) boundaries | GeoJSON |
| INE — Atlas de Distribución de Renta de los Hogares 2023 | Average net income by census section | CSV (downloaded from INEbase) |
| INE — Padrón | Population by census section | CSV |
| OpenStreetMap (via OSMnx) | Pedestrian network for walking-distance computation | Graph |

### Notes on data sources

The project initially planned to use the textile-container dataset published on Málaga's open data portal. On inspection, this dataset was found to be outdated and incomplete. Up-to-date container locations were obtained directly from LIMASAM (Limpieza de Málaga, S.A.M.), the municipal cleaning company, in March 2026. 
This discrepancy is itself a finding: open-data publication lags real-world infrastructure. This limits both the public's ability to audit the system and its practical usefulness — residents, tourists, and third-party tools alike depend on current data to find the nearest container, and Málaga's high share of temporary visitors makes this especially relevant.

The LIMASAM extract aggregates containers from both operators currently holding the municipal contract (East West Productos Textiles S.L. and Fundación Pueblo para Pueblo / Humana) without distinguishing between them. This is acceptable for the present analysis, which focuses on access rather than on operator-level comparison.

---

## Approach

The analysis is built in four layers:

1. **Unify the collection-point dataset** — merge official, private, brand, and charity sources into one geocoded layer with provenance tagging (data source, last verified).
2. **Compute access** — for each census section, calculate walking distance to the nearest collection point using OpenStreetMap's pedestrian network. Aggregate to neighborhoods. Report population coverage at several distance thresholds (200, 300, 500 m).
3. **Cross with equity** — overlay coverage metrics with INE income data to identify systematic gaps. Visualize side-by-side and quantify with simple inequality measures.
4. **Benchmark against outcomes** — compare the measured access in Málaga with regional collection performance reported in the national report (Andalucía: 2.01 kg/capita, 18% below national average) to assess whether observed access is consistent with observed collection rates, or whether the gap points to non-infrastructural causes (communication, behavior, model fit).

A final optional step proposes either new collection-point locations (greedy facility-location, prioritizing under-covered areas) or complementary intervention zones (where awareness or door-to-door schemes may be more cost-effective than additional containers).

### Tech stack

- **Python**: pandas, geopandas, shapely, OSMnx, networkx
- **Visualization**: Plotly (express + graph_objects)
- **Delivery**: Jupyter notebooks with rendered visuals on GitHub. *Stretch goal: an interactive Streamlit app deployed to Streamlit Community Cloud, if time allows.*

---

## Repository structure

```
malaga-textile-access/
├── README.md
├── requirements.txt
├── data/
│   ├── raw/              # original downloads, never modified
│   ├── interim/          # cleaning intermediates
│   └── processed/        # analysis-ready
├── notebooks/
│   ├── 01_load_containers.ipynb
│   ├── 02_geocode_brands_charities.ipynb
│   ├── 03_boundaries_and_income.ipynb
│   ├── 04_coverage_analysis.ipynb
│   └── 05_access_and_equity_metrics.ipynb
├── src/
│   ├── data_loaders.py
│   ├── geo.py
│   └── metrics.py
├── app/                  # optional: Streamlit app if time allows
│   └── streamlit_app.py
└── docs/
    └── presentation.pdf
```

---

## How to run

> *Section to be expanded as the code stabilizes.*

```bash
git clone https://github.com/lidavaynberg/malaga-textile-access.git
cd malaga-textile-access
pip install -r requirements.txt
jupyter lab
```

Notebooks under `notebooks/` are numbered in the order they should be run. Rendered outputs are committed to the repository so the analysis can be reviewed on GitHub without running the code.

*If the optional Streamlit app is built, it will be hosted at: <URL to be added>.*

---

## Limitations

- **No tonnage data per container.** Public reporting of textile collection in Spain is aggregated at the regional level only. This project measures **access**, not actual usage.
- **Container data has estimation gaps even at the national level.** The 2025 national report classifies all Andalucía container counts as estimates rather than direct data. This project uses primary container data shared by a local operator, which provides higher resolution than the national estimate but covers a single city only.
- **Walking distance is an approximation.** Real behavior depends on car ownership, age, mobility, and habit, none of which are modeled here.
- **Brand and charity points may be incomplete.** They are scraped at a single point in time; some entries may be missing or out of date.
- **Income is measured at census-section level.** Households inside the same section can have different realities.

---

## Next steps

- Validate the proposed interventions (new locations and/or complementary models) with a local operator or the Ayuntamiento.
- Pair this access analysis with behavioral data — collection tonnage by container, awareness surveys, participation rates — to test whether low collection in well-accessed areas indicates a communication or model-fit problem.
- Explore complementary collection models documented in EU and national reports — door-to-door, on-demand pickup, mandatory in-store take-back — and identify the urban contexts where each fits best.
- Extend the methodology to other Spanish cities with open data (Madrid, Santa Cruz de Tenerife, Sevilla).
- Compare cities to build a national picture as the EU EPR scheme is implemented (expected late 2026 / early 2027).

  
---

## Sources & references

- Cáritas / Moda re- (2025). [*Situación actual del sector de la recogida y tratamiento de la ropa usada en España*](https://modare.org/wp-content/uploads/2026/05/Informe-Recogida.pdf). Third edition, December 2025.
- [Directive (EU) 2018/851](https://eur-lex.europa.eu/eli/dir/2018/851/oj) amending Directive 2008/98/EC on waste, which introduced the obligation for separate textile collection by 1 January 2025.
- [Ley 7/2022, de 8 de abril, de residuos y suelos contaminados para una economía circular](https://www.boe.es/buscar/act.php?id=BOE-A-2022-5809) — Spanish transposition of the EU framework, including the mandate for a textile EPR scheme.
- Instituto Nacional de Estadística (2025). [*Atlas de Distribución de Renta de los Hogares*](https://www.ine.es/dyngs/INEbase/operacion.htm?c=Estadistica_C&cid=1254736177088&menu=ultiDatos&idp=1254735976608), 2023 edition.
- Ayuntamiento de Málaga. [*Portal de datos abiertos*](https://datosabiertos.malaga.eu/dataset/contenedores-de-ropa) — textile containers, district and neighborhood boundaries.
- [OpenStreetMap](https://www.openstreetmap.org/) contributors, accessed via OSMnx, for the pedestrian network used in walking-distance calculations.

---

## Author

Lidia Vainberg — final project, [*Practical Data Science: Tools for Social Change*](https://smolny.org/2025/11/27/practical-data-science-tools-for-social-change/), 2026.

[moda-re-2025]: https://modare.org/wp-content/uploads/2026/05/Informe-Recogida.pdf
[eu-directive]: https://eur-lex.europa.eu/eli/dir/2018/851/oj
