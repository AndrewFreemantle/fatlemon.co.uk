---
permalink: /tags/
title: Tags
layout: single
---

<h3><i class="fa fa-tags"></i> Tag Cloud</h3>
<ul class="tag-cloud">
{% for tag in site.tags %}
  <li style="font-size: {{ tag | last | size | times: 100 | divided_by: site.tags.size | plus: 70 }}%">
    <a href="#{{ tag | first }}">
      {{ tag | first }}
    </a>
  </li>
{% endfor %}
</ul>

<div class="articles">
{% for tag in site.tags %}
  <div class="article-group">
    {% capture tag_name %}{{ tag | first }}{% endcapture %}
    <h3 id="#{{ tag_name }}"><i class="fa fa-tag"></i> {{ tag_name }}<a href="#" name="{{ tag_name }}" class="invisible">{{ tag_name }}</a></h3>
    {% for post in site.tags[tag_name] %}
    <article class="article-item">
      <a href="{{ root_url }}{{ post.url }}">{{ post.title }}</a>
    </article>
    {% endfor %}
  </div>
{% endfor %}
</div>
