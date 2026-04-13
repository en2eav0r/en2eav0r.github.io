---
layout: page
title: Achievements
icon: fas fa-trophy
order: 4
---

A record of the milestones that define my journey in cybersecurity, from first-year competitions at IPNET to representing Africa on the international stage.

---

## CTF Competitions

{% assign ctf_posts = site.posts | where_exp: "post", "post.categories contains 'CTF'" | where_exp: "post", "post.categories contains 'Achievements'" | sort: "date" | reverse %}
{% for post in ctf_posts %}
### [{{ post.title }}]({{ post.url }})
{{ post.excerpt | strip_html | truncatewords: 25 }}
[Read more]({{ post.url }})

{% endfor %}

---

## International

{% assign intl_posts = site.posts | where_exp: "post", "post.categories contains 'International'" %}
{% for post in intl_posts %}
### [{{ post.title }}]({{ post.url }})
{{ post.excerpt | strip_html | truncatewords: 25 }}
[Read more]({{ post.url }})

{% endfor %}

---

## Community

{% assign community_posts = site.posts | where_exp: "post", "post.categories contains 'Community'" %}
{% for post in community_posts %}
### [{{ post.title }}]({{ post.url }})
{{ post.excerpt | strip_html | truncatewords: 25 }}
[Read more]({{ post.url }})

{% endfor %}

---

## Academic

{% assign acad_posts = site.posts | where_exp: "post", "post.categories contains 'Academic'" %}
{% for post in acad_posts %}
### [{{ post.title }}]({{ post.url }})
{{ post.excerpt | strip_html | truncatewords: 25 }}
[Read more]({{ post.url }})

{% endfor %}
