---
layout: default
courses: PDS
title: 5. Simulátor distribuované sítě
year: 2021
---


## Simulátor distribuované sítě

#### Užitečné odkazy
* [Distsim knihovna](https://github.com/mikulatomas/distsim)
* [Distsim příklady](https://github.com/mikulatomas/distsim/tree/master/examples)

### Distsim
Jednoduchá Python knihovna pro simulaci distribuované sítě za pomoci procesů.

### Instalace knihovny
Zatím není dostupná na ```pip```, proto je nutná ruční instalace:

{% highlight bash linenos %}
$ git clone https://github.com/mikulatomas/distsim
$ cd distsim
$ pip install -e .
{% endhighlight %}

### Použití knihovny distsim
Knihovna obsahuje třídy ```Network``` a ```Node```. Uživatelsky nás zajímá zejména třída ```Network```.

Třídě ```Network``` předáme topologii a cestu kde mají být uloženy logy. Níže vidíme příklad jednoduché sítě s dvěma uzly. Uzel ```node1``` a ```node2``` vykonávájí funkci ```node_code``` a nepřímá žádné argumenty. Z uzlu ```node1``` vede jednosměrný kanál do uzlu ```node2```. Podobně je tomu rovněž opačným směrem. V topologii sítě tedy musíme zadefinovat názvy uzlů, jejich odchozí kanály, funkce které mají vykonávat a jejich argumenty.

{% highlight python linenos %}
NETWORK_ARCHITECTURE = {
    'node1': {
        'out': {'node2', },
        'function': node_code,
        'args': ()},
    'node2': {
        'out': {'node1', },
        'function': node_code,
        'args': ()},
}
{% endhighlight %}

Funkce ```node_code``` může vypadat následovně:

{% highlight python linenos %}
def node_code():
    # get current node object
    node = current_process()

    # get logger
    logger = node.get_logger()

    logger.info(f"Starting node.")

    # send msg to each output pipelines
    for node_name in node.out_pipes.keys():
        node.send_to(node_name, f"Msg from node {node.name}")

    # recieve msg from each input pipelines
    for node_name in node.in_pipes:
        msg = node.recv_from(node_name)
        logger.info(f"Recieved msg: {msg}")

    time.sleep(2)

    logger.info(f"Shutting down.")
{% endhighlight %}

Jako poslední krok stačí síť vytvořit a spustit:

{% highlight python linenos %}
# run network
if __name__ == "__main__":
    network = Network(NETWORK_ARCHITECTURE,
                      log_dir=pathlib.Path(__file__).parent.absolute())

    network.start()
    network.join()
{% endhighlight %}

### Ostatní příklady
Pokročilejší funkcionalita knihovny je demonstrována v příkladech [zde](https://github.com/mikulatomas/distsim/tree/master/examples).


{% include task.html content="Nainstalujte a otestujte funkčnost knihovny. Použít můžete dostupné příklady." %}

{% include task.html content="Vytvořte úplnou topologii (každý uzel má spojení s každým). V této topologii každý uzel odešle zprávu všem ostatním a počká na zprávy příchozí. Vše zalogujte." %}
