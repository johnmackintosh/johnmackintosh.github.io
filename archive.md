---
layout: page
bigimg: /img/DSC_0096.JPG

---


<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{post.date | date_to_string }} {{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
