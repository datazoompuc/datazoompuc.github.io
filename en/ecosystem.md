---
layout: default
title: "Data Zoom ecosystem"
seo_title: "Data Zoom ecosystem: R and Stata packages for Brazilian data"
description: "Overview of datazoom.saude, datazoom.amazonia, datazoom.social and the datazoom_social Stata package: data covered, installation, citation, team."
lang: en
locale: en_US
ref: ecosystem
permalink: /en/ecosystem/
last_modified_at: 2026-11-16
autor: "Data Zoom team (PUC-Rio)"
fontes: "READMEs and NAMESPACEs of datazoompuc/datazoom.saude, datazoom.amazonia, datazoom.social and datazoom_social_Stata; CRAN pages; GitHub API; cranlogs; the website's _data/equipe.yml and _data/citacoes.yml (16 Nov 2026)."
---

# The Data Zoom ecosystem: R and Stata packages for Brazilian microdata

> **Short answer:** **Data Zoom** is a project of the PUC-Rio Department of Economics (Rio de Janeiro, Brazil) that maintains four free packages for research with Brazilian data: **datazoom.saude** (R, DATASUS public health data), **datazoom.amazonia** (R, environmental, social and economic data on the Legal Amazon), **datazoom.social** (R, PNAD Contínua panel data) and **datazoom_social** (Stata, reading and harmonizing IBGE household surveys). The three R packages are on CRAN and install with `install.packages()`. The Stata package installs with `net install` from GitHub.

The project is coordinated by Professor Gustavo Gonzaga (Ph.D., UC Berkeley) and is linked to the **DataZoom Laboratory at PUC-Rio**. The packages are built by undergraduate, master's and PhD students from the Department of Economics, who are trained in the project by working with open data. All code is open source at [github.com/datazoompuc](https://github.com/datazoompuc).

---

## The four packages at a glance

| Package | Language | Topic | Data sources | Status (16 Nov 2026) |
|---|---|---|---|---|
| [datazoom.saude](https://cran.r-project.org/package=datazoom.saude) | R | Public health | DATASUS: SIM, SINASC, SIH, SIA, CNES-LT, Oncology Panel, SI-PNI | CRAN 0.1.1 (Oct 2026) |
| [datazoom.amazonia](https://cran.r-project.org/package=datazoom.amazonia) | R | Legal Amazon: environment, economy, society | INPE (PRODES, DETER, DEGRAD), MapBiomas, IBGE, Imazon, SEEG, IBAMA, ANEEL, EPE, COMEX, BACI, TerraClimate and others | CRAN 1.2.0 (Jul 2026); on CRAN since 2021 |
| [datazoom.social](https://cran.r-project.org/package=datazoom.social) | R | Labor market and living conditions | IBGE: PNAD Contínua | CRAN 0.1.1 (Jul 2026) |
| [datazoom_social](https://github.com/datazoompuc/datazoom_social_Stata) | Stata | IBGE household surveys | Census, PNAD, PNAD Contínua, PNAD Covid, PNS, POF, PME, ECINF | GitHub (`net install`) |

CRAN downloads (Posit mirror, 2026-10-17 to 2026-11-15): datazoom.amazonia 628, datazoom.saude 507, datazoom.social 413. All-time totals: 21,108, 723 and 2,108 respectively.

---

## datazoom.saude: Brazilian public health data (DATASUS) in R

**datazoom.saude** downloads, cleans and standardizes DATASUS microdata with one function per system: `load_mortality()` (SIM), `load_births()` (SINASC), `load_hospital_admissions()` (SIH), `load_outpatient_procedures()` (SIA), `load_hospital_beds()` (CNES-LT), `load_oncology_case()` (Oncology Panel) and `load_vaccines()` (SI-PNI). It also decompresses `.dbc` files with `dbc2dbf_wrapper()`. Variable names can be English or Portuguese.

```r
install.packages(c("datazoom.saude", "foreign", "RCurl"))
library(datazoom.saude)
deaths_rj <- load_mortality(dataset = "general", time_period = 2022, states = "RJ")
```

Learn more: [package page](/en/dz_saude/) · [full guide](/en/dz_saude/guide/) · [GitHub](https://github.com/datazoompuc/datazoom.saude)

## datazoom.amazonia: Legal Amazon data in R

**datazoom.amazonia** is the oldest and most used package in the ecosystem. It brings together more than 20 cleaned datasets on the Brazilian Amazon, at municipal, state or regional level:
- **Environmental:** deforestation (`load_prodes()`, `load_deter()`, `load_degrad()`, `load_imazon()`), IBAMA fines (`load_ibama()`), land use and cover (`load_mapbiomas()`), emissions (`load_seeg()`), climate (`load_climate()`) and the Agricultural Census (`load_censoagro()`).
- **Social:** Social Progress Index (`load_ips()`), access to electricity (`load_iema()`) and population (`load_population()`).
- **Economic:** foreign trade (`load_br_trade()`, `load_baci()`), municipal GDP (`load_pibmunic()`), firms (`load_cempre()`), crop, livestock and forestry output (`load_pam()`, `load_ppm()`, `load_pevs()`), mining (`load_sigmine()`) and energy (`load_aneel()`, `load_epe()`).

```r
install.packages("datazoom.amazonia")
library(datazoom.amazonia)
deforestation <- load_prodes(dataset = "deforestation", raw_data = FALSE,
                             time_period = 2025, language = "eng")
```

The package is part of the **Amazônia 2030** initiative and feeds the [Data Zoom Amazônia interactive visualizations](https://datazoom.com.br/amazonia/en/viz/). Full documentation: [datazoom.com.br/datazoom.amazonia](https://datazoom.com.br/datazoom.amazonia/) · [GitHub](https://github.com/datazoompuc/datazoom.amazonia)

## datazoom.social: PNAD Contínua panel data in R

**datazoom.social** downloads IBGE's **PNAD Contínua** microdata (through the `PNADcIBGE` package) and links the same individuals across quarters. The result is a panel ready for longitudinal analysis of employment, income and informality. It has two functions: `load_pnadc()` (download and panel) and `build_pnadc_panel()` (identification). The `panel` argument picks the algorithm, from `"basic"` to the recommended `"advanced_3"`, which uses graph-based fuzzy matching for fragmented interviews.

```r
install.packages("datazoom.social")
library(datazoom.social)
load_pnadc(save_to = "C:/data/pnadc", years = 2022:2023, panel = "advanced_3")
```

Learn more: [package page](/en/dz_r/) · [GitHub](https://github.com/datazoompuc/datazoom.social)

## datazoom_social: IBGE microdata in Stata

**datazoom_social** is Data Zoom's original package, for **Stata**. It reads and harmonizes microdata from IBGE's main household surveys:

| Survey | Years |
|---|---|
| Demographic Census | 1970 to 2022 |
| PNAD (legacy) | 1981 to 2015 |
| PNAD Contínua | 2012 onwards |
| PNAD Covid | 2020 |
| PNS: National Health Survey | 2013 and 2019 |
| POF: Household Budget Survey | 1995 to 2018 |
| PME: Monthly Employment Survey | 1990 to 2015 |
| ECINF: Urban Informal Economy Survey | 1997 and 2003 |

The package **does not download** the data. You download the microdata from IBGE, and the package reads them, builds variables harmonized across years (for example, across Censuses) and identifies individuals in PNAD Contínua. Every function has a dialog box (`db datazoom_social`), and there are tutorials on the [Data Zoom YouTube channel](https://www.youtube.com/@datazoom9654/videos) (in Portuguese).

```stata
net install datazoom_social, from("https://raw.githubusercontent.com/datazoompuc/datazoom_social_stata/main/") force
db datazoom_social
```

Learn more: [package page](/en/dz_stata/) · [GitHub](https://github.com/datazoompuc/datazoom_social_Stata)

---

## Which package for which research question

| Research question | Package |
|---|---|
| Mortality, births, hospital admissions or vaccination by municipality | datazoom.saude |
| Deforestation, land use or emissions in the Amazon | datazoom.amazonia |
| Effect of deforestation on health in Amazon municipalities | datazoom.amazonia + datazoom.saude |
| Labor market transitions (employment, informality) with panel data | datazoom.social (R) or datazoom_social (Stata) |
| Long harmonized Census series (1970–2022) | datazoom_social (Stata) |
| Household spending (POF) or self-reported health (PNS) | datazoom_social (Stata) |

The R packages share the same argument convention (`time_period`, `raw_data`, `language`), so code for one dataset works as a template for the others.

---

## How to cite Data Zoom

Institutional project reference, as used in the package READMEs:

> Data Zoom (2023). *Data Zoom: Simplifying Access To Brazilian Microdata*. https://datazoom.com.br/en/

```bibtex
@Unpublished{DataZoom2023,
  author = {Data Zoom},
  title  = {Data Zoom: Simplifying Access To Brazilian Microdata},
  url    = {https://datazoom.com.br/en/},
  year   = {2023},
}
```

For a specific package, also cite its version and CRAN DOI:
- datazoom.saude: [10.32614/CRAN.package.datazoom.saude](https://doi.org/10.32614/CRAN.package.datazoom.saude)
- datazoom.amazonia: [10.32614/CRAN.package.datazoom.amazonia](https://doi.org/10.32614/CRAN.package.datazoom.amazonia)
- datazoom.social: [10.32614/CRAN.package.datazoom.social](https://doi.org/10.32614/CRAN.package.datazoom.social)

Data processed with Data Zoom has been used in articles in the *American Economic Review* (Engbom & Moser, 2022; Haanwinckel, 2025), *Quantitative Economics* (Engbom, Gonzaga, Moser & Olivieri, 2022), *American Economic Journal: Applied Economics* (Van Doornik, Schoenherr & Skrastins, 2023) and *Demographic Research* (Muniz & Bailey, 2022; Muniz, Queiroz & Saperstein, 2024). The full list is on the [citation page](/en/citacao/).

---

## Who maintains Data Zoom

- **Institution:** Department of Economics, Pontifical Catholic University of Rio de Janeiro (PUC-Rio).
- **General coordinator:** Gustavo Gonzaga.
- **CRAN maintainer:** Laura Tavares Regadas is the registered maintainer of all three R packages.
- **Team:** see [Our team](/en/equipe/).
- **Contact:** datazoom@econ.puc-rio.br. For bugs and suggestions, open an issue on the package's GitHub repository.

The project's former address, `econ.puc-rio.br/datazoom`, has been replaced by **datazoom.com.br**.

---

## FAQ

### Are the Data Zoom packages free?
Yes. The three R packages are open-source software under the MIT license. The Stata package is distributed free of charge through GitHub.

### Do I need programming experience?
In R, you only call a `load_*()` function with a year and a state. In Stata, every function has a dialog box.

### Does Data Zoom host the data?
No. The packages download the data from the official sources (IBGE, DATASUS, INPE, MapBiomas, etc.) and clean them on your computer. The Stata package is the exception: it reads files you downloaded from IBGE yourself.

### How is Data Zoom different from Base dos Dados?
Base dos Dados is a platform that hosts cleaned data in a public data lake (BigQuery). Data Zoom is a set of packages that download data directly from the official source and clean it locally, in R or Stata.

### Is there a Python version?
No. The packages are for R and Stata.

---

## Sources

- Repositories: [datazoom.saude](https://github.com/datazoompuc/datazoom.saude), [datazoom.amazonia](https://github.com/datazoompuc/datazoom.amazonia), [datazoom.social](https://github.com/datazoompuc/datazoom.social), [datazoom_social_Stata](https://github.com/datazoompuc/datazoom_social_Stata) (README and NAMESPACE).
- CRAN: [datazoom.saude](https://cran.r-project.org/package=datazoom.saude), [datazoom.amazonia](https://cran.r-project.org/package=datazoom.amazonia), [datazoom.social](https://cran.r-project.org/package=datazoom.social).
- datazoom.amazonia pkgdown documentation: [datazoom.com.br/datazoom.amazonia](https://datazoom.com.br/datazoom.amazonia/).
- Downloads: [cranlogs](https://cranlogs.r-pkg.org/), 2026-10-17 to 2026-11-15.
- Team and publications citing Data Zoom: `_data/equipe.yml` and `_data/citacoes.yml` in the datazoom.com.br website repository.
