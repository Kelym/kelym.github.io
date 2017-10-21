---
layout: page
title: Categories
permalink: /categories/
published: false
---

Jump to categories:

<div style="padding: 0 0 2em 2em">
{% assign categories = site.categories | sort %}
{% for category in categories %}
 <span class="site-tag">
    <a href="#{{ category | first | slugify }}">
            {{ category[0] | replace:'-', ' ' }} ({{ category | last | size }})
    </a>
</span>
{% endfor %}
</div>



<div id="index" style="padding: 2em 0 0 0">
{% for category in categories %}
  <a name="{{ category[0] }}"></a><h3>{{ category[0] | replace:'-', ' ' }} ({{ category | last | size }}) </h3>
  {% assign sorted_posts = site.posts | sort: 'title' %}
  {% for post in sorted_posts %}
    {%if post.categories contains category[0]%}

      <p><a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}" title="{{ post.title }}"> {{ post.title }}</a> <span class="date">{{ post.date |  date: "%B %e, %Y" }}</span></p>
        
       {% comment %}<p>{{ post.excerpt | strip_html | truncate: 160 }}</p> {% endcomment %}

    {%endif%}
    {% endfor %}
  {% endfor %}
</div>