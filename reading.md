---
layout: default
---
## Recent books
- - -
Test 1
<ul>
{% for book in site.books %}
    Test 2
    {% if book.categories contains 'posts'%}
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
