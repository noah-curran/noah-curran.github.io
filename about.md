---
layout: page
title: About
permalink: /about/
---

{% capture index_content %}
{% include_relative index.md %}
{% endcapture %}
{{ index_content | split: "---" | last }}
