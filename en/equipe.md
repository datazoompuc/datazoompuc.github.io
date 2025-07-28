---
layout: default
title: Our team
lang: en
---

{% assign current_lang = page.lang | default: 'en' %}

<style>

p {
  margin-bottom: 0.5rem !important;
}

a {
  color: #369;
  font-family: inherit;
  text-decoration: none;
  font-weight: 300;
}

h3 {
  font-weight: 300;
}

a:hover {
  color: #147;
  text-decoration: none;
}

a:visited {
  color: #525;
}

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
    color: #369;
    font-size: 2rem;
    margin-bottom: 0.5rem;
    text-align: center;
}

.team-card.team-name{
     margin-bottom: 2rem;
}

.team-role {
    color: #3498db;
    font-weight: 600;
    font-size: 1.5rem;
    max-width: 100%;
}

.team-year {
    color: #3498db;
    font-weight: 400;
    font-size: 1.5rem;
}

</style>

# Our team

<div class="team-grid">
{% for member in site.data.equipe[current_lang] %}
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

## Operational managers

<div class="ex-team-grid">
{% for member in site.data.gerentes[current_lang] %}
  <div class="ex-team-card">
    {% if member.site %}
      <h3 class="team-name"><a href="{{ member.site }}" target="_blank">{{ member.nome }}</a></h3>
    {% else %}
      <h3 class="team-name">{{ member.nome }}</h3>
    {% endif %}
    <p class="team-role">{{ member.formacao }}</p>
    <p class="team-year">{{ member.periodo }}</p>
  </div>
{% endfor %}
</div>

## Research assistants

<div class="ex-team-grid">
{% for member in site.data.assistentes %}
  <div class="ex-team-card">
    {% if member.site %}
      <h3 class="team-name"><a href="{{ member.site }}" target="_blank">{{ member.nome }}</a></h3>
    {% else %}
      <h3 class="team-name">{{ member.nome }}</h3>
    {% endif %}
    <p class="team-year">{{ member.periodo }}</p>
  </div>
{% endfor %}
</div>

# Acknowledgements

We would like to thank everyone who contributed their time and knowledge throughout the development of this project. Your suggestions were essential for the improvement of this initiative.

<div class="ex-team-grid">
{% for member in site.data.agradecimentos[current_lang] %}
  <div class="ex-team-card">
    {% if member.site %}
      <h3 class="team-name"><a href="{{ member.site }}" target="_blank">{{ member.nome }}</a></h3>
    {% else %}
      <h3 class="team-name">{{ member.nome }}</h3>
    {% endif %}
    <p class="team-role">{{ member.formacao }}</p>
  </div>
{% endfor %}
</div>