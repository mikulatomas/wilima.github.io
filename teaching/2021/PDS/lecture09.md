---
layout: default
courses: PDS
title: 9. Remote procedure call (RPC)
year: 2021
---


## Remote procedure call (RPC)

#### Užitečné odkazy
* [Distsim knihovna](https://github.com/mikulatomas/distsim)
* [Distsim příklady](https://github.com/mikulatomas/distsim/tree/master/examples)
* [Python `threading` — Thread-based parallelism](https://docs.python.org/3/library/threading.html)
* [Remote procedure call (wiki)](https://en.wikipedia.org/wiki/Remote_procedure_call)
* [SimpleXMLRPCServer Example](https://docs.python.org/3/library/xmlrpc.server.html#simplexmlrpcserver-example)

### Kolekce ``namedtuple``
Pro vytvoření jednoduché třídy na reprezentaci zprávy je vhodné použít ``namedtuple``. Interně se jedná o ``tuple`` a lze s ním tak pracovat.

{% highlight python linenos %}
from collections import namedtuple

Message = namedtuple('Message', ['type', 'value'])

new_message = Message(type='Error', value='Invalid function name.')
{% endhighlight %}

### Informace o funkci
V dnešním úkolu bude užitečné zjistit název funkce a počet a jména argumentů funkce.

{% highlight python linenos %}
def function(a, b):
    return a + b

# name (string)
function.__name__

# list of arguments names (strings)
function.__code__.co_varnames 

{% endhighlight %}

### RPC Server
Úkolem bude vytvořit RPC Server, který půjde spustit jako vlákno v rámci kódu třídy ``Node``. Uživatel při vytvoření RPC Serveru předá seznam funkcí, které bude RPC podporovat. Tyto funkce poté mohou klienti serveru vzdáleně volat s dodanýmy argumenty.

V aktuální verzi bude RPC Server obsluhovat veškeré zprávy, které do daného objektu ``Node`` přijdou. Zprávy které tak nepatří RPC Serveru může zahazovat. Při vytváření RPC Serveru je tedy nutné, aby byl předán objekt ``Node`` a ``logger`` pro logování do souboru daného uzlu.

Jakmile RPC Server obdrží zprávu pro vzdálený výpočet, vytvoří vlákno s vybranou funkcí a argumenty. Vlákno po skončení výpočtu samo pošle výsledek uzlu, který si o výpočet zažádal.

RPC Server je možné ukončit zavoláním metody ``stop()`` (to klasické vlákno neumožňuje).

Kód uzlu ``master`` může vypadat nasledovně:

{% highlight python linenos %}
def master():
    # get current node object
    node = current_process()

    # get logger
    logger = node.get_logger()

    logger.info(f"Starting node.")

    def add(x, a):
        return x + a

    def sub(x, a):
        return x - a

    rpc_server = RPCServer([add, sub], node, logger)

    rpc_server.start()
    rpc_server.join()

    logger.info(f"Shutting down.")
{% endhighlight %}

Jednoduchý klient, který zažádá o jeden výpočet, a po obdržení výsledku skončí:

{% highlight python linenos %}
def client():
    # get current node object
    node = current_process()

    # get logger
    logger = node.get_logger()

    message = RPCMessage('add', [1, 2], None)
    logger.info(f"{message}")
    node.send_to('master', message)

    result = node.recv_from('master')
    logger.info(f"{result}")

    logger.info(f"Starting node.")

    logger.info(f"Shutting down.")
{% endhighlight %}



{% include task.html content="Naprogramujte RPC Server jako potomka třídy <code>threading.Thread</code> dle specifikací výše." %}


