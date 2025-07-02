---
layout: default
title: Citação
lang: pt
---

# Cite o Data Zoom em sua pesquisa
    

<font size="4"> <label for="citation">Escolha um formato de citação:</label>
  <select id="citation">
    <option value="bibtex" selected>BibTeX</option>
    <option value="abnt">ABNT (NBR6023)</option>
    <option value="apa">APA</option>
    <option value="mla">MLA</option>
  </select></font>

<div id="output" class="logo-container">
    <pre id="bibtex" style="display: block;">@Unpublished{DataZoom{{YEAR}}, 
    author = {Data Zoom},
    title = {Data Zoom: Simplifying Access To Brazilian Microdata},
    url = {https://www.econ.puc-rio.br/datazoom/english/index.html},
    year = {{YEAR}}
    }</pre>

<pre id="abnt">Data Zoom ({{YEAR}}). Data Zoom: Simplifying Access To Brazilian Microdata. https://www.econ.puc-rio.br/datazoom/index.html</pre>

<pre id="apa">Data Zoom. ({{YEAR}}). Data Zoom: Simplifying access to Brazilian microdata. Retrieved from https://www.econ.puc-rio.br/datazoom/english/index.html</pre>

<pre id="mla">Data Zoom. Data Zoom: Simplifying Access to Brazilian Microdata. {{YEAR}}, https://www.econ.puc-rio.br/datazoom/english/index.html.</pre>
  </div>

Caso você tenha usado um dos nossos produtos, pedimos que nos cite e que envie uma cópia do seu trabalho para <a href="mailto:datazoom@econ.puc-rio.br">datazoom@econ.puc-rio.br</a>. Nós vamos adorar saber mais sobre os estudos que ajudamos a gerar.

## Exemplos de trabalhos que utilizaram o Data Zoom

<div class="scroll-box">
  {% for paper in site.data.citacoes.published %}
    <p>
      {{ paper.author_date }}
      <a href="{{ paper.link }}" target="_blank" rel="noopener noreferrer">
        <em>{{ paper.title }}</em>
      </a>.
    {{ paper.remainder | markdownify }}
  {% endfor %}  
</p>

{% endfor %}
</div>

  <script>
    const currentYear = new Date().getFullYear();

// Substitui todas as ocorrências de {{YEAR}} nos blocos de citação
  const formatIds = ["bibtex", "abnt", "apa", "mla"];
    formatIds.forEach(id => {
      const pre = document.getElementById(id);
      if (pre) {
        pre.innerHTML = pre.innerHTML.replace(/{{YEAR}}/g, currentYear);
      }
    });
      const citationSelect = document.getElementById("citation");
      const formats = ["bibtex", "abnt", "apa", "mla"];

  citationSelect.addEventListener("change", () => {
      const selected = citationSelect.value;
      formats.forEach(id => {
        document.getElementById(id).style.display = "none";
      });
      if (selected) {
        document.getElementById(selected).style.display = "block";
      }
    });
  </script>
          
