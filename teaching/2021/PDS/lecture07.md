---
layout: default
courses: PDS
title: 7. Použití v láken vrámci uzlu distribuované sítě (2)
year: 2021
---


## Použití vláken v rámci uzlu distribuované sítě

#### Užitečné odkazy
* [Distsim knihovna](https://github.com/mikulatomas/distsim)
* [Distsim příklady](https://github.com/mikulatomas/distsim/tree/master/examples)
* [Python `threading` — Thread-based parallelism](https://docs.python.org/3/library/threading.html)
* [Python ```nonlocal``` statement](https://docs.python.org/3/reference/simple_stmts.html#nonlocal)

#### Distsim changelog 4.11.2020
* ```Node.recv_any``` - [commit acb4e96](https://github.com/mikulatomas/distsim/commit/1068bd7c347a06f237f3bd932bcb3b40de94fa29?branch=1068bd7c347a06f237f3bd932bcb3b40de94fa29&diff=split)

### Výraz ```nonlocal```
Umožňuje hledat vazby symbolů v nejbližším nadřazeném prostředí. Výchozí chování je prohledat prostředí lokální. Tento krok je v případě ```nonlocal``` přeskočen.

{% highlight python linenos %}
def function():
    result = None

    def inner_function():
        nonlocal result
        result = True
    
    inner_function()

    print(result)
{% endhighlight %}

Výraz ```nonlocal``` je užitečný pokud vytváříme vlákna v rámci procesu a chceme mezi němi sdílet data.

### Rozšíření hearth beat
Dnešním úkolem bude rozšířit Hearth Beat z minulého cvičení. V minulém cvičení počet úloh odpovídal počtu ```worker``` uzlů. V realitě máme většínou větší počet úkolů než je uzlů typu ```worker```, proto je nutné implementovat postupné opakováné přiřazování práce jednotlivým uzlům.

Rozšířený uzel ```master``` bude mít tedy tyto tři části:
1. **Rozeslání úkolů**
2. **Kontrola Hearth Beats**
3. **Příjem a třídění zpráv**

Uzel ```master``` tedy rozdá práci tak aby zaměstnal všechny nepracující uzly typu ```worker```. Jakmile nějaký ```worker``` dokončí práci, informuje o tom uzel ```master```. Ten si výsledek uloží a poté uzlu pošle práci další.

### Výpadky uzlů ```worker```
V praxi se rovněž může stát, že některý uzel typu ```worker``` během výpočtu nečekane skončí (výpadek sítě, připojení, elektřiny atd.). V takovém případě musí uzel ```master``` stejný úkol zadat některému z volných uzlů ```worker```.



{% include task.html content="Naprogramujte rozšířenou verzi Hearth Beats. Jako problém použíjte zvětšení čísla o jedničku. Uzel <code>master</code> tedy obdrží <code>list</code> hodnot, který je větší než počet uzlů <code>worker</code>. Výsledkem bude seznam, kde každé číslo bude zvětšeno o jedna." %}

{% include task.html content="Naprogramujte rozšířenou verzi Hearth Beats, kde s určitou pravděpodobností budou uzly typu <code>worker</code> vypadávat." %}
