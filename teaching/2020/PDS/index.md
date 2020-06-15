---
layout: default
title: KMI/PDS Paralelní distribuované systémy
lector: Mgr. Petr Osička, Ph.D.
course_page: http://phoenix.inf.upol.cz/~osicka/pds.html
types: [course, winter]
course: PDS
year: 2020
---

## {{ page.title }}
**Vyučující:** {{ page.lector }}

**Stránky předmětu:** [{{ page.course_page }}]({{ page.course_page }})

### Seznam cvičení
<ul>
{% capture type %}{{ page.course }}{% endcapture %}
{% for lecture_page in site.pages %}
{% if lecture_page.courses contains type and lecture_page.year == page.year %}
<li>
<a href="{{lecture_page.url}}">{{lecture_page.title}}</a>
</li>
{% endif %}
{% endfor %}
</ul>

### Co bude obsahem cvičení PDS
Implementace algoritmů pro synchronizaci procesů, potažmo distribuovaných systémů.

### Zápočet
Zápočtový úkol -- naprogramovat jednoduchý simulátor distribuované sítě, pro jeho splnění je účast na cvičení více než doporučena. Hotový zápočtový úkol odevzdávejte e-mailem. 
