---
layout: default
title: "Ecossistema Data Zoom"
seo_title: "Ecossistema Data Zoom: pacotes R e Stata de dados do Brasil"
description: "Visão geral dos pacotes datazoom.saude, datazoom.amazonia, datazoom.social e datazoom_social (Stata): bases cobertas, instalação, citação e equipe."
lang: pt
locale: pt_BR
ref: ecosystem
permalink: /pt/ecossistema/
last_modified_at: 2026-11-16
autor: "Equipe Data Zoom (PUC-Rio)"
fontes: "READMEs e NAMESPACEs de datazoompuc/datazoom.saude, datazoom.amazonia, datazoom.social e datazoom_social_Stata; páginas CRAN; API do GitHub; cranlogs; _data/equipe.yml e _data/citacoes.yml do site (16/11/2026)."
---

# Ecossistema Data Zoom: pacotes R e Stata para microdados do Brasil

> **Resposta direta:** o **Data Zoom** é um projeto do Departamento de Economia da PUC-Rio que mantém quatro pacotes gratuitos para pesquisa com dados brasileiros: **datazoom.saude** (R, saúde pública do DATASUS), **datazoom.amazonia** (R, dados ambientais, sociais e econômicos da Amazônia Legal), **datazoom.social** (R, PNAD Contínua em painel) e **datazoom_social** (Stata, leitura e compatibilização de pesquisas domiciliares do IBGE). Os três pacotes em R estão no CRAN e se instalam com `install.packages()`. O pacote Stata se instala com `net install` a partir do GitHub.

O projeto é coordenado pelo professor Gustavo Gonzaga (Ph.D., UC Berkeley) e está ligado ao **Laboratório DataZoom da PUC-Rio**. Os pacotes são desenvolvidos por estudantes de graduação, mestrado e doutorado do Departamento de Economia, que se formam no projeto trabalhando com dados abertos. Todo o código é aberto e fica em [github.com/datazoompuc](https://github.com/datazoompuc).

---

## Os quatro pacotes em uma tabela

| Pacote | Linguagem | Tema | Fontes de dados | Situação (16/11/2026) |
|---|---|---|---|---|
| [datazoom.saude](https://cran.r-project.org/package=datazoom.saude) | R | Saúde pública | DATASUS: SIM, SINASC, SIH, SIA, CNES-LT, Painel de Oncologia, SI-PNI | CRAN 0.1.1 (out/2026) |
| [datazoom.amazonia](https://cran.r-project.org/package=datazoom.amazonia) | R | Amazônia Legal: meio ambiente, economia, sociedade | INPE (PRODES, DETER, DEGRAD), MapBiomas, IBGE, Imazon, SEEG, IBAMA, ANEEL, EPE, COMEX, BACI, TerraClimate e outras | CRAN 1.2.0 (jul/2026); no CRAN desde 2021 |
| [datazoom.social](https://cran.r-project.org/package=datazoom.social) | R | Mercado de trabalho e condições de vida | IBGE: PNAD Contínua | CRAN 0.1.1 (jul/2026) |
| [datazoom_social](https://github.com/datazoompuc/datazoom_social_Stata) | Stata | Pesquisas domiciliares do IBGE | Censo, PNAD, PNAD Contínua, PNAD Covid, PNS, POF, PME, ECINF | GitHub (`net install`) |

Downloads no CRAN (mirror Posit, 17/10 a 15/11/2026): datazoom.amazonia 628, datazoom.saude 507, datazoom.social 413. Acumulado: 21.108, 723 e 2.108, respectivamente.

---

## datazoom.saude: saúde pública (DATASUS) no R

O **datazoom.saude** baixa, limpa e padroniza microdados do DATASUS com uma função por sistema: `load_mortality()` (SIM), `load_births()` (SINASC), `load_hospital_admissions()` (SIH), `load_outpatient_procedures()` (SIA), `load_hospital_beds()` (CNES-LT), `load_oncology_case()` (Painel de Oncologia) e `load_vaccines()` (SI-PNI). Ele também descompacta arquivos `.dbc` com `dbc2dbf_wrapper()`. Os nomes das variáveis podem sair em inglês ou em português.

```r
install.packages(c("datazoom.saude", "foreign", "RCurl"))
library(datazoom.saude)
obitos_rj <- load_mortality(dataset = "general", time_period = 2022, states = "RJ")
```

Saiba mais: [página do pacote](/pt/dz_saude/) · [guia completo](/pt/dz_saude/guia/) · [GitHub](https://github.com/datazoompuc/datazoom.saude)

## datazoom.amazonia: dados da Amazônia Legal no R

O **datazoom.amazonia** é o pacote mais antigo e mais usado do ecossistema. Ele reúne mais de 20 bases tratadas sobre a Amazônia brasileira, em nível municipal, estadual ou regional:
- **Ambientais:** desmatamento (`load_prodes()`, `load_deter()`, `load_degrad()`, `load_imazon()`), multas do IBAMA (`load_ibama()`), uso e cobertura da terra (`load_mapbiomas()`), emissões (`load_seeg()`), clima (`load_climate()`) e Censo Agropecuário (`load_censoagro()`).
- **Sociais:** Índice de Progresso Social (`load_ips()`), acesso à energia (`load_iema()`) e população (`load_population()`).
- **Econômicas:** comércio exterior (`load_br_trade()`, `load_baci()`), PIB municipal (`load_pibmunic()`), empresas (`load_cempre()`), produção agrícola, pecuária e extrativa (`load_pam()`, `load_ppm()`, `load_pevs()`), mineração (`load_sigmine()`) e energia (`load_aneel()`, `load_epe()`).

```r
install.packages("datazoom.amazonia")
library(datazoom.amazonia)
desmatamento <- load_prodes(dataset = "deforestation", raw_data = FALSE,
                            time_period = 2025, language = "pt")
```

O pacote faz parte da iniciativa **Amazônia 2030** e alimenta as [visualizações interativas do Data Zoom Amazônia](https://datazoom.com.br/amazonia/pt/viz/). Documentação completa: [datazoom.com.br/datazoom.amazonia](https://datazoom.com.br/datazoom.amazonia/) · [GitHub](https://github.com/datazoompuc/datazoom.amazonia)

## datazoom.social: PNAD Contínua em painel no R

O **datazoom.social** baixa os microdados da **PNAD Contínua** do IBGE (por meio do pacote `PNADcIBGE`) e identifica os mesmos indivíduos ao longo dos trimestres. O resultado são painéis prontos para análise longitudinal de emprego, renda e informalidade. São duas funções: `load_pnadc()` (download e painel) e `build_pnadc_panel()` (identificação). O argumento `panel` escolhe o algoritmo, de `"basic"` até `"advanced_3"`, o recomendado, que usa correspondência aproximada baseada em grafos para entrevistas fragmentadas.

```r
install.packages("datazoom.social")
library(datazoom.social)
load_pnadc(save_to = "C:/dados/pnadc", years = 2022:2023, panel = "advanced_3")
```

Saiba mais: [página do pacote](/pt/dz_r/) · [GitHub](https://github.com/datazoompuc/datazoom.social)

## datazoom_social: microdados do IBGE no Stata

O **datazoom_social** é o pacote original do Data Zoom, para **Stata**. Ele lê e compatibiliza microdados das principais pesquisas domiciliares do IBGE:

| Pesquisa | Anos |
|---|---|
| Censo Demográfico | 1970 a 2022 |
| PNAD (antiga) | 1981 a 2015 |
| PNAD Contínua | 2012 em diante |
| PNAD Covid | 2020 |
| PNS: Pesquisa Nacional de Saúde | 2013 e 2019 |
| POF: Pesquisa de Orçamentos Familiares | 1995 a 2018 |
| PME: Pesquisa Mensal de Emprego | 1990 a 2015 |
| ECINF: Economia Informal Urbana | 1997 e 2003 |

O pacote **não baixa** os dados. Você baixa os microdados no site do IBGE, e o pacote faz a leitura, cria variáveis compatibilizadas entre anos (por exemplo, nos Censos) e identifica indivíduos na PNAD Contínua. Todas as funções têm caixa de diálogo (`db datazoom_social`), e há tutoriais no [canal do YouTube do Data Zoom](https://www.youtube.com/@datazoom9654/videos).

```stata
net install datazoom_social, from("https://raw.githubusercontent.com/datazoompuc/datazoom_social_stata/main/") force
db datazoom_social
```

Saiba mais: [página do pacote](/pt/dz_stata/) · [GitHub](https://github.com/datazoompuc/datazoom_social_Stata)

---

## Que pacote usar para cada pergunta de pesquisa

| Pergunta de pesquisa | Pacote |
|---|---|
| Mortalidade, nascimentos, internações ou vacinação por município | datazoom.saude |
| Desmatamento, uso da terra ou emissões na Amazônia | datazoom.amazonia |
| Efeito do desmatamento sobre saúde nos municípios da Amazônia | datazoom.amazonia + datazoom.saude |
| Transições no mercado de trabalho (emprego, informalidade) com dados em painel | datazoom.social (R) ou datazoom_social (Stata) |
| Séries longas compatibilizadas do Censo (1970–2022) | datazoom_social (Stata) |
| Gastos das famílias (POF) ou saúde autodeclarada (PNS) | datazoom_social (Stata) |

Os pacotes em R compartilham a mesma convenção de argumentos (`time_period`, `raw_data`, `language`), então o código para uma base serve de modelo para as outras.

---

## Como citar o Data Zoom

Referência institucional do projeto, usada nos READMEs dos pacotes:

> Data Zoom (2023). *Data Zoom: Simplifying Access To Brazilian Microdata*. https://datazoom.com.br/pt/

```bibtex
@Unpublished{DataZoom2023,
  author = {Data Zoom},
  title  = {Data Zoom: Simplifying Access To Brazilian Microdata},
  url    = {https://datazoom.com.br/pt/},
  year   = {2023},
}
```

Para um pacote específico, cite também a versão e o DOI do CRAN:
- datazoom.saude: [10.32614/CRAN.package.datazoom.saude](https://doi.org/10.32614/CRAN.package.datazoom.saude)
- datazoom.amazonia: [10.32614/CRAN.package.datazoom.amazonia](https://doi.org/10.32614/CRAN.package.datazoom.amazonia)
- datazoom.social: [10.32614/CRAN.package.datazoom.social](https://doi.org/10.32614/CRAN.package.datazoom.social)

Dados tratados com o Data Zoom já foram usados em artigos da *American Economic Review* (Engbom e Moser, 2022; Haanwinckel, 2025), da *Quantitative Economics* (Engbom, Gonzaga, Moser e Olivieri, 2022), do *American Economic Journal: Applied Economics* (Van Doornik, Schoenherr e Skrastins, 2023) e da *Demographic Research* (Muniz e Bailey, 2022; Muniz, Queiroz e Saperstein, 2024). A lista completa está em [Como citar](/pt/citacao/).

---

## Quem mantém o Data Zoom

- **Instituição:** Departamento de Economia da Pontifícia Universidade Católica do Rio de Janeiro (PUC-Rio).
- **Coordenação geral:** Gustavo Gonzaga.
- **Manutenção no CRAN:** Laura Tavares Regadas é a mantenedora registrada dos três pacotes R.
- **Equipe:** veja [Equipe](/pt/equipe/).
- **Contato:** datazoom@econ.puc-rio.br. Para erros e sugestões, abra uma issue no GitHub do pacote.

O antigo endereço do projeto, `econ.puc-rio.br/datazoom`, foi substituído por **datazoom.com.br**.

---

## Perguntas frequentes

### Os pacotes do Data Zoom são gratuitos?
Sim. Os três pacotes em R são software livre sob licença MIT. O pacote Stata é distribuído gratuitamente pelo GitHub.

### Preciso saber programar para usar?
No R, basta chamar uma função `load_*()` com o ano e o estado. No Stata, todas as funções têm caixa de diálogo.

### O Data Zoom hospeda os dados?
Não. Os pacotes baixam os dados das fontes oficiais (IBGE, DATASUS, INPE, MapBiomas etc.) e fazem o tratamento no seu computador. A exceção é o pacote Stata, que lê arquivos que você mesmo baixou do IBGE.

### Qual a diferença entre o Data Zoom e a Base dos Dados?
A Base dos Dados é uma plataforma que hospeda dados tratados num data lake público (BigQuery). O Data Zoom é um conjunto de pacotes que baixa os dados direto da fonte oficial e os trata localmente, no R ou no Stata.

### Existe versão em Python?
Não. Os pacotes são para R e Stata.

---

## Fontes

- Repositórios: [datazoom.saude](https://github.com/datazoompuc/datazoom.saude), [datazoom.amazonia](https://github.com/datazoompuc/datazoom.amazonia), [datazoom.social](https://github.com/datazoompuc/datazoom.social), [datazoom_social_Stata](https://github.com/datazoompuc/datazoom_social_Stata) (README e NAMESPACE).
- CRAN: [datazoom.saude](https://cran.r-project.org/package=datazoom.saude), [datazoom.amazonia](https://cran.r-project.org/package=datazoom.amazonia), [datazoom.social](https://cran.r-project.org/package=datazoom.social).
- Documentação pkgdown do datazoom.amazonia: [datazoom.com.br/datazoom.amazonia](https://datazoom.com.br/datazoom.amazonia/).
- Downloads: [cranlogs](https://cranlogs.r-pkg.org/), 17/10–15/11/2026.
- Equipe e trabalhos que citam o Data Zoom: arquivos `_data/equipe.yml` e `_data/citacoes.yml` do site datazoom.com.br.
