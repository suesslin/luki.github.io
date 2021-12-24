---
layout: page
title: Schriften
permalink: /schriften/
---

**Non-German speakers note**: This is the German writings archive. For English works, please refer to [Writings](/writings).

<ul class="post-list">
{% for post in site.posts %}
	{% if post.tags contains 'deutsch' %}
		<li>
			<span>{{ post.date | date_to_string }}</span>
			<h3>
				<a href="{{post.url}}">{{ post.title }}</a>
			</h3>
		</li>
	{% endif %}
{% endfor %}
</ul>