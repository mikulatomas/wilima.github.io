---
layout: lecture
courses: PDS
title: 2. Práce s vlákny
year: 2019
---


# Základní práce s vlákny

### Užitečné odkazy
* [Python `threading` — Thread-based parallelism](https://docs.python.org/3/library/threading.html)
* [Zdrojové kódy z cvičení formou notebooku](/assets/files/lecture01.ipynb)

## Objekt `Thread`
`Thread` třída reprezentuje aktivitu běžící v separátním vlákně. Jsou zde dvě možnosti jak objektu `Thread` specifikovat aktivitu: předání `callable object` do konstruktoru třídy `Thread`, nebo přepsání metody `run()`. Pro jednoduchost budeme používat první možnost.

{% highlight python linenos %}
import threading

def example_activity():
    print("Hello World")

example_thread = threading.Thread(target=example_activity)

example_thread.start()
{% endhighlight %}


Při vytváření objektu `Thread` je rovněž možné předat aktivitě argumenty.

{% highlight python linenos %}
import threading

def example_activity_with_args(number):
    print(f"Your number is {number}")

example_thread = threading.Thread(target=example_activity_with_args, args=(10,))

example_thread.start()
{% endhighlight %}

## Modifikace globální proměnné v rámci vlákna
Pokud potřebujeme modifikovat proměnnou v rámci vlákna musíme použít klíčové slovo `global`.

{% highlight python linenos %}
import threading

text = 'ahoj '

def thread_function():
    global text

    text += 'svete'

thread = threading.Thread(target=thread_function)

thread.start()
thread.join()

print(text)
{% endhighlight %}

## Problém sdíleného čítače
Máme dvě vlákna, která inkrementují sdílený čítač. Každé vlákno má čítač postupně zvýšit o 100 000. Očekávaná výsledná hodnota je 200 000, proč tomu tak ne vždy je?

> # Úkol
Vytvořte dvě vlákna, která se uspí na náhodně dlouhou dobu (v rozsahu několika sekund) a vypíší výstup na obrazovku.

> # Úkol
Naprogramujte úlohu sdíleného čítače a vyzkoušejte zda opravdu nefunguje.

> # Úkol
Mějme tři pole `a`, `b`, `c` s celými čísly. Je známo, že některé číslo se vyskytuje v každém poli. Paralelně nalézněte nejmenší index `i`, `j`, `k` pro které platí `a[i] == b[j] == c[k]`, hodnoty vypište na obrazovku. Úkol lze vyřešit tak, že není nutné použít synchronizaci.
<br><br>
> **Ukázkový vstup problému**
```
a = [3,4,5,6,7]
b = [1,2,5,7,9]
c = [3,4,6,7,9]
```
> **Ukázkový výstup**
```
Thread 1 result: 7
Thread 2 result: 7
Thread 3 result: 7
```
