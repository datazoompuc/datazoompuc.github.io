---
layout: default
title: Data Zoom Saúde
lang: en
seo_title: "datazoom.saude: download DATASUS microdata in R"
description: "R package that downloads and cleans Brazilian DATASUS microdata: SIM, SINASC, SIH, SIA, CNES, oncology and SI-PNI. Installation, functions, examples."
locale: "en_US"
ref: "dz_saude"
permalink: "/en/dz_saude/"
image: "/assets/img/hex_dzsaude.png"
---

# Data Zoom Saúde

<img src="{{ site.baseurl }}/assets/img/hex_dzsaude.png" alt="Data Zoom Saúde" class="logo-item">

The [`datazoom.saude` package](https://github.com/datazoompuc/datazoom.saude), available in R, facilitates access to the main public health databases maintained by DATASUS, 
Ministry of Health.
The package automates data downloading, cleaning, and standardization, enabling consistent and reproducible analyses of the Brazilian health system.

<div class="logo-container-small" style="position: relative;">
    <h2> Access our package on GitHub </h2>
    <img src="{{ site.baseurl }}/assets/img/github-mark.png" alt="GitHub">
    <a href="https://github.com/datazoompuc/datazoom.saude" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 1;" target="_blank"></a>
</div>

<!-- Links to the in-depth pages. -->
<h2 id="guide">Full guide and ecosystem</h2>
<ul>
  <li><a href="{{ site.baseurl }}/en/dz_saude/guide/">Guide: how to download DATASUS microdata in R with datazoom.saude</a></li>
  <li><a href="{{ site.baseurl }}/en/ecosystem/">Data Zoom ecosystem: all R and Stata packages</a></li>
</ul>

<!-- Visible FAQ: must stay identical to _includes/jsonld/faq-dz-saude-en.jsonld (FAQPage). -->
<section id="faq">
  <h2>FAQ</h2>
  <h3>What is datazoom.saude?</h3>
  <p>datazoom.saude is a free R package (MIT license) on CRAN, maintained by Data Zoom at the PUC-Rio Department of Economics. It downloads, cleans and standardizes Brazilian public health microdata from DATASUS (Ministry of Health): mortality (SIM), live births (SINASC), hospital admissions (SIH/SUS), hospital beds (CNES-LT), outpatient procedures (SIA/SUS), oncology cases (Oncology Panel) and vaccination (SI-PNI).</p>
  <h3>How do I download Brazilian health microdata (DATASUS) in R?</h3>
  <p>Install the package with install.packages("datazoom.saude") and call the function for the system you need. Each function takes the period (time_period), the states (states, "all" for the whole country), raw_data (TRUE for raw files, FALSE for cleaned data) and language ("eng" or "pt"). Example: library(datazoom.saude); load_births(time_period = 2023, states = "RJ").</p>
  <h3>How do I download SIM mortality data in R?</h3>
  <p>Use load_mortality(). The dataset argument selects the file: "general", "fetal", "external_causes", "infant" or "maternal". Example: load_mortality(dataset = "general", time_period = 2022, states = "RJ"). With keep_all = FALSE (default) the cleaned data are aggregated by municipality; keep_all = TRUE keeps all individual-level variables.</p>
  <h3>How do I download SIH/SUS hospital admissions in R?</h3>
  <p>Use load_hospital_admissions() with dataset = "reduced_aih" (RD), "professional_services" (SP), "rejected_aih" (RJ) or "rejected_aih_error" (ER). Example: load_hospital_admissions(dataset = "professional_services", time_period = 2020, states = "AC").</p>
  <h3>How do I download SINASC live birth data in R?</h3>
  <p>Use load_births(). Example: load_births(time_period = 2023, states = "RJ", raw_data = FALSE). For several years, pass a vector such as time_period = 2015:2020.</p>
  <h3>How do I get CNES hospital bed data in R?</h3>
  <p>Use load_hospital_beds(), which reads the CNES beds subsystem (CNES-LT) with monthly data by facility and bed type. Example: load_hospital_beds(time_period = "2020", states = "RO", language = "eng").</p>
  <h3>How do I download SIA/SUS outpatient procedures in R?</h3>
  <p>Use load_outpatient_procedures(). The dataset argument accepts, among others, "ambulatory_production", "bariatric_surgery", "diverse_reports", "medicines", "nephrology", "psychosocial", "dialytic_treatment" and "home_care". Example: load_outpatient_procedures(dataset = "diverse_reports", time_period = 2016, states = "PI", language = "pt").</p>
  <h3>Does the package include vaccination and cancer data?</h3>
  <p>Yes. load_vaccines() returns SI-PNI doses applied by municipality, month, strategy, vaccine and dose; for 1994–2023 it queries the legacy SI-PNI portal, and for more recent years you pass the .xlsx file downloaded from the official dashboard through the data argument. load_oncology_case() returns DATASUS Oncology Panel cases for all of Brazil, e.g. load_oncology_case(time_period = 2023).</p>
  <h3>How do I open DATASUS .dbc files?</h3>
  <p>The load_* functions download and decompress .dbc files automatically. To convert a .dbc file you already have, use dbc2dbf_wrapper(input.file, output.file), which writes the corresponding .dbf file.</p>
  <h3>Can I get variable names in Portuguese?</h3>
  <p>Yes. Every function accepts language = "pt" for Portuguese names and labels; the default is "eng" (English).</p>
  <h3>How do I cite datazoom.saude?</h3>
  <p>In R, run citation("datazoom.saude") to get the package reference. The page https://datazoom.com.br/en/citacao/ provides the Data Zoom citation in BibTeX, ABNT, APA and MLA.</p>
</section>
