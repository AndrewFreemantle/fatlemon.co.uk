---
permalink: /archives/
redirect_from:
- /archive/
- /posts/
title: Archives
layout: single
---

<div class="articles">
  {% capture first_post_year %}{{ site.posts.first.date | date: '%Y' }}{% endcapture %}
  {% capture current_year %}{{ site.time | date: '%Y' }}{% endcapture %}
  {% if first_post_year == current_year %}
    <h3>This year's posts<a href="#" name="{{ site.time | date: '%Y' }}" class="invisible">{{ site.time | date: '%Y' }}</a></h3>
  {% else %}
    <h3>{{ first_post_year }}<a href="#" name="{{ first_post_year }}" class="invisible">{{ post.date | date: '%Y' }}</a></h3>
  {% endif %}

  {% for post in site.posts %}
    {% unless post.next %}
      <div class="row">
    {% else %}
      {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
      {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
      {% if year != nyear %}
        </div>
        <h3>{{ post.date | date: '%Y' }}<a href="#" name="{{ post.date | date: '%Y' }}" class="invisible">{{ post.date | date: '%Y' }}</a></h3>
        <div class="row">
      {% endif %}
    {% endunless %}
        <div class="col-md-2 date text-right">
          {% assign d = post.date | date: '%-d' %}
          {% case d %}
            {% when '1' or '21' or '31' %}{{ d }}st
            {% when '2' or '22' %}{{ d }}nd
            {% when '3' or '23' %}{{ d }}rd
            {% else %}{{ d }}th
          {% endcase %}{{ post.date | date:'%b' }}
        </div>
        <div class="col-md-10 post">
          <a href="{{ post.url }}">{{ post.title }}</a>
        </div>
  {% endfor %}
  </div>
</div>
