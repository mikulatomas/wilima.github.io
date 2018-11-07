---
layout: lecture
courses: PDS
title: 7. Simulátor distribuované sítě (2)
---


# Simulátor distribuované sítě (2)

### Užitečné odkazy
* [Python `multiprocessing` — Process-based parallelism](https://docs.python.org/3.4/library/multiprocessing.html)
* [Class structure in Python](https://medium.com/@daetam/class-structure-in-python-297792428ef0)

## Odbočka: Struktura knihovny 
Naši knihovnu bude vhodné rozdělit minimálně do několika souborů. Dobrým zvykem je udržovat jednu třídu v jednom Python souboru. Soubor by měl být pojmenován malými písmeny, například tedy `nazev_tridy.py`. V tomto souboru je umístěna třída `NazevTridy`. Z ostatních souborů mohu poté provádět import této třídy za pomoci výrazu `from nazev_tridy import NazevTridy`.

Ukázková struktura projektu by tedy mohla být následující:

{% highlight python linenos %}
-rw-r--r--  1 tomasmikula  staff  1032 Oct 31 18:44 main.py
-rw-r--r--  1 tomasmikula  staff  1067 Oct 31 18:29 network.py
-rw-r--r--  1 tomasmikula  staff   681 Oct 30 21:35 node.py
{% endhighlight %}

V souboru `main.py` tedy provádíme základní vytvoření sítě a nastavení topologie. Takto vytvořené třídy můžeme importovat i v rámci interaktivní konzole Pythonu. Nemusíme tedy vůbec vytvářet hlavní soubor.

{% highlight python linenos %}
import logging
import multiprocessing
import time

from multiprocessing.connection import wait
from multiprocessing import Pipe, Process, current_process

# Importy z nasi knihovny
from network import Network

# Konstatnty
TOPOLOGY = [[False, True, False],
            [False, False, True],
            [True, False, False]]

LOGGER = multiprocessing.log_to_stderr()
LOGGER.setLevel(logging.INFO)

# Definice funkci
def function():
    """Function for node"""

    node = current_process()

    for output_node in node.out_pipes.values():
        for connection in output_node:
            connection.send("Msg from node {}".format(node.name))

    for input_node in node.in_pipes.values():
        for connection in input_node:
            LOGGER.info("Node: {}, Msg: {}".format(node.name,
                                                   connection.recv()))

    time.sleep(2)


# main, tento kod nebude spusten pri beznem importu,
# pouze pri spusteni souboru python main.py
if __name__ == '__main__':
    NETWORK = Network([function for i in range(len(TOPOLOGY))], TOPOLOGY)
    NETWORK.start()
{% endhighlight %}

## Rozšíření knihovny: Možnost předávat argumenty uzlům
Aktuálně umí naše knihovna spouštět každý uzel s jiným kódem. Neumí však předávat argumenty funkci v daném uzlu. Upravte knihovnu tak, aby krom seznamu funkcí bylo možné předat i seznamy argumentů.

{% highlight python linenos %}
NETWORK = Network(
    [function1, function2, function3],
    [(1,), (2,), (3,)],
    TOPOLOGY)
{% endhighlight %}

## Rozšíření knihovny: Možnost přijímat zprávy pouze od nějakého uzlu
Do knihovny přidejte funkcionalitu, aby v kódu uzlu bylo možné přijímat zprávy pouze od jiného názvem určeného uzlu.

{% highlight python linenos %}
node = current_process()

# prijme zpravy pouze od Node s nazvem "0"
node.recv_from(0)
{% endhighlight %}


## Rozšíření knihovny: Možnost příjmutí jakékoli zprávy
Do knihovny pridejte funkcionalitu, aby v kódu uzlu bylo možné přijmout náhodnou zprávu z jakéhokoli input kanálu, který nějakou zprávu obsahuje.

{% highlight python linenos %}
node = current_process()

node.recv_any()
{% endhighlight %}

