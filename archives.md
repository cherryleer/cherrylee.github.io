---
layout: nil
comments: true
---

<article class="main-article">
{% capture post_year1 %}{{ 'now' | date: '%Y-%m' }}{% endcapture %}

<h3>{{ post_year1 }}</h3>
<ul class="article-year clearfix">

{% for post in site.posts %}

{% capture post_year2 %}{{ post.date | date: '%Y-%m' }}{% endcapture %}

{% if post_year1 != post_year2 %}

{% assign post_year1 = post_year2 %}

</ul>
<h3>{{ post_year1 }}</h3>
<ul class="article-year clearfix">
{% endif %}

<li>
<span>{{ post.date | date_to_utc | date: '%Y-%m-%d' }}</span>
<a href="{{site.url}}{{ post.url }}">{{ post.title }}</a>
</li>
{% endfor %}
</ul>
</article>
