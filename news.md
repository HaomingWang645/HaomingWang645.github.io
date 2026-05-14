---
layout: page
title: news archive
permalink: /news/
---

<div class="news news-archive">
  <div class="table-responsive">
    <table class="table table-sm table-borderless">
      {% assign news_award = site.posts | where: "categories", "news_item" %}
      {% assign news_pub = site.posts | where: "categories", "research_pub" %}
      {% assign news_recent = site.posts | where: "categories", "research_recent" %}
      {% assign news_arxiv = site.posts | where: "categories", "research_arxiv" %}
      {% assign news_combined = news_award | concat: news_recent | concat: news_pub | concat: news_arxiv | sort: "date" | reverse %}
      {% for post in news_combined %}
      <tr>
        <th scope="row" style="width: 22%;">{{ post.date | date: "%b %d, %Y" }}</th>
        <td>
          {% if post.categories contains 'news_item' %}
            {{ post.news_text }}{% if post.news_link %} <a href="{{ post.news_link }}" target="_blank">[link]</a>{% endif %}
          {% else %}
            {% assign news_title = post.title | replace: '<span style="color:#c00">', '<strong class="news-venue">' | replace: '</span>', '</strong>' %}
            {% if post.categories contains 'research_pub' %}
              Paper accepted: <a href="{{ post.paper }}" target="_blank">{{ news_title }}</a>
            {% elsif post.categories contains 'research_recent' %}
              New work: <a href="{{ post.paper }}" target="_blank">{{ news_title }}</a>
            {% else %}
              New preprint: <a href="{% if post.arxiv %}{{ post.arxiv }}{% else %}{{ post.paper }}{% endif %}" target="_blank">{{ news_title }}</a>
            {% endif %}
          {% endif %}
        </td>
      </tr>
      {% endfor %}
    </table>
  </div>
</div>

<p style="margin-top:1.5rem;"><a href="{{ site.baseurl }}/">&larr; back to home</a></p>
