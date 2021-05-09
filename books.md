---
layout: default
---
## Recent books
- - -
Test 1
<ul>
{% for book in site.books %}
Test 2
	{% if book.categories contains 'Books'%}
		<li>
		<a href="{{ book.url }}">{{ book.title }}</a> <tab></tab>{{ book.date | date: "%b %d, %Y"}}
		<br>
		{{ book.excerpt| strip_html }}
		<br><br>
		</li>
	{% endif %}
{% endfor %}
</ul>

Test 3
