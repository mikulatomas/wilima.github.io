---
layout: default
courses: PDS
title: 1. Úvod a instalace jazyka Python
year: 2021
---

## Základní informace o jazyku Python
Co o Python říká oficiální web:

{% include quote.html content="Python is a programming language that lets you work quickly and integrate systems more effectively." %}

#### Užitečné odkazy
* [Dokumentace jazyka Python3](https://docs.python.org/3/)
* [The Hitchhiker’s Guide to Python!](https://docs.python-guide.org)
* [Real Python](https://realpython.com)
* [KMI/PHT Programování v jazyce Python](http://trnecka.inf.upol.cz/teaching/pht/)

### Python 2.7 vs 3.x
~~Většina produkce stále používá `Python 2.7`, `Python 3` je však zcela připraven pro ostré nasazení do produkce. `Python 2.7` bude dostávat bezpečnostní update pouze do roku 2020. Více info na [odkazu](https://docs.python-guide.org/starting/which-python/#the-state-of-python-3-2).~~

{% include quote.html content="We are volunteers who make and take care of the Python programming language. We have decided that January 1, 2020, was the day that we sunset Python 2. That means that we will not improve it anymore after that day, even if someone finds a security problem in it. You should upgrade to Python 3 as soon as you can." %}


## Instalace jazyka Python 3
Osobní zkušenost instalace mám pouze na Mac OS a Linux. V instalaci by vám měl pomoct následující [odkaz](https://docs.python-guide.org/starting/installation/). Pokud instalujete Python na vlastní PC, instalujte verzi Python 3.8.


{% include task.html content="Instalace jazyka Python 3.8 na vámi zvolenou platformu." %}

## Pip vs (Ana)Conda
Oba dva balíčkovače pro instalování programů/knihoven v rámci Python ekosystému. Anaconda zaměřená více na vědecký Python (dataminig, machine learning). Conda nejenom pro Python, na PC na univerzitě najdete Condu.

## IDE
Python je možné psát v jakémkoli textovém editoru [případně používat Python interpretr](https://docs.python.org/3/tutorial/interpreter.html).

#### Doporučené Python IDE
* [Visual Studio Code](https://code.visualstudio.com)
* Vim/Emacs (sadomasochisti)
* [Spyder](https://www.spyder-ide.org)
* ~~Atom/SublimeText 3~~ (V kontextu VSC nemá dle mého názoru smysl)
* [Ostatní](https://realpython.com/python-ides-code-editors-guide/)

### Jupyter notebook
Jupyter Notebook doporučuji na experimentování nejenom v jazyce Python. Jupyter umožňuje psát a vyhodnocovat kód rovnou v prohlížeči v takzvaném `jupyter notebook`. Více informací naleznete na [oficiálním webu projektu](http://jupyter.org).

Jupyter Notebook nemusíte mít spuštěný ve webovém prohlížeci. Například MS Visual Code podporuje jeho integraci.

{% include quote.html content="The Jupyter Notebook is an open-source web application that allows you to create and share documents that contain live code, equations, visualizations and narrative text. Uses include: data cleaning and transformation, numerical simulation, statistical modeling, data visualization, machine learning, and much more." %}

{% include task.html content="Zprovoznit libobolné IDE (ideálně Jupyter) a otestovat jednoduchý <code>hello_world.py</code>." %}

`hello_world.py`
{% highlight python linenos %}
print("Hello world!")
{% endhighlight %}

## Základy jazyka Python

Více informací dostupné na odkazu [https://realpython.com/python-first-steps/](https://realpython.com/python-first-steps/). Následující příklady je možné stáhnout ve formě [Jupyter Notebooku](/assets/files/python_basics.ipynb).

### Promněnné

{% highlight python linenos %}
# název proměnné by měl vždy obsahovat podtržítka
nazev_promenne = 'hodnota'
cislo = 1
desetine_cslo = 1.56
boolean = True
{% endhighlight %}

### Seznamy

{% highlight python linenos %}
seznam = []
seznam_s_hodnotami = [1, 2, 3]

# assert testuje zda je podminka splnena, pouzivano casto u testovani
# testuje zda je hodnota vracena seznam_s_hodnotami[0] rovna 1,
# jelikoz na indexu 0 ocekavame jednicku je podminka splnena a assert uspesne probehne
assert seznam_s_hodnotami[0] == 1

# test zda je prvek v seznamu
assert 1 in seznam_s_hodnotami

# delka seznamu
assert len(seznam_s_hodnotami) == 3

# pridani prvku do seznamu, seznam muze obsahovat rozdilene datove typy
seznam_s_hodnotami.append('ahoj')

assert seznam_s_hodnotami == [1, 2, 3, 'ahoj']

# seznamy lze "rezat"

# veskere prvky do indexu 2
assert seznam_s_hodnotami[:2] == [1, 2]

# veskere prvky od indexu 2
assert seznam_s_hodnotami[2:] == [3, 'ahoj']

# Seznamy lze libovolne zanorovat, nebo je pouzivat jako frontu, zasobnik, viz dokumentace
{% endhighlight %}

### Řetězce

{% highlight python linenos %}
retezec = 'abba'

# retezec se chova jako seznam
assert len(retezec) == 4
assert retezec[0] == 'a'

# prirazeni vsak nefunguje
# retezec[0] = 'c'

# podretezec ziskame jednoduse
assert retezec[:2] == 'ab'
{% endhighlight %}

### Tuple

{% highlight python linenos %}
# Tuple narozdíl od seznamu obsahuje neměnné prvky

toto_je_tuple = (1, 3, 2)

print(toto_je_tuple)

# nefunguje
toto_je_tuple[0] = 'ahoj'
{% endhighlight %}


{% highlight console linenos %}

(1, 3, 2)



---------------------------------------------------------------------------

TypeError                                 Traceback (most recent call last)

<ipython-input-51-4dd037267949> in <module>()
      6
      7 # nefunguje
----> 8 toto_je_tuple[0] = 'ahoj'


TypeError: 'tuple' object does not support item assignment
{% endhighlight %}

### Slovníky
{% highlight python linenos %}
slovnik = {'a': 1, 'b': 2}

assert slovnik.get('a') == slovnik['a']
{% endhighlight %}


### Print


{% highlight python linenos %}
var = 'světe'

# Jde to i takto, ale známe lepší
print("Ahoj " + var)

# Takto se formátovalo v Python 3
print("Ahoj {}".format(var))

# Takto by se mělo formátovat teď (Python 3.6+)
print(f"Ahoj {var}")

cislo = 1

# Dokonce není ani nutné převádět číselnou hodnotu na string
print(f"Krasne cislo {cislo}")
{% endhighlight %}

{% highlight console linenos %}
Ahoj světe
Ahoj světe
Ahoj světe
Krasne cislo 1
{% endhighlight %}


### Podmínky


{% highlight python linenos %}
cislo = 1

if cislo < 0:
    print("zaporne")
elif cislo > 0:
    print("kladne")
else:
    print("nula")


# v podminkach je mozne pouzivat klasicke logicke operatory
assert (True and False) == False
assert (True or False) == True
assert (not True) == False

{% endhighlight %}

{% highlight console linenos %}
kladne
{% endhighlight %}


### Cykly


{% highlight python linenos %}
# Ostatní programovací jazyky

my_items = [1, 2, 3, 4, 5]

i = 0
while i < len(my_items):
    print(my_items[i])
    i += 1
{% endhighlight %}

{% highlight console linenos %}
1
2
3
4
5
{% endhighlight %}



{% highlight python linenos %}
# Python

my_items = [1, 2, 3, 4, 5]

# nejcasteji pouzivana iterace, for each
for item in my_items:
    print(item)

# pokud precejenom potrebujeme iterovat v nejakem rozmezi pouzivame range()
for i in range(6):
    print(f"Index: {i}")

# range muzeme pouzit i na sestupnou iteraci
for i in range(5, -1, -1):
    print(f"Sestupny index: {i}")


slovnik = {'a': 1, 'b': 2}

# pomoci for each muzeme krasne iterovat pres slovniky,
# ze slovniku je nutne ziskat seznam dvojic 'klic, hodnota' pomoci .items()
for key, value in slovnik.items():
    print(f"klic {key} hodnota {value}")
{% endhighlight %}

{% highlight console linenos %}
1
2
3
4
5
Index: 0
Index: 1
Index: 2
Index: 3
Index: 4
Index: 5
Sestupny index: 5
Sestupny index: 4
Sestupny index: 3
Sestupny index: 2
Sestupny index: 1
Sestupny index: 0
klic a hodnota 1
klic b hodnota 2
{% endhighlight %}

### Funkce


{% highlight python linenos %}
# nazev funkce rovnez obsahuje podtrzitko
def moje_funkce(argument):
    return argument + 1

assert moje_funkce(1) == 2
{% endhighlight %}

## Python vs. parallel programming
Je Python vhodný pro paralelní/distribuované programování?

### Global Interpreter Lock
GIL je mutex, který pomáhá udržovat informaci o tom co v rámci Pythonu právě běží. Zároveň poskytuje aktuálnímu vlánknu přístup k interním funkcím Python interpretru. V čem je to problém? Pouze jedno vláknu může v jeden okamžik vyhodnocovat kód v interpretru. Multi-thread scripty jsou ve výsledku pomalejší než single-thread.


{% include quote.html content="The GIL is controversial because it prevents multithreaded CPython programs from taking full advantage of multiprocessor systems in certain situations. Note that potentially blocking or long-running operations, such as I/O, image processing, and NumPy number crunching, happen outside the GIL. Therefore it is only in multithreaded programs that spend a lot of time inside the GIL, interpreting CPython bytecode, that the GIL becomes a bottleneck." %}

### Jak vyřešit GIL problém?
Oficiální knihovna `multiprocessing` pro jazyk Python je alternativa řešící problém s GIL. Místo vytváření vláken dochází k vytváření procesů. Každý proces má vlastní GIL a proto ve výsledku nedochází k problému zmíněném výše. Více informací a příklady jsou dostupné [zde](https://charlienewey.github.io/parallel-processing-python/).

## Jak by to tedy správně mělo být?
**Scala** [https://www.scala-lang.org](https://www.scala-lang.org) + **Apache Spark** [http://spark.apache.org](http://spark.apache.org), **Clojure** [https://clojure.org](https://clojure.org), výpočty na GPU **CUDA** [https://developer.nvidia.com/cuda-zone](https://developer.nvidia.com/cuda-zone) (existuje i API do Python).

## Proč je dobré (i tak) umět Python?
Python je jazyk, který získává stále více a více příznivců. Použivají jej firmy jako Google, Apple, Facebook, nebo Seznam.cz. Je to vynikající skriptovací jazyk na psaní prototypů, datamining případně machine learning. Obsahuje nespočet knihoven, které umožňují řešit většinu problému rychle a jednoduše. Více informací o jazyku Python na [odkazu](http://www.bestprogramminglanguagefor.me/why-learn-python).
