---
layout: default
title: "Guia do datazoom.saude"
seo_title: "Como baixar microdados do DATASUS no R: guia datazoom.saude"
description: "Passo a passo para baixar SIM, SINASC, SIH, SIA, CNES-LT, Painel de Oncologia e SI-PNI no R com o datazoom.saude, com código, parâmetros e FAQ."
lang: pt
locale: pt_BR
ref: dz_saude_guide
permalink: /pt/dz_saude/guia/
image: /assets/img/hex_dzsaude.png
last_modified_at: 2026-11-16
autor: "Equipe Data Zoom (PUC-Rio)"
fontes: "DESCRIPTION, NAMESPACE, R/*.R, vignettes/ e inst/extdata/manifest/v1/datasets_link.csv do repositório datazoompuc/datazoom.saude (commit 98f127f); página CRAN do pacote (v0.1.1)."
---

# Como baixar microdados do DATASUS no R com o datazoom.saude

> **Resposta direta:** para baixar microdados do DATASUS no R, instale o pacote **datazoom.saude** com `install.packages(c("datazoom.saude", "foreign", "RCurl"))` e chame a função do sistema que você quer: `load_mortality()` (SIM), `load_births()` (SINASC), `load_hospital_admissions()` (SIH), `load_outpatient_procedures()` (SIA), `load_hospital_beds()` (CNES-LT), `load_oncology_case()` (Painel de Oncologia) ou `load_vaccines()` (SI-PNI). Cada função baixa os arquivos `.dbc` do FTP do DATASUS, descompacta, junta os estados e anos pedidos e devolve um `tibble` tratado, com nomes de variáveis em inglês (`language = "eng"`) ou em português (`language = "pt"`).

O **datazoom.saude** é um pacote R gratuito (licença MIT) do **Data Zoom**, projeto do Departamento de Economia da PUC-Rio. A versão 0.1.1 está no CRAN desde 6 de outubro de 2026 (DOI [10.32614/CRAN.package.datazoom.saude](https://doi.org/10.32614/CRAN.package.datazoom.saude)). O código fica em [github.com/datazoompuc/datazoom.saude](https://github.com/datazoompuc/datazoom.saude).

*Última atualização: 16/11/2026. Exemplos conferidos contra o código-fonte do pacote (versão de desenvolvimento 0.1.1.9000 e versão CRAN 0.1.1).*

---

## Resumo: uma função por sistema do DATASUS

| Sistema do DATASUS | O que contém | Função no R |
|---|---|---|
| **SIM**: Sistema de Informações sobre Mortalidade | Declarações de óbito (geral, fetal, infantil, materna, causas externas) | `load_mortality()` |
| **SINASC**: Sistema de Informações sobre Nascidos Vivos | Declarações de nascido vivo: recém-nascido e mãe | `load_births()` |
| **SIH/SUS**: Sistema de Informações Hospitalares | Internações pagas pelo SUS (AIH) | `load_hospital_admissions()` |
| **SIA/SUS**: Sistema de Informações Ambulatoriais | Procedimentos ambulatoriais, APAC, RAAS | `load_outpatient_procedures()` |
| **CNES-LT**: Cadastro Nacional de Estabelecimentos de Saúde (Leitos) | Leitos por estabelecimento, mensal | `load_hospital_beds()` |
| **PO**: Painel de Oncologia | Casos de câncer diagnosticados e tratados | `load_oncology_case()` |
| **SI-PNI**: Programa Nacional de Imunizações | Doses de vacina aplicadas por município | `load_vaccines()` |
| Arquivos `.dbc` locais | Descompactação `.dbc` → `.dbf` | `dbc2dbf_wrapper()` |

---

## Como instalar o datazoom.saude

### Versão estável (CRAN)

```r
install.packages(c("datazoom.saude", "foreign", "RCurl"))
library(datazoom.saude)
```

**Por que instalar `foreign` e `RCurl` junto?** Os dois estão em `Suggests` no DESCRIPTION, então o `install.packages("datazoom.saude")` sozinho não os instala. Mas `load_mortality()`, `load_births()`, `load_hospital_admissions()`, `load_outpatient_procedures()`, `load_hospital_beds()` e `load_oncology_case()` exigem os dois. Sem eles, a função para com a mensagem `Package "foreign" must be installed to use this function.`

### Versão de desenvolvimento (GitHub ou r-universe)

```r
# Pelo r-universe (binários prontos, sem compilar):
install.packages("datazoom.saude",
                 repos = c("https://datazoompuc.r-universe.dev", "https://cloud.r-project.org"))

# Ou direto do GitHub:
install.packages("remotes")
remotes::install_github("datazoompuc/datazoom.saude")
```

O pacote tem código em C (o descompactador de `.dbc`). No **Windows**, instalar pelo GitHub exige o [Rtools](https://cran.r-project.org/bin/windows/Rtools/). Pelo CRAN ou pelo r-universe você recebe um binário e não precisa de compilador.

### O que muda entre a versão CRAN (0.1.1) e a de desenvolvimento

| Ponto | CRAN 0.1.1 | Desenvolvimento (0.1.1.9000) |
|---|---|---|
| Vacinação automática (`load_vaccines()`) | 1994–2022, via navegador headless (`chromote`), exige o Google Chrome instalado | 1994–2023, via requisições HTTP diretas com `curl`, sem Chrome |
| Links de download | Fixos no código | Num manifesto CSV que o pacote atualiza do GitHub em tempo de execução, com fallback para a cópia local |
| Dependências | Inclui `sf`, `XML` e `data.table` em Imports | Remove `sf`, `XML` e `data.table` |
| Faixas de anos | — | Corrigidas para refletir o que existe hoje no FTP do DATASUS (veja a tabela abaixo) |

Fonte: `NEWS.md` do pacote.

---

## Quais bases do DATASUS o datazoom.saude cobre

As faixas de anos abaixo vêm do manifesto `inst/extdata/manifest/v1/datasets_link.csv` da versão de desenvolvimento (novembro de 2026). Elas refletem os arquivos disponíveis no FTP do DATASUS e podem mudar quando o DATASUS publica ou remove arquivos.

| Função | Valor de `dataset` | Arquivo no FTP | Anos disponíveis | Recorte por UF |
|---|---|---|---|---|
| `load_mortality()` | `"general"` | DO (SIM-DO) | 1996–2024 | Sim (ou arquivo nacional com `states = "all"`) |
| | `"fetal"` | DOFET | 1996–2025 | Não: só nacional |
| | `"external_causes"` | DOEXT | 1996–2025 | Não: só nacional |
| | `"infant"` | DOINF | 1996–2025 | Não: só nacional |
| | `"maternal"` | DOMAT | 1996–2025 | Não: só nacional |
| `load_births()` | (não tem) | DN (SINASC) | 1996–2024 | Sim |
| `load_hospital_admissions()` | `"reduced_aih"` | RD | 2008–2026 | Sim |
| | `"professional_services"` | SP | 2008–2026 | Sim |
| | `"rejected_aih"` | RJ | 2008–2026 | Sim |
| | `"rejected_aih_error"` | ER | 2011–2026 | Sim |
| `load_outpatient_procedures()` | `"ambulatory_production"` | PA | 2008–2026 | Sim |
| | `"bariatric_surgery"` | AB | 2008–2017 e 2025 | Sim |
| | `"bariatric_surgery_follow_up"` | ABO | 2014–2026 | Sim |
| | `"diverse_reports"` | AD | 2008–2026 | Sim |
| | `"medicines"` | AM | 2008–2026 | Sim |
| | `"nephrology"` | AN | 2008–2014 | Sim |
| | `"psychosocial"` | PS | 2012–2026 | Sim |
| | `"fistula_confection"` | ACF | 2014–2026 | Sim |
| | `"dialytic_treatment"` | ATD | 2014–2026 | Sim |
| | `"home_care"` | SAD | 2012–2018 | Sim |
| `load_hospital_beds()` | (não tem) | LT (CNES) | 2005–2026 | Sim |
| `load_oncology_case()` | (não tem) | PO | 2013–2026 | Não: só nacional |
| `load_vaccines()` | (não tem) | SI-PNI | 1994–2023 automático; 2024 em diante com arquivo local | Uma UF por chamada |

Se você pedir um ano fora da faixa, o pacote emite um aviso (`Option time_period must be in ...`) antes de tentar o download.

---

## Parâmetros comuns a todas as funções

| Argumento | O que faz | Padrão |
|---|---|---|
| `dataset` | Escolhe o subsistema (só em `load_mortality()`, `load_hospital_admissions()` e `load_outpatient_procedures()`) | obrigatório |
| `time_period` | Ano ou vetor de anos, por exemplo `2020` ou `2015:2020` | obrigatório |
| `states` | Sigla da UF (`"RJ"`), vetor (`c("SP", "RJ")`) ou `"all"` para o Brasil todo | `"all"` |
| `raw_data` | `TRUE` devolve os dados como vêm do DATASUS; `FALSE` devolve a versão tratada | `FALSE` |
| `language` | `"eng"` (inglês) ou `"pt"` (português) para nomes e rótulos das variáveis | `"eng"` |
| `keep_all` | Só em `load_mortality()`: `FALSE` agrega por município, ano e causa; `TRUE` mantém um registro por óbito | `FALSE` |

---

## Exemplos por sistema

Todos os exemplos usam nomes de funções e argumentos conferidos no código-fonte. Eles baixam dados reais do FTP do DATASUS: comece com um estado e um ano.

### Mortalidade (SIM) no R: `load_mortality()`

```r
library(datazoom.saude)

# Óbitos gerais do Rio de Janeiro em 2022, agregados por município, ano e causa (padrão)
obitos_rj <- load_mortality(
  dataset = "general",
  time_period = 2022,
  states = "RJ"
)

# Um registro por declaração de óbito (sem agregar), nomes em português
obitos_rj_ind <- load_mortality(
  dataset = "general",
  time_period = 2022,
  states = "RJ",
  keep_all = TRUE,
  language = "pt"
)

# Óbitos maternos do Brasil, 2020 a 2022 (arquivo só nacional)
obitos_maternos <- load_mortality(
  dataset = "maternal",
  time_period = 2020:2022,
  language = "pt"
)
```

**O que o tratamento faz no SIM** (`raw_data = FALSE`):
- converte a data do óbito (`DTOBITO`) para `Date`;
- cria a idade em anos a partir do campo codificado `IDADE`;
- padroniza o código CID-10 da causa básica (`CAUSABAS`);
- renomeia as colunas com o dicionário interno do pacote e aplica rótulos (`Hmisc::label`).

Com `keep_all = FALSE` (o padrão), o resultado é uma tabela agregada com as colunas `munic_code`, `year`, `cause_of_death` e `num_of_deaths`. Em português, as colunas são `cod_munic`, `ano`, `causa_de_morte` e `num_de_mortes`.

**Atenção:** a agregação usa o **município de ocorrência** do óbito (`CODMUNOCOR`), não o de residência. Para análises por residência, use `keep_all = TRUE` e agregue você mesmo pela variável de residência.

Os subsistemas `"fetal"`, `"external_causes"`, `"infant"` e `"maternal"` existem no FTP só como arquivo nacional. Se você passar `states = "AC"`, o pacote avisa e baixa o Brasil inteiro.

Exemplo de análise rápida com o resultado agregado:

```r
library(dplyr)

obitos_rj %>%
  group_by(cause_of_death) %>%
  summarise(total = sum(num_of_deaths)) %>%
  arrange(desc(total)) %>%
  head(10)
```

### Nascidos vivos (SINASC) no R: `load_births()`

```r
# Nascidos vivos no Rio de Janeiro em 2023, dados tratados
nascidos_rj <- load_births(time_period = 2023, states = "RJ")

# São Paulo e Rio em 2020, dados brutos do DATASUS
nascidos_brutos <- load_births(time_period = 2020, states = c("RJ", "SP"), raw_data = TRUE)

# Amazonas em 2015, nomes de variáveis em português
nascidos_am <- load_births(time_period = 2015, states = "AM", language = "pt")
```

O SINASC vem das Declarações de Nascido Vivo (DNV). Ele traz dados do recém-nascido (sexo, peso, idade gestacional) e da mãe (idade, número de filhos, escolaridade, entre outros).

### Internações hospitalares (SIH/SUS) no R: `load_hospital_admissions()`

```r
# AIH reduzida (RD), a base mais usada para análise de internações: Acre, 2020
internacoes_ac <- load_hospital_admissions(
  dataset = "reduced_aih",
  time_period = 2020,
  states = "AC"
)

# Serviços profissionais (SP) do Acre em 2020
servicos_ac <- load_hospital_admissions(
  dataset = "professional_services",
  time_period = 2020,
  states = "AC"
)
```

Os quatro valores de `dataset`:
- `"reduced_aih"` (RD): AIHs aprovadas, com procedimento principal, diagnósticos e valores;
- `"professional_services"` (SP): serviços profissionais por internação (CBO, CNS, valores);
- `"rejected_aih"` (RJ): AIHs rejeitadas;
- `"rejected_aih_error"` (ER): AIHs rejeitadas, com o código do erro.

Os arquivos do SIH são **mensais por UF**. O `time_period` seleciona anos inteiros: cada ano baixa os 12 meses de cada estado pedido.

### Procedimentos ambulatoriais (SIA/SUS) no R: `load_outpatient_procedures()`

```r
# Produção ambulatorial (PA) do Acre em 2022, em português
producao_ac <- load_outpatient_procedures(
  dataset = "ambulatory_production",
  time_period = 2022,
  states = "AC",
  language = "pt"
)

# Medicamentos de alto custo (AM) de Pernambuco em 2021, dados brutos
medicamentos_pe <- load_outpatient_procedures(
  dataset = "medicines",
  time_period = 2021,
  states = "PE",
  raw_data = TRUE
)

# Atenção psicossocial (RAAS/CAPS) do Acre em 2022 e 2023
psicossocial_ac <- load_outpatient_procedures(
  dataset = "psychosocial",
  time_period = 2022:2023,
  states = "AC"
)
```

Valores aceitos em `dataset`: `"ambulatory_production"`, `"bariatric_surgery"`, `"bariatric_surgery_follow_up"`, `"diverse_reports"`, `"medicines"`, `"nephrology"`, `"psychosocial"`, `"fistula_confection"`, `"dialytic_treatment"` e `"home_care"`. Nos dados tratados, as variáveis do Cartão Nacional de Saúde (CNS), que já vêm criptografadas pelo DATASUS, são removidas.

A produção ambulatorial (PA) de um estado grande num ano inteiro é muito volumosa. Teste primeiro com uma UF pequena.

### Leitos hospitalares (CNES-LT) no R: `load_hospital_beds()`

```r
# Leitos de Amazonas e Pará em 2010
leitos <- load_hospital_beds(time_period = 2010, states = c("AM", "PA"))

# Brasil, 2010 a 2022, em português (muitos arquivos: 27 UFs x 12 meses x 13 anos)
leitos_br <- load_hospital_beds(time_period = 2010:2022, states = "all", language = "pt")
```

O CNES-LT é mensal: cada linha é um tipo de leito num estabelecimento, num mês. Com `raw_data = TRUE`, a função devolve uma **lista** de data frames (um por arquivo), não um único data frame.

### Casos de câncer (Painel de Oncologia) no R: `load_oncology_case()`

```r
# Casos de oncologia do Brasil inteiro em 2023
oncologia_2023 <- load_oncology_case(time_period = 2023)

# 2021 e 2022, dados brutos
oncologia_bruta <- load_oncology_case(time_period = 2021:2022, raw_data = TRUE)
```

O Painel de Oncologia (PO) é publicado num arquivo nacional por ano, então a função não tem o argumento `states`. Filtre a UF depois, pelas variáveis de UF de residência, diagnóstico ou tratamento.

### Vacinação (SI-PNI) no R: `load_vaccines()`

```r
# Febre amarela, estratégia de rotina, Acre, 2020 (download automático)
vacinas_ac <- load_vaccines(
  year = 2020,
  state = "AC",
  strategy = "Rotina",
  product = "Febre amarela - FA",
  language = "eng"
)

# 2024 em diante: baixe o .xlsx no painel do DATASUS e passe o caminho em `data`
vacinas_mg <- load_vaccines(
  year = 2024,
  state = "MG",
  strategy = "Bloqueio",
  product = "Influenza Trivalente - FLU3V",
  data = "C:/caminho/para/arquivo_baixado.xlsx",
  language = "pt"
)
```

Diferenças do `load_vaccines()` em relação às outras funções:
- recebe **um ano** (`year`) e **uma UF** (`state`) por chamada;
- `strategy` e `product` seguem os rótulos do SI-PNI. Numa sessão interativa, se você deixar os dois como `NULL`, aparece um menu com as combinações válidas;
- o resultado é **agregado** (doses aplicadas por município, mês, estratégia, produto e tipo de dose), não um registro por pessoa vacinada;
- `dose` só é usado quando você fornece um arquivo baixado manualmente do sistema antigo (SI-PNI Web).

O download automático depende do portal SI-PNI Web, que costuma ficar instável. Se falhar, espere alguns minutos e tente de novo, ou use o modo manual descrito na vinheta `vignette("VACCINES", package = "datazoom.saude")`.

### Ler um arquivo .dbc do DATASUS no R: `dbc2dbf_wrapper()`

```r
# Descompacta um .dbc baixado manualmente e lê o .dbf resultante
entrada <- "C:/dados/DORJ2022.dbc"
saida <- tempfile(fileext = ".dbf")
dbc2dbf_wrapper(input.file = entrada, output.file = saida)
dados <- foreign::read.dbf(saida)
```

O `.dbc` é um `.dbf` comprimido com o algoritmo *implode* da PKWare, usado pelo DATASUS. A função devolve `TRUE` se a descompactação funcionar. O código em C vem do `blast`, de Mark Adler, e do `read.dbc`, de Daniela Petruzalek (ambos creditados no DESCRIPTION).

---

## Dados tratados ou brutos?

- **`raw_data = FALSE` (padrão):** colunas renomeadas pelo dicionário do pacote, rótulos de variáveis, tipos convertidos (datas, códigos) e, no SIM, agregação opcional. É o modo indicado para começar uma análise.
- **`raw_data = TRUE`:** os registros como o DATASUS publica, com uma coluna `file_name` indicando o arquivo de origem e os nomes das colunas em minúsculas (`janitor::clean_names()`). Use quando precisar de uma variável que o tratamento não cobre ou quiser aplicar sua própria limpeza.

O `language` só tem efeito nos dados tratados.

---

## Dicas de performance

1. **Comece pequeno.** Teste com uma UF e um ano antes de pedir `states = "all"` e vários anos.
2. **Saiba quantos arquivos você vai baixar.** SIH, SIA e CNES são mensais por UF: um ano do Brasil inteiro são 27 × 12 = 324 arquivos. SIM geral com `states = "all"` usa um único arquivo nacional por ano.
3. **Não há filtro de mês nem de colunas no download.** Se você só precisa de alguns meses ou variáveis, filtre depois (`dplyr::filter()`, `dplyr::select()`) e salve o resultado reduzido.
4. **Salve o que baixou.** Os `.dbc` vão para `tempdir()` e são apagados depois da leitura. Guarde o data frame com `saveRDS(obitos_rj, "obitos_rj_2022.rds")` para não baixar de novo.
5. **Faça laços por ano para bases grandes**, salvando cada ano, em vez de uma chamada com `2008:2026` que precisa caber inteira na memória.
6. **Use `raw_data = TRUE` quando o tratamento não for necessário.** Ele pula a renomeação e a rotulagem.
7. **Rede institucional bloqueando FTP?** As funções baseadas no FTP (`ftp://ftp.datasus.gov.br`) falham em redes que bloqueiam FTP. Teste em outra rede.
8. **Sem internet para o GitHub?** Na versão de desenvolvimento, `options(datazoom.saude.use_remote_manifest = FALSE)` faz o pacote usar só o manifesto local de links.

---

## Limitações conhecidas

- Não cobre o **SINAN** (doenças de notificação) nem todos os 13 layouts do CNES: só o CNES-LT (leitos).
- Os layouts SIA AQ (quimioterapia) e AR (radioterapia) estão no manifesto, mas não são aceitos em `dataset` na versão atual.
- Não seleciona meses nem colunas antes do download.
- A agregação padrão do SIM usa o município de ocorrência.
- Os nomes dos arquivos do FTP são interpretados por posição (ex.: UF nos caracteres 3–4). Se o DATASUS mudar o padrão de nomes, a função pode deixar de encontrar arquivos até uma atualização do pacote.

---

## Perguntas frequentes

### O datazoom.saude é gratuito?
Sim. É software livre sob licença MIT, distribuído pelo CRAN e pelo GitHub.

### Preciso do Rtools no Windows?
Só se instalar pelo GitHub com `remotes::install_github()`. A instalação pelo CRAN ou pelo r-universe usa binários.

### Por que aparece o erro "Package "foreign" must be installed"?
Porque `foreign` e `RCurl` estão em `Suggests`. Rode `install.packages(c("foreign", "RCurl"))`.

### Posso baixar só alguns meses do SIH ou do SIA?
Não diretamente. O `time_period` aceita anos, e o pacote baixa os 12 meses. Filtre os meses depois pela variável de competência.

### Como obter os nomes das variáveis em português?
Use `language = "pt"` com `raw_data = FALSE`.

### Por que o filtro de estado é ignorado em óbitos infantis, maternos, fetais ou por causas externas?
Porque o DATASUS publica esses arquivos só em versão nacional. O pacote avisa e baixa o Brasil inteiro. Filtre a UF depois pelas variáveis de município.

### Como baixar vacinação de 2024 em diante?
Baixe a planilha no painel de vacinação do DATASUS e passe o caminho em `data`. O passo a passo está na vinheta `VACCINES`.

### Os dados são oficiais?
Sim. O pacote baixa os arquivos diretamente do FTP público do DATASUS (Ministério da Saúde). As únicas transformações são as descritas em "Dados tratados ou brutos?".

### Onde reporto um erro?
Em [github.com/datazoompuc/datazoom.saude/issues](https://github.com/datazoompuc/datazoom.saude/issues).

---

## Como citar o datazoom.saude

Cite o pacote com a versão usada:

> Regadas, L. T.; Laport Santos, L.; dos Santos P. Barbosa, J. P.; Souza, V. A. B. A. (2026). *datazoom.saude: Download Brazilian Health Data from DATASUS*. Pacote R versão 0.1.1. Data Zoom, PUC-Rio. https://doi.org/10.32614/CRAN.package.datazoom.saude

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

No R, `citation("datazoom.saude")` gera uma referência a partir do DESCRIPTION. O pacote ainda não tem arquivo `CITATION` próprio. Cite também a fonte dos dados (DATASUS, Ministério da Saúde) e, se quiser, o projeto Data Zoom (veja [Como citar o Data Zoom](/pt/citacao/)).

---

## Fontes

- Código-fonte e vinhetas: [github.com/datazoompuc/datazoom.saude](https://github.com/datazoompuc/datazoom.saude) (DESCRIPTION, NAMESPACE, `R/*.R`, `vignettes/*.Rmd`, `inst/extdata/manifest/v1/datasets_link.csv`, `NEWS.md`).
- Página do pacote no CRAN: [cran.r-project.org/package=datazoom.saude](https://cran.r-project.org/package=datazoom.saude).
- r-universe do Data Zoom: [datazoompuc.r-universe.dev](https://datazoompuc.r-universe.dev).
- DATASUS: [datasus.saude.gov.br](https://datasus.saude.gov.br/).
- Rtools: [cran.r-project.org/bin/windows/Rtools](https://cran.r-project.org/bin/windows/Rtools/).
