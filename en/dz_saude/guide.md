---
layout: default
title: "datazoom.saude guide"
seo_title: "How to download DATASUS microdata in R: datazoom.saude guide"
description: "Step-by-step guide to download SIM, SINASC, SIH, SIA, CNES-LT, Oncology Panel and SI-PNI data in R with datazoom.saude: code, arguments and FAQ."
lang: en
locale: en_US
ref: dz_saude_guide
permalink: /en/dz_saude/guide/
image: /assets/img/hex_dzsaude.png
last_modified_at: 2026-11-16
autor: "Data Zoom team (PUC-Rio)"
fontes: "DESCRIPTION, NAMESPACE, R/*.R, vignettes/ and inst/extdata/manifest/v1/datasets_link.csv from datazoompuc/datazoom.saude (commit 98f127f); CRAN package page (v0.1.1)."
---

# How to download DATASUS microdata in R with datazoom.saude

> **Short answer:** to download Brazilian DATASUS microdata in R, install **datazoom.saude** with `install.packages(c("datazoom.saude", "foreign", "RCurl"))` and call the function for the system you need: `load_mortality()` (SIM), `load_births()` (SINASC), `load_hospital_admissions()` (SIH), `load_outpatient_procedures()` (SIA), `load_hospital_beds()` (CNES-LT), `load_oncology_case()` (Oncology Panel) or `load_vaccines()` (SI-PNI). Each function downloads the `.dbc` files from the DATASUS FTP server, decompresses them, stacks the requested states and years, and returns a clean tibble with English (`language = "eng"`) or Portuguese (`language = "pt"`) variable names.

**datazoom.saude** is a free R package (MIT license) from **Data Zoom**, a project of the PUC-Rio Department of Economics in Rio de Janeiro, Brazil. Version 0.1.1 has been on CRAN since 6 October 2026 (DOI [10.32614/CRAN.package.datazoom.saude](https://doi.org/10.32614/CRAN.package.datazoom.saude)). Source code: [github.com/datazoompuc/datazoom.saude](https://github.com/datazoompuc/datazoom.saude).

*Last updated: 16 Nov 2026. Examples checked against the package source code (development version 0.1.1.9000 and CRAN version 0.1.1).*

---

## Summary: one function per DATASUS system

| DATASUS system | What it contains | R function |
|---|---|---|
| **SIM**: Mortality Information System | Death certificates (general, fetal, infant, maternal, external causes) | `load_mortality()` |
| **SINASC**: Live Birth Information System | Live birth certificates: newborn and mother | `load_births()` |
| **SIH/SUS**: Hospital Information System | SUS-funded hospital admissions (AIH) | `load_hospital_admissions()` |
| **SIA/SUS**: Outpatient Information System | Outpatient procedures, APAC, RAAS | `load_outpatient_procedures()` |
| **CNES-LT**: National Registry of Health Facilities (Beds) | Beds per facility, monthly | `load_hospital_beds()` |
| **PO**: Oncology Panel (Painel de Oncologia) | Diagnosed and treated cancer cases | `load_oncology_case()` |
| **SI-PNI**: National Immunization Program | Vaccine doses applied per municipality | `load_vaccines()` |
| Local `.dbc` files | `.dbc` → `.dbf` decompression | `dbc2dbf_wrapper()` |

---

## How to install datazoom.saude

### Stable version (CRAN)

```r
install.packages(c("datazoom.saude", "foreign", "RCurl"))
library(datazoom.saude)
```

**Why install `foreign` and `RCurl` too?** Both are listed under `Suggests` in DESCRIPTION, so `install.packages("datazoom.saude")` on its own does not install them. But `load_mortality()`, `load_births()`, `load_hospital_admissions()`, `load_outpatient_procedures()`, `load_hospital_beds()` and `load_oncology_case()` all need them. Without them the function stops with `Package "foreign" must be installed to use this function.`

### Development version (GitHub or r-universe)

```r
# From r-universe (prebuilt binaries, no compiler needed):
install.packages("datazoom.saude",
                 repos = c("https://datazoompuc.r-universe.dev", "https://cloud.r-project.org"))

# Or straight from GitHub:
install.packages("remotes")
remotes::install_github("datazoompuc/datazoom.saude")
```

The package contains C code (the `.dbc` decompressor). On **Windows**, installing from GitHub needs [Rtools](https://cran.r-project.org/bin/windows/Rtools/). CRAN and r-universe ship binaries, so no compiler is needed there.

### CRAN 0.1.1 vs. the development version

| Topic | CRAN 0.1.1 | Development (0.1.1.9000) |
|---|---|---|
| Automatic vaccination download (`load_vaccines()`) | 1994–2022, through a headless browser (`chromote`), requires Google Chrome | 1994–2023, through direct HTTP requests with `curl`, no Chrome needed |
| Download links | Hard-coded | In a CSV manifest refreshed from GitHub at run time, with a fallback to the bundled copy |
| Dependencies | Imports `sf`, `XML` and `data.table` | Drops `sf`, `XML` and `data.table` |
| Year ranges | — | Corrected to match the files currently on the DATASUS FTP server (see table below) |

Source: the package's `NEWS.md`.

---

## Which DATASUS databases datazoom.saude covers

Year ranges come from the development version's manifest, `inst/extdata/manifest/v1/datasets_link.csv` (November 2026). They mirror the files on the DATASUS FTP server and can change when DATASUS adds or removes files.

| Function | `dataset` value | FTP file | Years available | State-level files |
|---|---|---|---|---|
| `load_mortality()` | `"general"` | DO (SIM-DO) | 1996–2024 | Yes (or the national file with `states = "all"`) |
| | `"fetal"` | DOFET | 1996–2025 | No: national only |
| | `"external_causes"` | DOEXT | 1996–2025 | No: national only |
| | `"infant"` | DOINF | 1996–2025 | No: national only |
| | `"maternal"` | DOMAT | 1996–2025 | No: national only |
| `load_births()` | (none) | DN (SINASC) | 1996–2024 | Yes |
| `load_hospital_admissions()` | `"reduced_aih"` | RD | 2008–2026 | Yes |
| | `"professional_services"` | SP | 2008–2026 | Yes |
| | `"rejected_aih"` | RJ | 2008–2026 | Yes |
| | `"rejected_aih_error"` | ER | 2011–2026 | Yes |
| `load_outpatient_procedures()` | `"ambulatory_production"` | PA | 2008–2026 | Yes |
| | `"bariatric_surgery"` | AB | 2008–2017 and 2025 | Yes |
| | `"bariatric_surgery_follow_up"` | ABO | 2014–2026 | Yes |
| | `"diverse_reports"` | AD | 2008–2026 | Yes |
| | `"medicines"` | AM | 2008–2026 | Yes |
| | `"nephrology"` | AN | 2008–2014 | Yes |
| | `"psychosocial"` | PS | 2012–2026 | Yes |
| | `"fistula_confection"` | ACF | 2014–2026 | Yes |
| | `"dialytic_treatment"` | ATD | 2014–2026 | Yes |
| | `"home_care"` | SAD | 2012–2018 | Yes |
| `load_hospital_beds()` | (none) | LT (CNES) | 2005–2026 | Yes |
| `load_oncology_case()` | (none) | PO | 2013–2026 | No: national only |
| `load_vaccines()` | (none) | SI-PNI | 1994–2023 automatic; 2024 onwards from a local file | One state per call |

If you ask for a year outside the range, the package issues a warning (`Option time_period must be in ...`) before trying to download.

---

## Arguments shared by the functions

| Argument | What it does | Default |
|---|---|---|
| `dataset` | Picks the sub-system (only in `load_mortality()`, `load_hospital_admissions()` and `load_outpatient_procedures()`) | required |
| `time_period` | A year or vector of years, e.g. `2020` or `2015:2020` | required |
| `states` | State abbreviation (`"RJ"`), a vector (`c("SP", "RJ")`) or `"all"` for the whole country | `"all"` |
| `raw_data` | `TRUE` returns the data as published by DATASUS; `FALSE` returns the cleaned version | `FALSE` |
| `language` | `"eng"` (English) or `"pt"` (Portuguese) variable names and labels | `"eng"` |
| `keep_all` | Only in `load_mortality()`: `FALSE` aggregates by municipality, year and cause; `TRUE` keeps one row per death | `FALSE` |

---

## Examples by system

All examples use function and argument names checked against the source code. They download real data from the DATASUS FTP server, so start with one state and one year.

### Mortality data (SIM) in R: `load_mortality()`

```r
library(datazoom.saude)

# General deaths in Rio de Janeiro, 2022, aggregated by municipality, year and cause (default)
deaths_rj <- load_mortality(
  dataset = "general",
  time_period = 2022,
  states = "RJ"
)

# One row per death certificate (no aggregation), Portuguese names
deaths_rj_ind <- load_mortality(
  dataset = "general",
  time_period = 2022,
  states = "RJ",
  keep_all = TRUE,
  language = "pt"
)

# Maternal deaths in Brazil, 2020 to 2022 (national file only)
maternal <- load_mortality(
  dataset = "maternal",
  time_period = 2020:2022
)
```

**What cleaning does for SIM** (`raw_data = FALSE`):
- converts the date of death (`DTOBITO`) to `Date`;
- derives age in years from the coded `IDADE` field;
- standardizes the ICD-10 underlying cause code (`CAUSABAS`);
- renames columns with the package's built-in dictionary and attaches variable labels (`Hmisc::label`).

With `keep_all = FALSE` (the default), the result is an aggregated table with columns `munic_code`, `year`, `cause_of_death` and `num_of_deaths`. In Portuguese the columns are `cod_munic`, `ano`, `causa_de_morte` and `num_de_mortes`.

**Note:** the aggregation uses the **municipality where the death occurred** (`CODMUNOCOR`), not the municipality of residence. For residence-based analysis, use `keep_all = TRUE` and aggregate on the residence variable yourself.

The `"fetal"`, `"external_causes"`, `"infant"` and `"maternal"` sub-systems exist on the FTP server only as national files. If you pass `states = "AC"`, the package warns you and downloads the whole country.

Quick analysis on the aggregated output:

```r
library(dplyr)

deaths_rj %>%
  group_by(cause_of_death) %>%
  summarise(total = sum(num_of_deaths)) %>%
  arrange(desc(total)) %>%
  head(10)
```

### Live births (SINASC) in R: `load_births()`

```r
# Live births in Rio de Janeiro, 2023, cleaned data
births_rj <- load_births(time_period = 2023, states = "RJ")

# São Paulo and Rio, 2020, raw DATASUS data
births_raw <- load_births(time_period = 2020, states = c("RJ", "SP"), raw_data = TRUE)

# Amazonas, 2015, Portuguese variable names
births_am <- load_births(time_period = 2015, states = "AM", language = "pt")
```

SINASC comes from live birth certificates (DNV). It has newborn variables (sex, birth weight, gestational age) and mother variables (age, number of children, schooling, among others).

### Hospital admissions (SIH/SUS) in R: `load_hospital_admissions()`

```r
# Reduced AIH (RD), the most used admissions file: Acre, 2020
admissions_ac <- load_hospital_admissions(
  dataset = "reduced_aih",
  time_period = 2020,
  states = "AC"
)

# Professional services (SP), Acre, 2020
services_ac <- load_hospital_admissions(
  dataset = "professional_services",
  time_period = 2020,
  states = "AC"
)
```

The four `dataset` values:
- `"reduced_aih"` (RD): approved AIHs, with main procedure, diagnoses and amounts paid;
- `"professional_services"` (SP): professional services per admission (CBO, CNS, amounts);
- `"rejected_aih"` (RJ): rejected AIHs;
- `"rejected_aih_error"` (ER): rejected AIHs with the error code.

SIH files are **monthly per state**. `time_period` selects whole years: each year downloads all 12 months for every requested state.

### Outpatient procedures (SIA/SUS) in R: `load_outpatient_procedures()`

```r
# Ambulatory production (PA), Acre, 2022, Portuguese names
production_ac <- load_outpatient_procedures(
  dataset = "ambulatory_production",
  time_period = 2022,
  states = "AC",
  language = "pt"
)

# High-cost medicines (AM), Pernambuco, 2021, raw data
medicines_pe <- load_outpatient_procedures(
  dataset = "medicines",
  time_period = 2021,
  states = "PE",
  raw_data = TRUE
)

# Psychosocial care (RAAS/CAPS), Acre, 2022 and 2023
psychosocial_ac <- load_outpatient_procedures(
  dataset = "psychosocial",
  time_period = 2022:2023,
  states = "AC"
)
```

Accepted `dataset` values: `"ambulatory_production"`, `"bariatric_surgery"`, `"bariatric_surgery_follow_up"`, `"diverse_reports"`, `"medicines"`, `"nephrology"`, `"psychosocial"`, `"fistula_confection"`, `"dialytic_treatment"` and `"home_care"`. In cleaned data, the National Health Card (CNS) variables, which DATASUS already publishes encrypted, are dropped.

Ambulatory production (PA) for a large state over a full year is very large. Try a small state first.

### Hospital beds (CNES-LT) in R: `load_hospital_beds()`

```r
# Beds in Amazonas and Pará, 2010
beds <- load_hospital_beds(time_period = 2010, states = c("AM", "PA"))

# Brazil, 2010 to 2022, Portuguese names (many files: 27 states x 12 months x 13 years)
beds_br <- load_hospital_beds(time_period = 2010:2022, states = "all", language = "pt")
```

CNES-LT is monthly: each row is a bed type at a facility in a given month. With `raw_data = TRUE` the function returns a **list** of data frames (one per file) instead of a single data frame.

### Cancer cases (Oncology Panel) in R: `load_oncology_case()`

```r
# Oncology cases for all of Brazil, 2023
oncology_2023 <- load_oncology_case(time_period = 2023)

# 2021 and 2022, raw data
oncology_raw <- load_oncology_case(time_period = 2021:2022, raw_data = TRUE)
```

DATASUS publishes the Oncology Panel (PO) as one national file per year, so this function has no `states` argument. Filter by state afterwards using the residence, diagnosis or treatment state variables.

### Vaccination (SI-PNI) in R: `load_vaccines()`

```r
# Yellow fever, routine strategy, Acre, 2020 (automatic download)
vaccines_ac <- load_vaccines(
  year = 2020,
  state = "AC",
  strategy = "Rotina",
  product = "Febre amarela - FA",
  language = "eng"
)

# 2024 onwards: download the .xlsx from the DATASUS dashboard and pass its path in `data`
vaccines_mg <- load_vaccines(
  year = 2024,
  state = "MG",
  strategy = "Bloqueio",
  product = "Influenza Trivalente - FLU3V",
  data = "C:/path/to/downloaded_file.xlsx",
  language = "eng"
)
```

How `load_vaccines()` differs from the other functions:
- it takes **one year** (`year`) and **one state** (`state`) per call;
- `strategy` and `product` use the SI-PNI labels, which are in Portuguese. In an interactive session, leaving both as `NULL` opens a menu with the valid combinations;
- the output is **aggregated** (doses applied by municipality, month, strategy, product and dose type), not one row per vaccinated person;
- `dose` is only used when you supply a file downloaded manually from the legacy SI-PNI Web system.

The automatic mode depends on the SI-PNI Web portal, which is often unstable. If it fails, wait a few minutes and retry, or use the manual mode described in `vignette("VACCINES", package = "datazoom.saude")`.

### Reading a DATASUS .dbc file in R: `dbc2dbf_wrapper()`

```r
# Decompress a manually downloaded .dbc file and read the resulting .dbf
input <- "C:/data/DORJ2022.dbc"
output <- tempfile(fileext = ".dbf")
dbc2dbf_wrapper(input.file = input, output.file = output)
data <- foreign::read.dbf(output)
```

`.dbc` is a `.dbf` file compressed with PKWare's *implode* algorithm, used by DATASUS. The function returns `TRUE` on success. The C code comes from Mark Adler's `blast` and Daniela Petruzalek's `read.dbc` (both credited in DESCRIPTION).

---

## Clean or raw data?

- **`raw_data = FALSE` (default):** columns renamed with the package dictionary, variable labels, converted types (dates, codes) and, for SIM, optional aggregation. This is the recommended starting point for analysis.
- **`raw_data = TRUE`:** records as DATASUS publishes them, plus a `file_name` column with the source file and lower-case column names (`janitor::clean_names()`). Use it when you need a variable that cleaning does not handle or want your own cleaning pipeline.

`language` only affects cleaned data.

---

## Performance tips

1. **Start small.** Test with one state and one year before using `states = "all"` and many years.
2. **Know how many files you are asking for.** SIH, SIA and CNES are monthly per state: one year for all of Brazil is 27 × 12 = 324 files. General SIM with `states = "all"` uses a single national file per year.
3. **There is no month or column filter at download time.** If you only need some months or variables, filter afterwards (`dplyr::filter()`, `dplyr::select()`) and save the smaller result.
4. **Save what you download.** `.dbc` files go to `tempdir()` and are deleted after reading. Keep the data frame with `saveRDS(deaths_rj, "deaths_rj_2022.rds")` to avoid downloading it again.
5. **Loop over years for large databases**, saving each year, instead of a single call with `2008:2026` that must fit in memory at once.
6. **Use `raw_data = TRUE` when you do not need cleaning.** It skips renaming and labelling.
7. **Institutional network blocking FTP?** FTP-based functions (`ftp://ftp.datasus.gov.br`) fail on networks that block FTP. Try another network.
8. **No access to GitHub?** In the development version, `options(datazoom.saude.use_remote_manifest = FALSE)` makes the package use only its bundled link manifest.

---

## Known limitations

- It does not cover **SINAN** (notifiable diseases) or all 13 CNES layouts: only CNES-LT (beds).
- The SIA layouts AQ (chemotherapy) and AR (radiotherapy) are in the manifest but are not accepted as `dataset` values in the current version.
- It cannot select months or columns before downloading.
- The default SIM aggregation uses the municipality of occurrence.
- FTP file names are parsed by position (e.g. state in characters 3–4). If DATASUS changes its naming scheme, a function may stop finding files until the package is updated.

---

## FAQ

### Is datazoom.saude free?
Yes. It is open-source software under the MIT license, distributed through CRAN and GitHub.

### Do I need Rtools on Windows?
Only if you install from GitHub with `remotes::install_github()`. CRAN and r-universe provide binaries.

### Why do I get "Package "foreign" must be installed"?
Because `foreign` and `RCurl` are in `Suggests`. Run `install.packages(c("foreign", "RCurl"))`.

### Can I download only some months of SIH or SIA?
Not directly. `time_period` takes years, and the package downloads all 12 months. Filter the months afterwards using the reference-month variable.

### How do I get Portuguese variable names?
Use `language = "pt"` with `raw_data = FALSE`.

### Why is the state filter ignored for infant, maternal, fetal or external-cause deaths?
Because DATASUS publishes those files only at the national level. The package warns you and downloads all of Brazil. Filter by state afterwards using the municipality variables.

### How do I get vaccination data from 2024 onwards?
Download the spreadsheet from the DATASUS vaccination dashboard and pass its path in `data`. Step-by-step instructions are in the `VACCINES` vignette.

### Are these official data?
Yes. The package downloads the files directly from the public DATASUS FTP server (Brazilian Ministry of Health). The only transformations are those described under "Clean or raw data?".

### Where do I report a bug?
At [github.com/datazoompuc/datazoom.saude/issues](https://github.com/datazoompuc/datazoom.saude/issues).

---

## How to cite datazoom.saude

Cite the package with the version you used:

> Regadas, L. T., Laport Santos, L., dos Santos P. Barbosa, J. P., & Souza, V. A. B. A. (2026). *datazoom.saude: Download Brazilian Health Data from DATASUS* (R package version 0.1.1). Data Zoom, PUC-Rio. https://doi.org/10.32614/CRAN.package.datazoom.saude

```bibtex
@Manual{datazoom_saude,
  title  = {datazoom.saude: Download Brazilian Health Data from DATASUS},
  author = {Laura {Tavares Regadas} and Luana {Laport Santos} and Jo{\~a}o Paulo {dos Santos P. Barbosa} and Vanndher Am{\'o}s {B. A. de Souza}},
  year   = {2026},
  note   = {R package version 0.1.1},
  doi    = {10.32614/CRAN.package.datazoom.saude},
  url    = {https://CRAN.R-project.org/package=datazoom.saude},
}
```

In R, `citation("datazoom.saude")` builds a reference from DESCRIPTION. The package does not ship its own `CITATION` file yet. Also cite the data source (DATASUS, Brazilian Ministry of Health) and, optionally, the Data Zoom project (see [How to cite Data Zoom](/en/citacao/)).

---

## Sources

- Source code and vignettes: [github.com/datazoompuc/datazoom.saude](https://github.com/datazoompuc/datazoom.saude) (DESCRIPTION, NAMESPACE, `R/*.R`, `vignettes/*.Rmd`, `inst/extdata/manifest/v1/datasets_link.csv`, `NEWS.md`).
- CRAN package page: [cran.r-project.org/package=datazoom.saude](https://cran.r-project.org/package=datazoom.saude).
- Data Zoom r-universe: [datazoompuc.r-universe.dev](https://datazoompuc.r-universe.dev).
- DATASUS: [datasus.saude.gov.br](https://datasus.saude.gov.br/).
- Rtools: [cran.r-project.org/bin/windows/Rtools](https://cran.r-project.org/bin/windows/Rtools/).
