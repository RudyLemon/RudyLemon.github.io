---
layout: page
title: About
description: 追梦赤子心_Rudy
keywords: Rudy
comments: true
menu: 关于
permalink: /about/
---

不忘初心，

自律，专注，

刻意练习^_^

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
