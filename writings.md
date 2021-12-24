---
layout: page
title: Writings
permalink: /writings/
---

<ul class="post-list">
{% for post in site.posts %}
	{% if post.tags contains 'english' %}
		<li>
			<span>{{ post.date | date_to_string }}</span>
			<h3>
				<a href="{{post.url}}">{{ post.title }}</a>
			</h3>
		</li>
	{% endif %}
{% endfor %}
</ul>