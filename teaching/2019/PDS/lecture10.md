---
layout: lecture
courses: PDS
title: 10. Výpočet infima 
year: 2019
---


# Výpočet infima 

### Užitečné odkazy
* [Python `multiprocessing` — Process-based parallelism](https://docs.python.org/3.4/library/multiprocessing.html)
* [Slidy k předmětu PDS](http://phoenix.inf.upol.cz/~osicka/dist-sys.pdf)

## Využití algoritmu Echo z minula 
Vlnový algoritmus `Echo` lze použít například k výpočtu infima v distribuované síti. Více informací k zadání problému naleznete na slidu č. 30. Jako operaci použíjte `max`.

<img src="/assets/images/PDS/lecture10/img_2.png" class="img-fluid" srcset="/assets/images/PDS/lecture10/img_2@2x.png 2x" />

{% highlight python linenos %}
TOPOLOGY = [[False, True, True, False],
            [True, False, False, True],
            [True, False, False, True],
            [False, True, True, False]]

args = [(6, ), (2, ), (3, ), (1, )]

{% endhighlight %}


> # Úkol
Za pomoci algoritmu `Echo` naprogramujte výpočet infima v distribuované síti.
