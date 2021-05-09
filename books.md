---
layout: default
---
## Recent books
- - -

### Machine learning
<ul>
{% for book in site.books %}
	{% if book.categories contains 'MachineLearning'%}
		<li>
		<a href="{{ book.url }}">{{ book.title }}</a> <tab></tab>{{ book.date | date: "%b %d, %Y"}}
		<br>
		{{ book.excerpt| strip_html }}
		<br><br>
		</li>
	{% endif %}
{% endfor %}
</ul>

