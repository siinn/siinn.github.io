---
layout: default
---
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a> {{ post.date | date: "%B %-d, %Y"}}
	  {{ post.excerpt | truncatewords:50 }}

    </li>
  {% endfor %}
</ul>
