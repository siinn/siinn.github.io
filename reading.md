---
layout: default
---
### Night reads
What did I read last night? Why does it matter? I am going to share my thoughts
any way.


<div class="card" align="center">
<div style="font-size:25px;font-weight:600;margin:10px 0px 10px 0px">
.<span style="margin-left:0.8em"></span>.<span style="margin-left:0.8em"></span>.</div>
</div>

<table>
{% for post in site.posts %}
    {% if post.categories contains 'ClimateChange'%}

        <tr>

        <td style="vertical-align:top;background:#FFF"  align="left" width="20%">
        <a href="{{ post.url }}"><img src="{{post.cover}}" width="130"></a>
        </td>

        <td style="vertical-align:top;background:#FFF"  align="left" width="80%">
        <a href="{{ post.url }}"><h4>{{ post.title }}</h4></a>
        by {{post.author}}
        <br>
        Date: <blue>{{ post.date | date: "%b, %Y"}}</blue>
        <br>
        {{ post.excerpt| strip_html }}
        </td>

        </tr>
    {% endif %}
{% endfor %}
</table>
