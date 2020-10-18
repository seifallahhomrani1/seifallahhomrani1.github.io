---
title : /Posts
author : Seif-Allah
layout : page
---

<ul>
{% for category in site.categories %}
<b>[{{ category | first }}]</b>  <br>
    <ul>
    {% for post in category.last %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
    </ul>

{% endfor %}
</ul>