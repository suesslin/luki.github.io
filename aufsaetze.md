---
layout: page
title: Aufsätze
permalink: /aufsaetze/
---

**Non-German speakers note**: This is the German writings archive. For English works, please refer to [Writings](/writings).

<ul class="post-list">
{% for post in site.posts %}
	{% if post.tags contains 'aufsatz' %}
		<li>
			<span>{{ post.date | date_to_string }}</span>
			<h3>
				<a href="{{post.url}}">{{ post.title }}</a>
			</h3>
		</li>
	{% endif %}
{% endfor %}
</ul>
