---
layout: default
title: Citação
lang: pt
---

<style>
#output pre { display: none; }
#output pre#bibtex { display: block; }
  
pre {
  border: 2px solid #369; /* Blue border, change color as you like */
  border-radius: 6px;         /* Rounded corners */
  padding: 1em;               /* Space inside the border */
  background-color: #ecf1f5; /* Light blue background */
  font-family: monospace;
  font-size: 0.9em;
  white-space: pre-wrap;  /* wraps long lines */
  word-wrap: break-word;  /* breaks long words if needed */
  cursor: pointer; /* Shows it's clickable */
  user-select: all;
}

.scroll-box {
  width: 100% !important;
  max-width: 100% !important;
  box-sizing: border-box;
}
  
.scroll-box h3 {
  font-size: 1.4em;
  margin-top: 2em;
  margin-bottom: 1em;
}

.scroll-box h3:first-of-type {
  margin-top: 0.5em;
}

.scroll-box p {
  margin-bottom: 1.5em;
}
  
</style>

# Cite o Data Zoom em sua pesquisa

<div id="output" class="scroll-box">

  <label for="citation">Escolha um formato de citação:</label>
  <select id="citation">
    <option value="bibtex" selected>BibTeX</option>
    <option value="abnt">ABNT (NBR6023)</option>
    <option value="apa">APA</option>
    <option value="mla">MLA</option>
  </select>
  
<pre id="bibtex" style="display: block;">@Unpublished{DataZoom{2023}, 
    author = {Data Zoom},
    title = {Data Zoom: Simplifying Access To Brazilian Microdata},
    url = {https://datazoom.com.br/en/},
    year = {2023}
}</pre>

<pre id="abnt">Data Zoom (2023). Data Zoom: Simplifying Access To Brazilian Microdata. https://datazoom.com.br/en/</pre>

<pre id="apa">Data Zoom. (2023). Data Zoom: Simplifying access to Brazilian microdata. Retrieved from https://datazoom.com.br/en/</pre>

<pre id="mla">Data Zoom. Data Zoom: Simplifying Access to Brazilian Microdata. 2023, https://datazoom.com.br/en/.</pre>
  </div>

Caso o Data Zoom tenha ajudado em algum estudo escrito por você, pedimos que envie uma cópia para [datazoom@econ.puc-rio.br](mailto:datazoom@econ.puc-rio.br). Nós vamos adorar saber mais sobre os estudos que ajudamos a gerar. Artigos enviados serão acrescentados ao quadro abaixo.

## Exemplos de trabalhos que utilizaram o Data Zoom

<div class="scroll-box">
  <h3> Artigos publicados </h3>
  {% for paper in site.data.citacoes.published %}
    <p>
      {{ paper.author_date }}
      <a href="{{ paper.link }}" target="_blank" rel="noopener noreferrer">
        <em>{{ paper.title }}</em>
      </a>.
    {{ paper.remainder | markdownify | remove: "<p>" | remove: "</p>" }}
    </p>
  {% endfor %}
  <h3> Working papers e outros textos </h3>
  {% for paper in site.data.citacoes.unpublished %}
    <p>
      {{ paper.author_date }}
      <a href="{{ paper.link }}" target="_blank" rel="noopener noreferrer">
        <em>{{ paper.title }}</em>
      </a>.
    {{ paper.remainder }}
    </p>
  {% endfor %}
  <h3> Teses e dissertações </h3>
  {% for paper in site.data.citacoes.theses %}
    <p>
      {{ paper.author_date }}
      <a href="{{ paper.link }}" target="_blank" rel="noopener noreferrer">
        <em>{{ paper.title }}</em>
      </a>.
    {{ paper.remainder }}
    </p>
  {% endfor %}  
</div>

<script>
document.addEventListener("DOMContentLoaded", function () {
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
});

// Click to select all text
document.addEventListener('click', function(e) {
    if (e.target.tagName === 'PRE') {
        selectText(e.target);
    }
}); // <- This closing brace was missing

// Helper function to select text (moved outside the event listener)
function selectText(element) {
    if (window.getSelection) {
        const range = document.createRange();
        range.selectNodeContents(element);
        const selection = window.getSelection();
        selection.removeAllRanges();
        selection.addRange(range);
    }
}
</script>

          
