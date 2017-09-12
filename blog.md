---
layout: default
---
## Recent posts
- - -
### on python, pandas, matplotlib, seaborn
<ul>
{% for post in site.posts %}
	{% if post.categories contains 'python'%}
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

### on statistics, machine learning
<ul>
{% for post in site.posts %}
	{% if post.categories contains 'statistics'%}
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
{% endcomment %}


