---
layout: default
title: Data Zoom Saúde
lang: pt
seo_title: "datazoom.saude: baixar microdados do DATASUS no R"
description: "Pacote R que baixa e trata microdados do DATASUS: SIM, SINASC, SIH, SIA, CNES, oncologia e SI-PNI. Veja instalação, funções e exemplos de código."
locale: "pt_BR"
ref: "dz_saude"
permalink: "/pt/dz_saude/"
image: "/assets/img/hex_dzsaude.png"
---

# Data Zoom Saúde

<img src="{{ site.baseurl }}/assets/img/hex_dzsaude.png" alt="Data Zoom Saúde" class="logo-item">

O [pacote `datazoom.saude`](https://github.com/datazoompuc/datazoom.saude), disponível em **R**, facilita o acesso às principais bases públicas de saúde do DATASUS, Ministério da Saúde. 
O pacote automatiza o download, a limpeza e a padronização dos dados, permitindo análises consistentes e reprodutíveis do sistema de saúde brasileiro.

<div class="logo-container-small" style="position: relative;">
    <h2> Acesse o pacote no GitHub </h2>
    <img src="{{ site.baseurl }}/assets/img/github-mark.png" alt="GitHub">
    <a href="https://github.com/datazoompuc/datazoom.saude" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 1;" target="_blank"></a>
</div>

<!-- Links to the in-depth pages. -->
<h2 id="guide">Guia completo e ecossistema</h2>
<ul>
  <li><a href="{{ site.baseurl }}/pt/dz_saude/guia/">Guia: como baixar microdados do DATASUS no R com o datazoom.saude</a></li>
  <li><a href="{{ site.baseurl }}/pt/ecossistema/">Ecossistema Data Zoom: todos os pacotes em R e Stata</a></li>
</ul>

<!-- Visible FAQ: must stay identical to _includes/jsonld/faq-dz-saude-pt.jsonld (FAQPage). -->
<section id="faq">
  <h2>Perguntas frequentes</h2>
  <h3>O que é o datazoom.saude?</h3>
  <p>O datazoom.saude é um pacote R gratuito (licença MIT), publicado no CRAN e mantido pelo Data Zoom, do Departamento de Economia da PUC-Rio. Ele baixa, limpa e padroniza microdados públicos de saúde do DATASUS (Ministério da Saúde): mortalidade (SIM), nascidos vivos (SINASC), internações hospitalares (SIH/SUS), leitos (CNES-LT), procedimentos ambulatoriais (SIA/SUS), casos de oncologia (Painel de Oncologia) e vacinação (SI-PNI).</p>
  <h3>Como baixar microdados do DATASUS no R?</h3>
  <p>Instale o pacote com install.packages("datazoom.saude") e chame a função do sistema desejado. Cada função recebe o período (time_period), os estados (states, com "all" para o Brasil inteiro), raw_data (TRUE para os dados brutos, FALSE para os dados tratados) e language ("eng" ou "pt"). Exemplo: library(datazoom.saude); load_births(time_period = 2023, states = "RJ").</p>
  <h3>Como baixar dados de mortalidade do SIM no R?</h3>
  <p>Use load_mortality(). O argumento dataset escolhe a base: "general" (óbitos gerais), "fetal", "external_causes" (causas externas), "infant" (óbitos infantis) ou "maternal" (óbitos maternos). Exemplo: load_mortality(dataset = "general", time_period = 2022, states = "RJ"). Com keep_all = FALSE (padrão) os dados tratados são agregados por município; com keep_all = TRUE todas as variáveis individuais são mantidas.</p>
  <h3>Como baixar internações hospitalares do SIH/SUS no R?</h3>
  <p>Use load_hospital_admissions() com dataset = "reduced_aih" (AIH reduzida, RD), "professional_services" (serviços profissionais, SP), "rejected_aih" (AIH rejeitada, RJ) ou "rejected_aih_error" (AIH rejeitada com código de erro, ER). Exemplo: load_hospital_admissions(dataset = "professional_services", time_period = 2020, states = "AC").</p>
  <h3>Como baixar dados de nascidos vivos do SINASC no R?</h3>
  <p>Use load_births(). Exemplo: load_births(time_period = 2023, states = "RJ", raw_data = FALSE). Para vários anos, passe um vetor, como time_period = 2015:2020.</p>
  <h3>Como obter dados de leitos hospitalares do CNES no R?</h3>
  <p>Use load_hospital_beds(), que lê o subsistema de leitos do CNES (CNES-LT) com dados mensais por estabelecimento e tipo de leito. Exemplo: load_hospital_beds(time_period = "2020", states = "RO", language = "eng").</p>
  <h3>Como baixar procedimentos ambulatoriais do SIA/SUS no R?</h3>
  <p>Use load_outpatient_procedures(). O argumento dataset aceita, entre outros, "ambulatory_production" (produção ambulatorial), "bariatric_surgery", "diverse_reports", "medicines", "nephrology", "psychosocial", "dialytic_treatment" e "home_care". Exemplo: load_outpatient_procedures(dataset = "diverse_reports", time_period = 2016, states = "PI", language = "pt").</p>
  <h3>O pacote tem dados de vacinação e de câncer?</h3>
  <p>Sim. load_vaccines() traz doses aplicadas do SI-PNI por município, mês, estratégia, vacina e dose; para 1994–2023 a função consulta o portal legado do SI-PNI, e para anos mais recentes você informa, pelo argumento data, o arquivo .xlsx baixado do painel oficial. load_oncology_case() traz os casos do Painel de Oncologia do DATASUS para todo o Brasil, por exemplo load_oncology_case(time_period = 2023).</p>
  <h3>Como abrir arquivos .dbc do DATASUS?</h3>
  <p>As funções load_* já baixam e descompactam os arquivos .dbc automaticamente. Para converter um arquivo .dbc que você já tem, use dbc2dbf_wrapper(input.file, output.file), que gera o .dbf correspondente.</p>
  <h3>Posso ter os nomes das variáveis em português?</h3>
  <p>Sim. Todas as funções aceitam language = "pt" para nomes e rótulos em português; o padrão é "eng" (inglês).</p>
  <h3>Como citar o datazoom.saude?</h3>
  <p>No R, rode citation("datazoom.saude") para obter a referência do pacote. A página https://datazoom.com.br/pt/citacao/ traz a citação do Data Zoom em BibTeX, ABNT, APA e MLA.</p>
</section>
