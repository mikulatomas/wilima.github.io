---
layout: default_small
---

# Showcase - želví grafika
Zde budou umísťovány obrazy zaslané na moji emailovou adresu [tomas.mikula01@upol.cz](mailto:tomas.mikula01@upol.cz). Kreativitě se meze nekladou :)
<p style="text-align: center">
{% for image in site.static_files %}
    {% if image.path contains 'images/PAPR' %}
<img class="showcase" src="{{ site.baseurl }}{{ image.path }}">
    {% endif %}
{% endfor %}
</p>
