---
layout: default
title: Citation
lang: en
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
</style>

# Cite Data Zoom in your research

<div id="output" class="scroll-box">

  <label for="citation">Choose citation format:</label>
  <select id="citation">
    <option value="bibtex" selected>BibTeX</option>
    <option value="abnt">ABNT (NBR6023)</option>
    <option value="apa">APA</option>
    <option value="mla">MLA</option>
  </select>
  
<pre id="bibtex" style="display: block;">@Unpublished{DataZoom{2023}, 
    author = {Data Zoom},
    title = {Data Zoom: Simplifying Access To Brazilian Microdata},
    url = {https://www.econ.puc-rio.br/datazoom/english/index.html},
    year = {2023}
}</pre>

<pre id="abnt">Data Zoom (2023). Data Zoom: Simplifying Access To Brazilian Microdata. https://www.econ.puc-rio.br/datazoom/index.html</pre>

<pre id="apa">Data Zoom. (2023). Data Zoom: Simplifying access to Brazilian microdata. Retrieved from https://www.econ.puc-rio.br/datazoom/english/index.html</pre>

<pre id="mla">Data Zoom. Data Zoom: Simplifying Access to Brazilian Microdata. 2023, https://www.econ.puc-rio.br/datazoom/english/index.html.</pre>
  </div>

In case you've used one of our products, please cite us and send us a copy of your work at <a href="mailto:datazoom@econ.puc-rio.br">datazoom@econ.puc-rio.br</a>. We are glad to know about the research our tools help power.

## Example works that used Data Zoom

<div class="scroll-box">
  <h3> Published articles </h3>
  {% for paper in site.data.citacoes.published %}
    <p>
      {{ paper.author_date }}
      <a href="{{ paper.link }}" target="_blank" rel="noopener noreferrer">
        <em>{{ paper.title }}</em>
      </a>.
    {{ paper.remainder }}
    </p>
  {% endfor %}
  <h3> Working papers and other texts </h3>
  {% for paper in site.data.citacoes.unpublished %}
    <p>
      {{ paper.author_date }}
      <a href="{{ paper.link }}" target="_blank" rel="noopener noreferrer">
        <em>{{ paper.title }}</em>
      </a>.
    {{ paper.remainder }}
    </p>
  {% endfor %}
  <h3> Theses and dissertations </h3>
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

// Helper function to select text
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

          
