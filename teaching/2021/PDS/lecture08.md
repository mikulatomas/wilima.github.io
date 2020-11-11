---
layout: default
courses: PDS
title: 8. Použití v láken vrámci uzlu distribuované sítě (3)
year: 2021
---


## Použití vláken v rámci uzlu distribuované sítě (3)

#### Užitečné odkazy
* [Distsim knihovna](https://github.com/mikulatomas/distsim)
* [Distsim příklady](https://github.com/mikulatomas/distsim/tree/master/examples)
* [Python `threading` — Thread-based parallelism](https://docs.python.org/3/library/threading.html)
* [Python ```nonlocal``` statement](https://docs.python.org/3/reference/simple_stmts.html#nonlocal)

#### Distsim changelog 11.11.2020
* ```Node.poll_from``` - [commit 1068bd7](https://github.com/mikulatomas/distsim/commit/ef7380cd29ad4da77a3c70034d741bd0515bd087?branch=ef7380cd29ad4da77a3c70034d741bd0515bd087&diff=split)

### Simulace selhání workera
Dnešním rozšířením bude simulace selhání workera během výpočtu. Nová distribuovaná síť by tedy měla fungovat následovně:

1. Jakmile nastartuje ```worker```, oznámí uzlu ```master```, že je připraven.
2. Nastartovaný ```worker``` se pravidelně hlásí uzlu ```master``` skrze hearth beats.
3. Uzel ```master``` si udržuje aktuálně aktivní uzly ```worker``` a přiřazuje jim úlohy.
4. Práce je přiřazena okamžitě jakmile nějaký uzel ```worker``` oznámí, že práci dokončil, nebo se připojí ```worker``` nový (např. restart po selhání).
5. Jakmile se po nějaké době přestane některý z uzlů ```worker``` hlásit, tak ```master``` uzel ```worker``` vyřadí. V případě, že měl ```worker``` přidělenou práci, ```master``` ji přiradí někomu jinému.
6. Pokud se nějaký ```worker``` restartuje a během nějaké doby se mu ```master``` neozve. Vypne se.
7. Master se vypne jakmile dokončí veškerou práci.

### Deadlock semaforů
Při práci si dávejte pozor na semafory. Pokud někde čekáte na semafor donekonečna, může nastat deadlock.

### Jak simulovat selhání?
Například za pomoci generování náhodného čísla. Celý kód uzlu ```worker``` se může nacházet uvnitř ```while``` cyklu, ten se stará o samotný "restart".

{% include task.html content="Naprogramujte rozšířenou verzi Hearth Beats s podporou selhání uzlu <code>worker</code>." %}


