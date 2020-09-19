---
layout: default
title: KMI/PDS Paralelní distribuované systémy
lector: Mgr. Jan Tříska, Ph.D.
types: [course, winter]
course: PDS
year: 2021
alert: Z důvodu studijní stáže mě na cvičení do 12.10.2020 zastupuje Mgr. Jan Tříska, Ph.D.
alert_covid: Na vyučovaných předmětech <strong><a href="https://koronavirus.mzcr.cz/aktualni-opatreni/">dodržujte vládní nařízení týkající se COVID-19</a></strong>. V případě <strong>jakýchkoli příznaků se neúčastněte prezenční výuky mých předmětů a <a href="/">kontaktujte mě</a></strong> pro více informací k distanční výuce.
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
Bude doplněno.

### Zápočet
Bude doplněno.
