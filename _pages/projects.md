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
            <strong>Platforms:</strong>
              {{ project.platforms }}<br>

            <strong>Engine and tools:</strong>
            {{ project.tools }}<br>

            <strong>Role:</strong>
            {{ project.role }}

            <strong>Time spent on project:</strong>
            {{ project.duration }}<br>
        </p>

        <div class="tech-tags">

        {% for tech in project.technologies %}
            <span class="chip">{{ tech }}</span>
        {% endfor %}

      </div>

    </div>

</a>

{% endfor %}

</div>