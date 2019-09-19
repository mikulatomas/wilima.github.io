---
layout: lecture
courses: PDS
title: 9. Vlnový algoritmus Echo 
year: 2019
---


# Vlnový algoritmus Echo

### Užitečné odkazy
* [Python `multiprocessing` — Process-based parallelism](https://docs.python.org/3.4/library/multiprocessing.html)
* [Slidy k předmětu PDS](http://phoenix.inf.upol.cz/~osicka/dist-sys.pdf)

## Separátní logy pro každý proces
Ve třídě `Node` implementujte separátní logger podobně jak je uvedeno níže.

{% highlight python linenos %}
self.logger = logging.getLogger(name_of_the_node)
hdlr = logging.FileHandler('/path/to/log/{}.log'.format(name), mode='w')
formatter = logging.Formatter('%(asctime)s %(levelname)s %(message)s')
hdlr.setFormatter(formatter)
self.logger.addHandler(hdlr)
self.logger.setLevel(logging.INFO)
{% endhighlight %}

## Algoritmus Echo 
Vlnový, centralizovaný algoritmus pro libovolnou topologii. Algoritmus je pro neorientované sítě, je tedy nutné mít obousměrné kanály, případně dvojci kanálů (náš případ).

Pseudokód procesu `initiator`:
{% highlight python linenos %}
for node in neighbors:
	send(token, node)

while rec < len(neighbors):
	recieve(token)
	rec += 1

decide()
{% endhighlight %}

Pseudokód ostatních procesů:
{% highlight python linenos %}
token, source = recieve_any()
father = source
rec += 1

for node in neighbors:
	if node is not father:
		send(token, node)

while rec < len(neighbors):
	recieve(token)
	rec += 1

send(token, father)
{% endhighlight %}

> # Úkol
V simulátoru dist. sítě implementujte vlnový algoritmus Echo.

