---
title: Publications
featured_image: /images/demo/demo-portrait.jpg
years: [2021, 2020, 2019]
layout: page
---

\*These authors contributed equally to this work

<div class="publications">

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}

</div>
