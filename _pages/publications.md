---
title: Publications
featured_image: /images/img/c2c12_rnascope.png
sidebar_image: /images/img/c2c12_rnascope.png
years: [2023, 2022, 2021, 2020, 2019]
layout: page
---

\*These authors contributed equally to this work

<div class="publications">

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}

</div>
