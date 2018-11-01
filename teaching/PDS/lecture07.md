---
layout: lecture
courses: PDS
title: 7. Simulátor distribuované sítě
---


# Simulátor distribuované sítě

### Užitečné odkazy
* [Python `multiprocessing` — Process-based parallelism](https://docs.python.org/3.4/library/multiprocessing.html)

## Distribuovaná síť
Distribuovaná síť obsahuje několik uzlů a komunikačních kanálů, které propojují jednotlivé uzly. Pomoci komunikačních kanálů mají uzly možnost zasílat a přijímat zprávy.

Distribuovaná síť o třech uzlech může vypadat následovně:

<img src="/assets/images/PDS/lecture07/img_0.png" class="img-fluid" srcset="/assets/images/PDS/lecture07/img_0@2x.png 2x" />

Topologii takové sítě můžeme zapsat stejně jako orientovaný graf:

{% highlight python linenos %}
TOPOLOGY = [
    [False, True, False],
    [False, False, True],
    [True, False, False]
]
{% endhighlight %}

Pokud se podíváme na uzel `N1` vidíme, že obsahuje jeden vstupní a jeden výstupní kanál.

<img src="/assets/images/PDS/lecture07/img_1.png" class="img-fluid" srcset="/assets/images/PDS/lecture07/img_1@2x.png 2x" />

Každému uzlu je možné předat zdrojový kód, který má uzel vykonávat. V rámci tohoto zdrojového kódu jsou přístupny všechny komunikační kanály a název daného uzlu. Volitelně by mělo být možné zjistit název procesu z/do kterého komunikační kanál vede.

{% highlight python linenos %}
from multiprocessing import current_process

def function():
    """Function for node"""

    node = current_process()

    for connection in node.out_pipes:
        connection.send("Msg from node {}".format(node.name))

    for connection in node.in_pipes:
        connection.recv()

    time.sleep(2)

{% endhighlight %}

V prvním stádiu by měla knihovna fungovat následovně:

{% highlight python linenos %}
TOPOLOGY = [[False, True, False],
            [False, False, True],
            [True, False, False]]

# functions - pole funkci pro uzly, delky rovné počtu uzlů

NETWORK = Network(functions, TOPOLOGY)

NETWORK.start()
NETWORK.join()
{% endhighlight %}

## Třída `multiprocessing.Process`
Reprezentuje aktivitu, která běží v samostatném procesu. Pro reprezentaci použijeme třídu `Process` s upravenou funkcionalitou. Více informací naleznete [zde](https://docs.python.org/3.4/library/multiprocessing.html#multiprocessing.Process).

## Třída `multiprocessing.Pipe`
Komunikační kanál mezi dvěma procesy. Po vytvoření objektu `Pipe` je vrácena dvojice objektu `Connection`. První reprezentuje cílový uzel (můžeme pouze přijímat), druhý počáteční (může pouze odesílat). Objekt budeme vytvářet s parametrem `duplex=False`, ten zařídí, že komunikační kanál je jednosměrný.

{% highlight python linenos %}
client, server = Pipe(duplex=False)

...

# Odešleme zprávu
client.send("Test")

...

# Příjmeme zprávu na druhém konci
server.recv()
{% endhighlight %}

## Logování
V rámci multiprocessingu je možné používat globální logger. Použítí je vidět na příkladu:

{% highlight python linenos %}
import logging
import multiprocessing

LOGGER = multiprocessing.log_to_stderr()
LOGGER.setLevel(logging.INFO)

def function():
    """Function for node"""

    node = current_process()

    for output_node in node.out_pipes:
        output_node.send("Msg from node {}".format(node.name))

    for input_node in node.in_pipes:
        LOGGER.info("Node: {}, Msg: {}".format(node.name,
                                               input_node.recv()))

    time.sleep(2)
{% endhighlight %}

V pozdější verzi by měla knihovna umět logovat do souborů, názvy těchto souborů budou korespondovat s názvy uzlů.

> # Úkol
Naprogramujte základní verzi knihovny, která umožňí vytvořit distribuovaný systém na základně předané topologie. Knihovnu otestujte na jednoduchém příkladu, kdy každý uzel pošle informační zprávu sousedu.

{% highlight python linenos %}
TOPOLOGY = [[False, True, False],
            [False, False, True],
            [True, False, False]]

Ukázkový log:

[INFO/0] child process calling self.run()
[INFO/MainProcess] process shutting down
[INFO/1] child process calling self.run()
[INFO/1] Node: 1, Msg: Msg from node 0
[INFO/MainProcess] calling join() for process 0
[INFO/2] child process calling self.run()
[INFO/2] Node: 2, Msg: Msg from node 1
[INFO/0] Node: 0, Msg: Msg from node 2
[INFO/1] process shutting down
[INFO/1] process exiting with exitcode 0
[INFO/0] process shutting down
[INFO/2] process shutting down
[INFO/0] process exiting with exitcode 0
[INFO/2] process exiting with exitcode 0
[INFO/MainProcess] calling join() for process 1
[INFO/MainProcess] calling join() for process 2
{% endhighlight %}
