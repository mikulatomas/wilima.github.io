---
layout: default
courses: PDS
title: 10. MapReduce - Wordcount
year: 2021
---


## MapReduce - Wordcount

#### Užitečné odkazy
* [Distsim knihovna](https://github.com/mikulatomas/distsim)
* [Distsim příklady](https://github.com/mikulatomas/distsim/tree/master/examples)
* [MapReduce: Simplified Data Processing on Large Clusters](http://static.googleusercontent.com/media/research.google.com/cs//archive/mapreduce-osdi04.pdf)

### Výpočetní úloha
V zadaném úkolu bude MapReduce provádět jednoduchou úlohu. Pro vstupní soubory spočíta četnost výskytu slov. V našem případě budou slova tvořena jedním písmenem.

Ukázkový vstupní soubor může vypadat následovně:

{% highlight python linenos %}
a b c a a b c
{% endhighlight %}

Sada vstupních souborů se bude nacházet v adresáři ``data``. Zdrojové soubory budou mít příponu ``.txt``.

### Architektura MapReduce
V rámci simulované sítě vytvořenou knihovnou ``distsim`` bude architektura MapReduce vypadat následovně:

1. jeden ``master`` uzel
2. několik ``worker`` uzlů provádějící ``map`` nebo ``reduce``

Podstatné jsou dva parametry. 

#### Parametr ``R``
Určuje počet ``reduce`` operací. Parametr ``R`` hraje důležitou roli v hashovací funkci zmíněné níže.

### Uzel ``master``
Uzel ``master`` má několik povinností:

* Rozešlě ``map`` požadavek všem uzlům ``worker``.
* Počká jakmile budou ``map`` výsledky hotovy pro všechny vstupní soubory.
* Jakmile jsou části ``map`` hotovy, rozešle požadavek ``reduce`` všem ``R`` uzlům typu ``worker``.
* Během těchto všech operací zasílá informační zprávy ``ping`` všem uzlům typu ``worker`` a kontroluje jejich odpověď.

### Uzel ``worker``
Uzel ``worker`` má v sobě uložené dvě funkce ``map`` a ``reduce``. Uzel ``woker`` má několik povinností:

* Periodicky odpovída na zprávy typu ``ping``.
* V případě přijetí zprávy ``map`` si otevře příslušné soubory a provede funkci ``map``. Výsledek uloží do speciálního souboru a jeho název pošle uzlu ``master``.
* V případě přijetí zpávy ``reduce`` si otevře příslušné soubory a provede funkci ``reduce``. Výsledek uloží do speciálního souboru a jeho název pošle uzlu ``master``.

### Speciální soubor
Jak již bylo řečeno, uzel ``worker`` výsledky operace ``map`` ukládá do speciálního souboru. Jeho název se skládá ze dvou částí. První část získame pomoci hashovací funkce ``hash(key) mod R``, ``key`` je aktuálně zpracovávaný znak ze souboru. Druhá část je tvořena unikátním UUID vytvořeném na začátku výpočtu.

{% highlight python linenos %}
import hashlib
import uuid

# number of reducers
R = 4

# char, for example 'a'
s = 'a'

prefix = int(hashlib.sha1(s.encode()).hexdigest(), 16) % R
worker_task_id = str(uuid.uuid1())

filename = f'{prefix}_{worker_task_id}'
{% endhighlight %}

### Zpracování požadavku ``map`` v uzlu ``worker``
Jakmile uzel ``worker`` obdrží zprávu ``map`` otevře si přijaté soubory a provede funkci ``map``. V našem případě to bude uložení hodnot ve formátu ``znak: četnost`` do speciálně vytvořeného souboru.

Příklad speciálního souboru, všimněte si, že se opakuje řádek ``a 1``. To znamená, že vstupní soubor poslaný uzlu ``worker`` k mapování obsahoval dvě slova ``a``.

{% highlight python linenos %}
a 1
b 1
a 1
c 1
{% endhighlight %}

Příklad funkce ``map``:

{% highlight python linenos %}
def map_emit_fun(r):
    def emit(key, value):
        prefix = int(hashlib.sha1(s.encode()).hexdigest(), 16) % r
        worker_task_id = str(uuid.uuid1())

        filename = f'{prefix}_{worker_task_id}'
        append_line(filename, f"{key} {value}")

    return emit

def map_count (file, emit):
    content = file_to_string(file)
    words = content.split(" ")

    for word in words:
        emit(word, 1)
{% endhighlight %}


### Vytvoření ``reduce`` požadavků v uzlu ``master``
Jakmile uzel ``master`` obdrží výsledky od všech uzlů ``worker`` vykonávajícíh ``map`` (všechny vstupní soubory byly mapovány). Použije stejnou hashovací funkci pro rozdělení mapovacích souborů mezi ``R`` uzlů typu ``worker``.

Pokud je ``R = 2`` rozdělí uzel ``master`` speciální sobory s názem ``hash_uuid`` dle prefixu a rozdělené soubory (cesty k nim) zašlě uzlům ``worker``.

### Zpracování požadavku ``reduce`` v uzlu ``worker``
Jakmile uzel ``worker`` obdrží zprávu ``reduce`` otevře si přijaté soubory a provede funkci ``reduce``. V našem případě se bude jednat o součet četností informace ve tvaru ``znak: četnost``. Díky použití hashovací funkce bude každý worker provádět součet pro určitou podmnožinu znaků.

Ukázkový vstupní speciální soubor do operace ``reduce``:
{% highlight python linenos %}
a 1
b 1
a 1
c 1
{% endhighlight %}

Vysledek poté uloží do souboru a zašlě informaci uzlu ``master``.

Příklad výsledku po operaci ``reduce``:
{% highlight python linenos %}
a 2
b 1
c 1
{% endhighlight %}

Příklad funkce ``reduce``:

{% highlight python linenos %}
def reduce_emit_fun(reduce_id):
    def emit(key, value):
        worker_task_id = str(uuid.uuid1())

        filename = f'{reduce_id}_{worker_task_id}'
        append_line(filename, f"{key} {value}")

    return emit

def reduce_count(key, values, emit):
    emit(key, len(values))
{% endhighlight %}

{% include task.html content="Implementujte MapReduce na úkol výpočtu word count pro vstupní data dostupná <a href='/assets/files/2021/dataset.zip'>zde</a>." %}