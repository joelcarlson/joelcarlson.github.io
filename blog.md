---
layout: page
title: Blog
permalink: /blog/
---
<p class="message">
  This is where I will post..well...blog related stuff.  Will it be different from my "Research" page? Hard to say!
</p>

<div class="posts">
  {% for post in site.posts %}
  <div class="post">
    <h1 class="post-title">
      <a href="{{ post.url }}">
        {{ post.title }}
      </a>
    </h1>

    <span class="post-date">{{ post.date | date_to_string }}</span>

    {{ post.excerpt }}
  </div>
  {% endfor %}
</div>

