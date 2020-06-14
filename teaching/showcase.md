---
layout: default
---

## Showcase - želví grafika
Zde budou umísťovány obrazy zaslané na moji emailovou adresu [{{ site.email }}](mailto:{{ site.email }}). Kreativitě se meze nekladou :)

<p style="text-align: center">
{% for image in site.static_files %}
    {% if image.path contains 'images/PAPR' %}
<img class="showcase" src="{{ site.baseurl }}{{ image.path }}">
    {% endif %}
{% endfor %}
</p>
