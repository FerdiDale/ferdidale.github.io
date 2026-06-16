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
        preload="metadata"
        poster="{{ project.thumbnail | relative_url }}">

        <source
            src="{{ project.video | relative_url }}"
            type="video/mp4">
    </video>

    <div class="project-info">

        <h2>{{ project.title }}</h2>

        <p>
            <strong>Duration:</strong>
            {{ project.duration }}<br>

            <strong>Engine:</strong>
            {{ project.engine }}<br>

            <strong>Languages:</strong>
            {{ project.languages }}<br>

            <strong>Role:</strong>
            {{ project.role }}
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

<script>
document.querySelectorAll('.project-card').forEach(card => {

    const video = card.querySelector('video');

    card.addEventListener('mouseenter', () => {
        video.play();
    });

    card.addEventListener('mouseleave', () => {
        video.pause();
        video.currentTime = 0;
    });

});
</script>