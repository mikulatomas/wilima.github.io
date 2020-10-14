---
layout: default
courses: PDS
title: 4. Producent a konzument
year: 2021
---


## Producent a konzument

#### Užitečné odkazy
* [Python `threading` — Thread-based parallelism](https://docs.python.org/3/library/threading.html)
* [Python `Condition` — Thread-based parallelism](https://docs.python.org/3/library/threading.html#condition-objects)
* [Zdrojové kódy z cvičení formou notebooku](/assets/files/2020/lecture04.ipynb)

### Objekt ```Condition```
Podmínka je vždy asociována s nějakým druhem zámku. Podmínce může být zámek předán, případně si vytvoří zámek svůj. Již známe metody ```acquire()``` a ```release()``` jsou předány zámku v objektu ```Condition```.

Novinkou však jsou metody ```wait()``` a ```notify()```. Metoda ```wait()``` uvolní zámek a čeká dokud jej jiné vlákno nepotvrdí metodou ```notify()```, nebo ```notify_all()``` a poté zámek zamče.

Metody ```notify()``` a ```notify_all()``` neuvolňují zámek. To znamená, že probuzené vlákno okamžitě nevysoupí ze svého volání ```wait()```, ale počká až se vlákno které volalo ```notify()``` vzdá vlastnictví zámku.

`Condition` je většinou používán následovně:

{% highlight python linenos %}
# Consume one item
with cv:
    while not an_item_is_available():
        cv.wait()
    get_an_available_item()

# Produce one item
with cv:
    make_an_item_available()
    cv.notify()
{% endhighlight %}

### Producent a konzument
V následující implementaci dochází k problému konzumace prázdné fronty.

{% highlight python linenos %}
from threading import Thread, Lock
import time
import random

queue = []
lock = Lock()

class ProducerThread(Thread):
    def run(self):
        # Vytvoreni seznamu [0, 1, 2, 3, 4]
        nums = range(5) 
        
        # Umozneni zapisu do globalni promenne (append)
        global queue
        
        for _ in range(10):
            # Vyber nahodneho cisla z [0, 1, 2, 3, 4]
            num = random.choice(nums) 
            
            with lock:
                queue.append(num)
                print("Produced {}".format(num))
            
            time.sleep(random.random())


class ConsumerThread(Thread):
    def run(self):
        # Umozneni zapisu do globalni promenne (pop)
        global queue
        
        for _ in range(10):
            with lock:
                # Demonstrace problemu, pop na prazdnou frontu
                # Konzument by mel cekat
                if not queue:
                    print("Nothing in queue, but consumer will try to consume")

                num = queue.pop(0)
                print("Consumed {}".format(num))
            
            time.sleep(random.random())


ProducerThread().start()
ConsumerThread().start()
{% endhighlight %}

{% include task.html content="Předchozí kód upravte za pomoci <code>Condition</code> tak, aby nedocházelo ke konzumaci prázdné fronty. Pokud ve frontě nebude žádné číslo, konzument musí čekat." %}

{% include task.html content="Upravte úkol tak aby velikost fronty byla omezena. Například na délku 5 hodnot ve frontě. K řešení využíjte <code>Condition</code>." %}
