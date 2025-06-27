---
layout: default
title: Data Zoom Social Stata
---

# Quais são nossos produtos disponíveis em Stata?

Para facilitar a leitura dos microdados de pesquisas domiciliares do IBGE, oferecemos o pacote *datazoom_social* em Stata. O pacote possui funções específicas para a leitura dos dados da PNS, Censo Demográfico, PNAD Contínua (anual e trimestral), PNAD, PNAD Covid, PME, ECINF e POF.

## Instalação do Pacote:

Para utilizar o pacote, é necessário ter acesso ao Stata 12 ou versões mais recentes e à base de dados de interesse no formato original do IBGE. Para instalar o pacote, digite na janela de comando do Stata:

```
net install datazoom_social, from("https://raw.githubusercontent.com/datazoompuc/datazoom_social_stata/master/") force
```

## Tutoriais:

Para dicas de como usar o *datazoom_social*, acesse nosso [Tutorial](tutoriais/tutorial_datazoom_social_stata.html).

Em nosso [canal](https://www.youtube.com/channel/UC1kYkpfiYSvL2-oTYPMMkug) no YouTube, os usuários podem encontrar também vídeos ilustrando o uso do pacote.
Neste [vídeo](https://www.youtube.com/watch?v=821AcyKRT_k), ensinamos como instalar o pacote. Neste outro [vídeo](https://www.youtube.com/watch?v=EBtGcauiNQQ), ensinamos como utilizar o pacote para a leitura de dados.

## GitHub:

Para uma explicação mais detalhada sobre o que o pacote faz e sobre quais as opções disponíveis em cada uma das funções do pacote, visite nossa página no [GitHub](https://github.com/datazoompuc/datazoom_social_Stata).

## Documentação dos Microdados:

Para acessar a documentação dos microdados suportado pelo pacote *datazoom_social* basta acessar aqui: [Microdados](microdados.html).

**Aviso:**

Para fazer a leitura dos microdados do Censo de 2010, é necessário baixar, além do arquivo contendo os microdados, o arquivo *"microdados_14_municipios_com_areas_redefinidas_20160331"* disponível [neste link](https://ftp.ibge.gov.br/Censos/Censo_Demografico_2010/Resultados_Gerais_da_Amostra_areas_de_ponderacao/Municipios_com_areas_de_ponderacao_redefinidas/microdados_%2014_municipios_com_areas_redefinidas_20160331.zip) e adicioná-lo à mesma pasta onde se localizam os microdados no seu computador.