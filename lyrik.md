---
layout: page
title: Lyrik
permalink: /lyrik/
---

<ul class="post-list">
{% for post in site.posts %}
	{% if post.tags contains 'lyrik' %}
		<li>
			<span>{{ post.date | date_to_string }}</span>
			<h3>
				<a href="{{post.url}}">{{ post.title }}</a>
			</h3>
		</li>
	{% endif %}
{% endfor %}
</ul>