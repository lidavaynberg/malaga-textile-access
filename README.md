# Textile Collection Equity in Málaga

> Who has access to separate textile waste collection in Málaga — and who doesn't?

A data project mapping access to textile recycling points across the city of Málaga, Spain, in the context of the EU Waste Framework Directive that obliged all Spanish municipalities to provide separate textile collection from January 1, 2025.

**Final project for the course *Practical Data Science: Tools for Social Change*.**

---

## The Problem

In 2024, Spain generated an estimated 922,068 tonnes of post-consumer textile waste — but only 12.9% was collected separately (118,952 tonnes, or 2.45 kg per capita) ([Moda re-, 2025][moda-re-2025]). The remaining ~87% went to mixed waste streams, ending up in landfill or incineration.

To close this gap, the revised EU Waste Framework Directive obliged every member state to set up separate textile collection by January 1, 2025 ([Directive (EU) 2018/851][eu-directive]). In the run-up to that deadline, Spanish municipalities scaled up infrastructure rapidly: the number of dedicated textile containers grew from 21,476 in 2021 to 29,692 in 2024 (+38.3%), and public tenders for textile waste services tripled — from 18 in 2022 to 58 in 2024.

More than a year after the deadline, the infrastructure is in place. But a question that was rarely asked while these decisions were being made still has no public answer:

**Was this new infrastructure distributed equitably across neighborhoods?**

The same national report acknowledges that "the inequality observed in tonnes collected is also reproduced in collection infrastructure" — but documents this only at the regional (autonomous community) level. No published study has examined whether containers within a single Spanish city reach all residents equally, regardless of income.

This project investigates that question for Málaga. Andalucía as a whole collects only 2.01 kg per capita per year — 18% below the national average — and the EU framework warns that circular transitions must "not exacerbate existing inequalities." If access to recycling has become a privilege of wealthier districts, the textile transition is failing its social mandate.

---

## The Question

Concretely, this project asks:

1. **Coverage** — what share of Málaga's population lives within walking distance (≈300 m) of any textile collection point?
2. **Equity** — does coverage vary by neighborhood income? Are low-income districts under-served?
3. **Operator mix** — do different types of collection points (municipal containers, social-economy operators like Cáritas/Moda re-, foundations like Humana, commercial operators like East West, brand stores like Inditex/H&M/Decathlon) reach different demographics?
44. **Action** — where should new collection points be placed to maximize equity gains?

---

## Data
| Source | What it provides | Format |
|---|---|---|
| Limpieza de Málaga, S.A.M. (LIMASAM) | Active municipal textile container locations (May 2026) | CSV |
| Ayuntamiento de Málaga — open data portal | Originally intended source. Found to be outdated; replaced with direct request to LIMASAM (see Notes on data quality below). | CSV, GeoJSON |

| Local textile operator (private) | Up-to-date list of active containers, including non-municipal | CSV |
| Brand store collection points | Inditex, H&M, Decathlon, Mango pilot scheme (April 2025) | Scraped from brand websites |
| Charity collection points | Cáritas/Moda re-, Humana, Roba Amiga | Scraped + geocoded |
| Ayuntamiento de Málaga — cartography | District and neighborhood (`barrio`) boundaries | GeoJSON |
| INE — Atlas de Distribución de Renta de los Hogares 2023 | Average net income by census section | CSV (downloaded from INEbase) |
| INE — Padrón | Population by census section | CSV |
| OpenStreetMap (via OSMnx) | Pedestrian network for walking-distance computation | Graph |

### A note on data quality

The project initially planned to use the textile-container dataset published on Málaga's open data portal. On inspection, this dataset was found to be outdated and incomplete. Up-to-date container locations were obtained directly from LIMASAM (Limpieza de Málaga, S.A.M.), the municipal cleaning company. This discrepancy is itself a finding: open-data publication lags real-world infrastructure, which limits the public's ability to audit the textile collection system.
