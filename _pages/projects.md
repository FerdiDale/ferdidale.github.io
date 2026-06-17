---
layout: page
permalink: /
title: Projects
---

<div class="projects-list">

{% for project in site.projects %}

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

            {% if project.tools %}<strong>Engine and tools:</strong>
            {{ project.tools }}<br>{% endif %}

            {% if project.role %}<strong>Role:</strong>
            {{ project.role }}<br>{% endif %}

            {% if project.duration %}<strong>Time spent on project:</strong>
            {{ project.duration }}<br>{% endif %}

            {% if project.status %}<strong>Status:</strong>
            {{ project.status }}<br>{% endif %}
        </p>

        {% if page.technologies %}
        <div class="tech-tags">
            {% for tech in page.technologies %}
            <span class="chip">{{ tech }}</span>
            {% endfor %}
        </div>
        {% endif %}

    </div>

</a>

{% endfor %}

</div>