---
layout: page
title: Blog
permalink: /blog/
---

# 📚 Blog de Ciberseguridad

Aquí encontrarás todos mis artículos publicados hasta ahora:

<div class="blog-list">
  {% for post in site.posts %}
    <div class="blog-item" style="margin-bottom: 2rem; padding: 1rem; background: rgba(0,0,0,0.6); border-radius: 8px;">
      <h2 style="margin-bottom: 0.5rem;">
        <a href="{{ post.url }}" style="color: #00ffcc; text-decoration: none;">
          {{ post.title }}
        </a>
      </h2>
      <p style="color: #aaa; font-size: 0.9rem; margin-top: -0.3rem;">
        📅 {{ post.date | date: "%d %b %Y" }} — Categorías: {{ post.categories | join: ", " }}
      </p>
      <p style="color: #ddd;">
        {{ post.excerpt | strip_html | truncatewords: 30 }}
      </p>
      <a href="{{ post.url }}" style="color: #00ffcc; font-weight: bold;">👉 Leer más</a>
    </div>
  {% endfor %}
</div>
