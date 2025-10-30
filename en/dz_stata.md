---
layout: default
title: Data Zoom Social Stata
lang: en
---

# Data Zoom Social Stata

<img src="{{ site.baseurl }}/assets/img/hex_dzstata.png" alt="Data Zoom Social Stata" class="logo-item">

The [`datazoom_social_Stata` Stata package](https://github.com/datazoompuc/datazoom_social_Stata) eases the access to microdata from IBGE's household surveys. The package has specific funtions to read data from PNS, Censo Demográfico, PNAD Contínua (annual and quarterly), PNAD, PNAD Covid, PME, ECINF e POF.

<div class="logo-container-small" style="position: relative;">
    <h2> Access our package on GitHub </h2>
    <img src="{{ site.baseurl }}/assets/img/github-mark.png" alt="GitHub">
    <a href="https://github.com/datazoompuc/datazoom_social_Stata/blob/main/README_en.md" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%; z-index: 1;" target="_blank"></a>
</div>


---

## Support Files {#dz-support}

This section provides support files that can assist in research using the surveys microdata. They include variable dictionaries, descriptions, layouts, and harmonization documents that help users interpret and analyze the data more effectively.

<div class="dz-accordions" id="dz-accordions" data-lang="{{ page.lang | default: 'pt' }}">

  <div class="dz-box" data-folder="Censo" data-title="Census">
    <button class="dz-box__header" type="button" aria-expanded="false">
      <span class="dz-box__title">Census</span>
      <span class="dz-box__chevron">+</span>
    </button>
    <div class="dz-box__panel" hidden>
      <div class="dz-box__status" aria-live="polite"></div>
      <div class="dz-file-list"></div>
    </div>
  </div>

  <div class="dz-box" data-folder="ECINF" data-title="Urban Informal Economy Survey (ECINF)">
    <button class="dz-box__header" type="button" aria-expanded="false">
      <span class="dz-box__title">Urban Informal Economy Survey (ECINF)</span>
      <span class="dz-box__chevron">+</span>
    </button>
    <div class="dz-box__panel" hidden>
      <div class="dz-box__status" aria-live="polite"></div>
      <div class="dz-file-list"></div>
    </div>
  </div>

  <div class="dz-box" data-folder="PME" data-title="Brazilian Monthly Employment Survey (PME)">
    <button class="dz-box__header" type="button" aria-expanded="false">
      <span class="dz-box__title">Brazilian Monthly Employment Survey (PME)</span>
      <span class="dz-box__chevron">+</span>
    </button>
    <div class="dz-box__panel" hidden>
      <div class="dz-box__status" aria-live="polite"></div>
      <div class="dz-file-list"></div>
    </div>
  </div>

  <div class="dz-box" data-folder="PNAD" data-title="Brazilian National Household Sample Survey (PNAD)">
    <button class="dz-box__header" type="button" aria-expanded="false">
      <span class="dz-box__title">Brazilian National Household Sample Survey (PNAD)</span>
      <span class="dz-box__chevron">+</span>
    </button>
    <div class="dz-box__panel" hidden>
      <div class="dz-box__status" aria-live="polite"></div>
      <div class="dz-file-list"></div>
    </div>
  </div>

  <div class="dz-box" data-folder="POF" data-title="Brazilian Consumer Expenditure Survey (POF)">
    <button class="dz-box__header" type="button" aria-expanded="false">
      <span class="dz-box__title">Brazilian Consumer Expenditure Survey (POF)</span>
      <span class="dz-box__chevron">+</span>
    </button>
    <div class="dz-box__panel" hidden>
      <div class="dz-box__status" aria-live="polite"></div>
      <div class="dz-file-list"></div>
    </div>
  </div>
  
  <div class="dz-box" data-folder="PNAD Contínua" data-title="Continuous National Household Sample Survey (PNAD-Contínua)">
    <button class="dz-box__header" type="button" aria-expanded="false">
      <span class="dz-box__title">Continuous National Household Sample Survey (PNAD-Contínua)</span>
      <span class="dz-box__chevron">+</span>
    </button>
    <div class="dz-box__panel" hidden>
      <div class="dz-box__status" aria-live="polite"></div>
      <div class="dz-file-list"></div>
    </div>
  </div>
  
  <div class="dz-box" data-folder="PNS" data-title="National Health Survey (PNS)">
    <button class="dz-box__header" type="button" aria-expanded="false">
      <span class="dz-box__title">National Health Survey (PNS)</span>
      <span class="dz-box__chevron">+</span>
    </button>
    <div class="dz-box__panel" hidden>
      <div class="dz-box__status" aria-live="polite"></div>
      <div class="dz-file-list"></div>
    </div>
  </div>

  <div class="dz-box" data-folder="PNAD Covid" data-title="PNAD Covid">
    <button class="dz-box__header" type="button" aria-expanded="false">
      <span class="dz-box__title">PNAD Covid</span>
      <span class="dz-box__chevron">+</span>
    </button>
    <div class="dz-box__panel" hidden>
      <div class="dz-box__status" aria-live="polite"></div>
      <div class="dz-file-list"></div>
    </div>
  </div>

  
</div>



<script>
(function () {
    /* ==========================
       CONFIGURAÇÃO BÁSICA
       ==========================
       ORG/REPO/BRANCH definem onde estão os arquivos no GitHub.
       O script consulta a árvore do repositório uma única vez,
       guarda em cache no navegador (localStorage) e reutiliza nos cliques.
    */
  const ORG = "datazoompuc";
  const REPO = "datazoom_social_Stata";
  const BRANCH = "main";
  
  const container = document.getElementById("dz-accordions");
  const LANG = (container?.dataset?.lang || "{{ page.lang | default: 'pt' }}").toLowerCase(); // 'pt' ou 'en'

  /* Chave do cache no localStorage. O sufixo :v4 força “versão” do cache */
  const CACHE_KEY = `dz_tree:${ORG}/${REPO}@${BRANCH}:${LANG}:v4`;

  /* Quanto tempo o cache fica válido (24h). */
  const CACHE_TTL_MS = 24 * 60 * 60 * 1000;

  /* URLs usadas:
     - branchUrl: pega o SHA do último commit da branch (para ler a árvore)
     - rawUrl: link direto para baixar/abrir o arquivo
     - ghFolderUrl: fallback para abrir a pasta no site do GitHub
  */
  const branchUrl = `https://api.github.com/repos/${ORG}/${REPO}/branches/${encodeURIComponent(BRANCH)}`;
  const rawUrl = (path) => `https://raw.githubusercontent.com/${ORG}/${REPO}/${encodeURIComponent(BRANCH)}/${path}`;
  const ghFolderUrl = (folder) =>
  `https://github.com/${ORG}/${REPO}/tree/${encodeURIComponent(BRANCH)}/docs/${encodeURIComponent(LANG)}/${encodeURIComponent(folder)}`;
  
  /* ==========================
     UTILITÁRIOS DE TEXTO
     ==========================
     Funções pequenas para limpar e formatar nomes de arquivo
     e para filtrar arquivos em inglês.
  */

  // Remove somente a última extensão do arquivo (ex.: .pdf, .xlsx, .docx)
  function stripExt(name) {
    return name.replace(/\.[^.]+$/i, "");
  }



  /* Tabelas de normalização:
     - translateMap: traduz palavras
     - acronyms: mantém siglas importantes em maiúsculas
  */
  const translateMap = new Map([
    ["compatibilizacao","Harmonization"],["dicionario","Dictionary"],["continua","Contínua"],["antiga","Old"],["nova","New"],
    ["compatibilizado","Harmonized"],["tradutor","Translator"],["descricao","Description"],["alimentos","Alimentation"],
    ["despesas","Expenses"],["renda","Income"],["variaveis","Variables"]
  ]);

  const acronyms = new Map([
    ["ibge","IBGE"],["pnad","PNAD"],["pof","POF"],["pme","PME"],["ecinf","ECINF"],["pns","PNS"]
  ]);

  /* Converte o nome “cru” de arquivo em um nome “bonito” para exibição:
     - remove extensão e sufixo EN;
     - substitui _ e - por espaço;
     - aplica Title Case (com exceções);
     - corrige acentos de palavras conhecidas;
     - mantém siglas em maiúsculas;
     - junta anos "1992 1995" → "1992-1995";
     - troca "Dom" → "(Households)" e "Pess" → "(Individuals)";
     - mantém certas preposições minúsculas (de, da, do, dos, das, com) quando não são a 1ª palavra.
  */
  function prettyName(filename) {
    let base = stripExt(filename).trim();
    base = base.replace(/[“”"']+$/g, "").trim();
    base = base.replace(/(?:[\s_\-\(\)]+)en$/i, ""); // remove sufixo EN
    base = base.replace(/[_-]+/g, " ").replace(/\s{2,}/g, " ").trim();

    const smallWords = new Set(["de","da","do","dos","das","com"]);

    // Aplica Title Case com regras de siglas, acentos e preposições
    const words = base.split(/\s+/).map((w, i) => {
      const lower = w.toLowerCase();

      if (acronyms.has(lower))     return acronyms.get(lower); // siglas (PNAD, POF, etc.)
      if (translateMap.has(lower))   return translateMap.get(lower); // palavras traduzidas
      if (/^[A-Z0-9]{2,}$/.test(w)) return w; // mantém siglas/números já em maiúsculas

      // Preposições minúsculas quando não são a primeira palavra
      if (smallWords.has(lower) && i > 0) return lower;

      // Title Case simples
      return lower.charAt(0).toUpperCase() + lower.slice(1);
    });

    // Junta as palavras e aplica ajustes finais
    let title = words.join(" ");

    // Dois anos seguidos viram um intervalo: "1992 1995" → "1992-1995"
    title = title.replace(/\b((?:19|20)\d{2})\s+((?:19|20)\d{2})\b/g, "$1-$2");

    // Abreviações por extenso
    title = title
      .replace(/\bDom\b/gi, "(Households)")
      .replace(/\bPess\b/gi, "(Individuals)");

    return title;
  }

  /* ==========================
     CACHE NO NAVEGADOR
     ==========================
     Guardamos a árvore do repositório no localStorage por 24h.
     Assim evitamos bater no limite de chamadas da API do GitHub.
  */
  function getCachedTree() {
    try {
      const raw = localStorage.getItem(CACHE_KEY);
      if (!raw) return null;
      const data = JSON.parse(raw);
      if (!data || !data.tree || !data.ts) return null;
      if (Date.now() - data.ts > CACHE_TTL_MS) return null; // cache expirado
      return data.tree;
    } catch {
      return null;
    }
  }

  function setCachedTree(tree) {
    try {
      localStorage.setItem(CACHE_KEY, JSON.stringify({ ts: Date.now(), tree }));
    } catch {
      // se o navegador bloquear (quota), apenas ignoramos o cache
    }
  }

  // Mantém a árvore carregada em memória após a primeira leitura
  let treeCache = null;

  /* Carrega a árvore do repositório UMA VEZ:
     - tenta ler do cache do navegador;
     - se não houver cache, chama a API do GitHub:
       1) pega o SHA da branch;
       2) baixa a árvore completa (recursiva).
     Retorna { ok: true, tree } em caso de sucesso; senão { ok: false, errStatus }.
  */
  async function loadRepoTreeOnce() {
    if (treeCache) return { ok: true, tree: treeCache };

    const cached = getCachedTree();
    if (cached) {
      treeCache = cached;
      return { ok: true, tree: treeCache };
    }

    try {
      const b = await fetch(branchUrl, { headers: { "Accept": "application/vnd.github+json" } });
      if (!b.ok) return { ok: false, errStatus: b.status };

      const branchInfo = await b.json();
      const sha = branchInfo?.commit?.sha;
      if (!sha) return { ok: false, errStatus: 500 };

      const treeRes = await fetch(
        `https://api.github.com/repos/${ORG}/${REPO}/git/trees/${encodeURIComponent(sha)}?recursive=1`,
        { headers: { "Accept": "application/vnd.github+json" } }
      );
      if (!treeRes.ok) return { ok: false, errStatus: treeRes.status };

      const data = await treeRes.json();
      const tree = Array.isArray(data.tree) ? data.tree : [];

      treeCache = tree;      // guarda em memória
      setCachedTree(tree);   // guarda no localStorage
      return { ok: true, tree };
    } catch (e) {
      console.error(e);
      return { ok: false, errStatus: 0 };
    }
  }

  /* Filtra os arquivos diretamente dentro de docs/<folder>/:
     - mantém somente “blobs” (arquivos), ignorando subpastas;
     - exclui arquivos marcados como EN;
     - devolve { name, path } já ordenados por nome “bonito”. */
  function listDocsFiles(tree, folder) {
    const p1 = `docs/${LANG}/${folder}/`;   // nova estrutura
    const p0 = `docs/${folder}/`;           // legado (fallback)
  
    // helper que coleta arquivos diretos de um prefixo
    const collect = (prefix) => tree
      .filter(n =>
        n.type === "blob" &&
        n.path.startsWith(prefix) &&
        !n.path.slice(prefix.length).includes("/")   // só nível direto
      )
      .map(n => ({
        name: n.path.substring(n.path.lastIndexOf("/") + 1),
        path: n.path
      }));
  
    // tenta nova estrutura; se vazio, usa legado
    let files = collect(p1);
    if (files.length === 0) files = collect(p0);
  
    // ordena por nome exibido
    files.sort((a, b) =>
      prettyName(a.name).localeCompare(prettyName(b.name), "pt-BR", { sensitivity: "base", numeric: true })
    );
    return files;
  }

  /* Se a API estiver indisponível ou rate-limited, ainda mostramos
     um link para a pasta no GitHub, para o usuário não ficar “preso”. */
  function renderFallbackLink(listEl, folder) {
    const a = document.createElement("a");
    a.className = "dz-file-link";
    a.href = ghFolderUrl(folder);
    a.target = "_blank";
    a.rel = "noopener";
    a.textContent = "Ver documentos no GitHub";
    listEl.appendChild(a);
  }

  /* Carrega os arquivos de UMA caixa (uma pesquisa):
     - mostra “Carregando…”;
     - busca/usa a árvore do repo;
     - filtra apenas os arquivos da pasta da pesquisa;
     - cria a lista de links (um por linha) com nome “bonito”.
  */
  async function loadBox(boxEl) {
    if (boxEl.dataset.loaded === "true") return; // evita recarregar a mesma caixa

    const statusEl = boxEl.querySelector(".dz-box__status");
    const listEl   = boxEl.querySelector(".dz-file-list");
    const folder   = boxEl.getAttribute("data-folder"); // ex.: "PNAD", "POF", "PNAD Contínua"

    statusEl.textContent = "Carregando…";
    listEl.innerHTML = "";

    const repoTree = await loadRepoTreeOnce();
    if (!repoTree.ok) {
      const s = repoTree.errStatus;
      statusEl.textContent = s === 403
        ? "Limite de consultas da API do GitHub atingido no momento."
        : (s ? `Erro ao carregar (${s}).` : "Erro ao carregar.");

      // Fallback: ainda oferecemos um link para a pasta no GitHub
      renderFallbackLink(listEl, folder);
      return;
    }

    const files = listDocsFiles(repoTree.tree, folder);
    if (files.length === 0) {
      statusEl.textContent = "Nenhum documento disponível para esta pesquisa.";
      return;
    }

    // Cria os links clicáveis para cada arquivo
    const frag = document.createDocumentFragment();
    files.forEach(f => {
      const a = document.createElement("a");
      a.className = "dz-file-link";
      a.href = rawUrl(f.path);
      a.target = "_blank";
      a.rel = "noopener";
      a.textContent = prettyName(f.name);
      frag.appendChild(a);
    });

    listEl.appendChild(frag);
    statusEl.textContent = "";           // limpa a mensagem de status
    boxEl.dataset.loaded = "true";       // marca como carregado
  }

  /* ==========================
     LÓGICA DO ACORDEÃO
     ==========================
     O container tem um event listener único (delegação de evento).
     Ao clicar no cabeçalho de uma caixa:
       - fechamos outras caixas;
       - alternamos a caixa clicada (abre/fecha);
       - se abrindo pela primeira vez, carregamos a lista via loadBox().
  */

  container.addEventListener("click", (ev) => {
    const btn = ev.target.closest(".dz-box__header");
    if (!btn) return;

    const box   = btn.closest(".dz-box");
    const panel = box.querySelector(".dz-box__panel");
    const chev  = box.querySelector(".dz-box__chevron");

    // Fecha todas as outras caixas
    container.querySelectorAll(".dz-box").forEach(other => {
      if (other !== box) {
        other.querySelector(".dz-box__header").setAttribute("aria-expanded", "false");
        other.querySelector(".dz-box__panel").hidden = true;
        other.querySelector(".dz-box__chevron").textContent = "+";
      }
    });

    // Abre/fecha a caixa atual
    const expanded = btn.getAttribute("aria-expanded") === "true";
    btn.setAttribute("aria-expanded", String(!expanded));
    panel.hidden = expanded;
    chev.textContent = expanded ? "+" : "–";

    // Se acabou de abrir, carrega os arquivos
    if (!expanded) loadBox(box);
  });
})();

</script>