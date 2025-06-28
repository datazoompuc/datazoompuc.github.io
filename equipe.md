---
layout: default
title: Nossa equipe
---

<style>
.team-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
    margin: 2rem 0 8rem 0;
}

.ex-team-grid {
  display: flex;
  gap: 2rem;
  overflow-x: auto;
  padding: 1rem 0;
  margin: 2rem 0 8rem 0;
  scroll-snap-type: x mandatory;
}

.ex-team-card {
  min-width: 200px; /* Set the card width */
  flex-shrink: 0;
  scroll-snap-align: start;
  background: white;
  border-radius: 15px;
  padding: 2rem;
  text-align: center;
  box-shadow: 0 4px 15px rgba(0,0,0,0.1);
  transition: transform 0.3s ease;
  border-top: 4px solid #3498db;
}

.team-card {
    background: white;
    border-radius: 15px;
    padding: 2rem;
    text-align: center;
    box-shadow: 0 4px 15px rgba(0,0,0,0.1);
    transition: transform 0.3s ease;
    border-top: 4px solid #3498db;
}

.team-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 25px rgba(0,0,0,0.15);
}

.ex-team-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 25px rgba(0,0,0,0.15);
}

.team-photo {
    width: 150px;
    height: 150px;
    border-radius: 50%;
    object-fit: cover;
    margin: 0 auto 1rem;
    border: 4px solid #ecf0f1;
}

.team-name {
    font-size: 1.4em;
    font-weight: bold;
    color: #2c3e50;
    margin-bottom: 0.5rem;
}

.team-role {
    color: #3498db;
    font-weight: 600;
    margin-bottom: 1rem;
    font-size: 1.1em;
}

.team-bio {
    color: #7f8c8d;
    line-height: 1.6;
    margin-bottom: 1.5rem;
}

.team-social {
    display: flex;
    justify-content: center;
    gap: 1rem;
}

.team-social a {
    color: #7f8c8d;
    text-decoration: none;
    transition: color 0.3s ease;
}

.team-social a:hover {
    color: #3498db;
}
</style>

# Nossa equipe

<div class="team-grid">
{% for member in site.data.equipe %}
  <div class="team-card">
    <img src="{{ member.photo | relative_url }}" alt="{{ member.nome }}" class="team-photo">
    {% if member.site %}
      <h3 class="team-name"><a href="{{ member.site }}" target="_blank">{{ member.nome }}</a></h3>
    {% else %}
      <h3 class="team-name">{{ member.nome }}</h3>
    {% endif %}
    <p class="team-role">{{ member.cargo }}</p>
    <p class="team-role">{{ member.formacao }}</p>
  </div>
{% endfor %}
</div>

# Alumni

## Gerentes operacionais

<div class="ex-team-grid">
{% for member in site.data.gerentes %}
  <div class="ex-team-card">
    {% if member.site %}
      <h3 class="team-name"><a href="{{ member.site }}" target="_blank">{{ member.nome }}</a></h3>
    {% else %}
      <h3 class="team-name">{{ member.nome }}</h3>
    {% endif %}
    <p class="team-role">{{ member.formacao }}</p>
    <p class="team-role">{{ member.periodo }}</p>
  </div>
{% endfor %}
</div>

## Assistentes de pesquisa

<div class="ex-team-grid">
{% for member in site.data.assistentes %}
  <div class="ex-team-card">
    {% if member.site %}
      <h3 class="team-name"><a href="{{ member.site }}" target="_blank">{{ member.nome }}</a></h3>
    {% else %}
      <h3 class="team-name">{{ member.nome }}</h3>
    {% endif %}
    <p class="team-role">{{ member.periodo }}</p>
  </div>
{% endfor %}
</div>

# Colaboradores

### Agradecemos a todos e todas que contribuíram com seu tempo e conhecimento ao longo da construção deste projeto. Suas sugestões foram essenciais para o aprimoramento desta iniciativa.

<div class="ex-team-grid">
{% for member in site.data.agradecimentos %}
  <div class="ex-team-card">
    {% if member.site %}
      <h3 class="team-name"><a href="{{ member.site }}" target="_blank">{{ member.nome }}</a></h3>
    {% else %}
      <h3 class="team-name">{{ member.nome }}</h3>
    {% endif %}
    <p class="team-role">{{ member.periodo }}</p>
  </div>
{% endfor %}
</div>