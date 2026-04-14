---
layout: page
title: Deployed Projects
icon: fas fa-server
order: 5
---

Security systems, infrastructure deployments, and technical projects built and validated in real environments.

---

## Security & Infrastructure

{% assign project_posts = site.posts | where_exp: "post", "post.categories contains 'Projects'" | sort: "date" | reverse %}
{% for post in project_posts %}
### [{{ post.title }}]({{ post.url }})
{{ post.excerpt | strip_html | truncatewords: 25 }}
[Read more]({{ post.url }})

{% endfor %}
