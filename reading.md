---
layout: default
---
## Recent books
- - -
## Machine learning
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
