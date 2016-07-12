---
layout: page
title: Research
permalink: /research/
---

<p class="message">
My research goals involve creating methods for improving treatment outcomes, predicting disease prognosis, and implementing these methods as clinical decision support systems.
</p> 

<div class="posts">
	{% for post in site.research %}
	<div class="post">
		<h2 class="post-title">
			<a href="{{ post.url }}">
				{{ post.title }}
			</a>
		</h2>
		<a href="{{ post.url }}">
			<img src="{{ post.thumbnail }}">
		</a>

		{{ post.excerpt }}
	</div>
	{% endfor %}
</div>




