---
layout: projects
title: Publications
permalink: /projects/
published: true
---

<table class="projlist">
{% for proj in site.projects reversed %}
<tr>
  {% unless proj.workshop %}
    <td class="projimg">
     {%- if proj.image -%}
        <img src="/assets/img/{{ proj.image }}" alt="teaser"/>
     {%- endif -%}
    </td>
    <td class="projtext">
        <strong>{{ proj.title }}</strong> <br>
        <span class="projauthor">{{ proj.author}}</span> <br>
        <p class="projvenue">{{ proj.venue}} {{ proj.year }}</p>
        <div class='projbutton'>
          {% if proj.page %} | <a href="{{proj.page}}" target="_blank">project webpage</a> {% endif %}
          <a href="javascript:toggleblock('{{proj.bibname}}-abs')">abstract</a>
          {% if proj.bibtype %} | <a href="javascript:toggleblock('{{proj.bibname}}-bib')">bibtex</a> {% endif %}
          {% if proj.arxiv %} | <a href="{{proj.arxiv}}" target="_blank">arXiv</a> {% endif %}
          {% if proj.pdf %} | <a href="{{proj.pdf}}" target="_blank">PDF</a> {% endif %}
          {% if proj.code %} | <a href="{{proj.code}}" target="_blank">code</a> {% endif %}
          {% if proj.poster %} | <a href="{{site.url}}/assets/download/{{proj.poster}}" target="_blank">poster</a> {% endif %}
        </div>
        <p class='abstract'>
            <span id='{{proj.bibname}}-abs' style="display:none;"> {{proj.excerpt}}</span>
        </p>
        {% if proj.bibtype %}
        <pre xml:space='preserve' class='bib' id='{{proj.bibname}}-bib' style="display:none;">@{{proj.bibtype}}&#123;{{proj.bibname}},
    title=&#123;{{proj.title}}},
    author=&#123;{{proj.bibauthor}}},
    booktitle=&#123;{{proj.bibbook}}},
    year=&#123;{{proj.year}}}}
{{proj.bib}}</pre>
        {% endif %}
    </td>
  {% endunless %}
</tr>
{% endfor %}
</table>

<br>
<h2>Workshops</h2>

<table class="wksplist">
{% for proj in site.projects reversed %}
<tr>
  {%- if proj.workshop -%}
    <td class="projtext">
        <strong>{{ proj.title }}</strong> <br>
        <span>{{ proj.author}}</span> <br>
        <em>{{ proj.venue}},</em> {{ proj.year }} <br>
        <div class='projbutton'>
          {% if proj.page %} | <a href="{{proj.page}}" target="_blank">project webpage</a> {% endif %}
          <a href="javascript:toggleblock('{{proj.bibname}}-abs')">abstract</a>
          {% if proj.bibtype %} | <a href="javascript:toggleblock('{{proj.bibname}}-bib')">bibtex</a> {% endif %}
          {% if proj.arxiv %} | <a href="{{proj.arxiv}}" target="_blank">arXiv</a> {% endif %}
          {% if proj.pdf %} | <a href="{{proj.pdf}}" target="_blank">PDF</a> {% endif %}
          {% if proj.code %} | <a href="{{proj.code}}" target="_blank">code</a> {% endif %}
          {% if proj.poster %} | <a href="{{site.url}}/download/{{proj.poster}}" target="_blank">poster</a> {% endif %}
          {% if proj.video %} | <a href="{{proj.video}}" target="_blank">video</a> {% endif %}
        </div>
        <p class='abstract'>
            <i id='{{proj.bibname}}-abs' style="display:none;"> {{proj.excerpt}}</i>
        </p>
        {% if proj.bibtype %}
        <pre xml:space='preserve' class='bib' id='{{proj.bibname}}-bib' style="display:none;">@{{proj.bibtype}}&#123;{{proj.bibname}},
    title=&#123;{{proj.title}}},
    author=&#123;{{proj.bibauthor}}},
    booktitle=&#123;{{proj.bibbook}}},
    year=&#123;{{proj.year}}}}
{{proj.bib}}</pre>
        {% endif %}
    </td>
  {%- endif -%}
</tr>
{% endfor %}
</table>



