---
layout: default
courses: PDS
title: 6. Použití v láken vrámci uzlu distribuované sítě
year: 2021
---


## Použití vláken v rámci uzlu distribuované sítě

#### Užitečné odkazy
* [Distsim knihovna](https://github.com/mikulatomas/distsim)
* [Distsim příklady](https://github.com/mikulatomas/distsim/tree/master/examples)
* [Python `threading` — Thread-based parallelism](https://docs.python.org/3/library/threading.html)

Dnes si ukážeme jak je možné použít ```Thread``` v rámci objektu ```Node``` v knihovně ```distsim```.

### Broadcast
Minule bylo za úkol naprogramovat jednoduchý broadcast. Každý proces odeslal zprávu všem uzlům a poté počkal na zprávy od všech ostatních uzlů.

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

Dnes upravíme toto řešení, aby se příjem a odeslání zpráv mohlo prokládat. To je možné za pomoci dvou vláken. První vlákno se stará o odesílání a druhé o přijímaní zpráv.

{% highlight python linenos %}
def node_code():
    # get current node object
    node = current_process()

    # get logger
    logger = node.get_logger()

    logger.info(f"Starting node.")

    def outputs(node, logger):
        for node_name in node.out_pipes.keys():
            logger.info(f"Sending msg: Msg from node {node_name}")
            node.send_to(node_name, f"Msg from node {node_name}")

    def inputs(node, logger):
        for node_name in node.in_pipes:
            msg = node.recv_from(node_name)
            logger.info(f"Recieved msg: {msg}")

    out_msg = threading.Thread(target=outputs, args=(node, logger))
    in_msg = threading.Thread(target=inputs, args=(node, logger))

    out_msg.start()
    in_msg.start()

    out_msg.join()
    in_msg.join()

    logger.info(f"Shutting down.")
{% endhighlight %}

Celý příklad včetně logů naleznete [zde](https://github.com/mikulatomas/distsim/tree/master/examples/threads).

### Hearth Beat
Dalším možným příkladem je takzvaný Hearth Beat. V distribuované síti se nachází jeden řídící uzel ```master``` a několik podřízených uzlů ```slave```.

Uzel ```master``` zašle všem ```slave``` uzlům pokyn k zahájení práce. Podřízené ```slave``` uzly zahájí práci a periodicky informují uzel ```master```, že výpočet stále běží (takzvaný hearth beat).

Jakmile nějaký ```slave``` uzel práci dokončí, rovněž o této skutečnosti informuje ```master``` uzel. Jakmile má ```master``` informaci od všech ```slave``` uzlů ukončí se.

{% include task.html content="Naprogramujte implementaci Hearth Beat v knihovně <code>distsim</code>. Veškeré zprávy logujte." %}
