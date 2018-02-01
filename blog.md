---
layout: default
---
## Recent posts
- - -
### Python for Data Science
<ul>
{% for post in site.posts %}
	{% if post.categories contains 'Python'%}
		<li>
		<a href="{{ post.url }}">{{ post.title }}</a> <tab></tab>{{ post.date | date: "%b %d, %Y"}}
		<br>
		{{ post.excerpt| strip_html }}
		<br><br>
		</li>
	{% endif %}
{% endfor %}
</ul>
- - -

### Machine learning
<ul>
{% for post in site.posts %}
	{% if post.categories contains 'MachineLearning'%}
		<li>
		<a href="{{ post.url }}">{{ post.title }}</a> <tab></tab>{{ post.date | date: "%b %d, %Y"}}
		<br>
		{{ post.excerpt| strip_html }}
		<br><br>
		</li>
	{% endif %}
{% endfor %}
</ul>

{% comment %}
| Title | Date |
{% for post in site.posts %}{% if post.title %}
|[{{ post.title }}](post.url)  |{{ post.date }}  |{% endif %}{% endfor %}

### Statistics
<ul>
{% for post in site.posts %}
	{% if post.categories contains 'Statistics'%}
		<li>
		<a href="{{ post.url }}">{{ post.title }}</a> <tab></tab>{{ post.date | date: "%b %d, %Y"}}
		<br>
		{{ post.excerpt| strip_html }}
		<br><br>
		</li>
	{% endif %}
{% endfor %}
</ul>
{% endcomment %}


