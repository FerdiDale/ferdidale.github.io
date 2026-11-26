---
layout: page
permalink: /jamgames
title: Jam Games
---

<div class="projects-list">

{% assign sorted_projects = site.jamgames | sort: "order" | reverse %}
{% for project in sorted_projects %}

<a href="{{ project.url | relative_url }}"
   class="project-card">

    <video
      class="project-video"
      muted
      loop
      playsinline
      preload="metadata"
      poster="{{ project.thumbnail | relative_url }}">

        <source
            src="{{ project.video | relative_url }}"
            type="video/mp4">
    </video>

    <div class="project-info">

        <h2>{{ project.title }}</h2>

        <p>
            {% if project.platforms %}<strong>Platform(s):</strong>
              {{ project.platforms }}<br>{% endif %}
              
            {% if project.genres %}<strong>Genre(s):</strong>
              {{ project.genres }}<br>{% endif %}

            {% if project.tools %}<strong>Engine and tools:</strong>
            {{ project.tools }}<br>{% endif %}
            
            {% if project.teamsize %}<strong>Team size:</strong>
              {{ project.teamsize }}<br>{% endif %}

            {% if project.role %}<strong>Role:</strong>
            {{ project.role }}<br>{% endif %}

            {% if project.gamejam %}<strong>Game Jam:</strong>
            {{ project.gamejam }}<br>{% endif %}

            {% if project.duration %}<strong>Time spent on project:</strong>
            {{ project.duration }}<br>{% endif %}

            {% if project.status %}<strong>Status:</strong>
            {{ project.status }}<br>{% endif %}
        </p>

        {% if project.technologies %}
        <div class="tech-tags">
            {% for tech in project.technologies %}
            <span class="chip">{{ tech }}</span>
            {% endfor %}
        </div>
        {% endif %}

    </div>

</a>

{% endfor %}

</div>