---
layout: lecture
courses: PDS
title: 3. Základy se semafory
year: 2020
---


# Základní práce se semafory

### Užitečné odkazy
* [Python Semaphore Objects](https://docs.python.org/3/library/threading.html#semaphore-objects)
* [Zdrojové kódy z cvičení formou notebooku](/assets/files/lecture02.ipynb)

## Objekt `Semaphore`
`Semaphore` je třída reprezentující synchronizační primitivum semafor. Semafor je při vytvoření nastaven na určitou hodnotu. Z teorie známe dvě funkce `P()` a `V()`, které tuto hodnotu mění, v Python3 jsou tyto funkce pojmenovány `acquire()` a `release()`.

> # Úkol
Pomoci semaforu můžeme elegantně implementovat takzvaný `mutex` a vyřešít tím problém z předchozí hodiny. Upravte tedy sdílený čítač tak, aby byla hodnota vždy správná

{% highlight python linenos %}
import threading

def increment():
    global counter

    for i in range(100000):
        counter += 1

# Globální counter        
counter = 0    

thread1 = threading.Thread(target=increment)
thread2 = threading.Thread(target=increment)

# Spustíme thread
thread1.start()
thread2.start()

# Počkáme na to až thread doběhne
thread1.join()
thread2.join()

print(counter)
{% endhighlight %}

> # Úkol
Následující kód doplňte o synchronizaci, tak aby byl `print()` prováděn ve správném pořadí. Synchronizace by neměla být příliš silná, například nás nezajímá v jakém pořadí bude vypsáno `Print before B` a `Print before A`.

{% highlight python linenos %}
import threading
import time
import random

def thread_a_function():
    # Náhodné čekání, simuluje pomalejší/rychlejší vlákno
    time.sleep(random.uniform(0, 1))
    # Obsah vlákna
    print("Print before B")
    print("A")

def thread_b_function():
    # Náhodné čekání, simuluje pomalejší/rychlejší vlákno
    time.sleep(random.uniform(0, 1))
    # Obsah vlákna
    print("Print before A")
    print("B")


thread_a = threading.Thread(target=thread_a_function)
thread_b = threading.Thread(target=thread_b_function)

thread_a.start()
thread_b.start()
{% endhighlight %}
