---
layout: post
title: Planificaci�n de procesos
category: PSP
permalink: "blog/planificacion-procesos"
published: no
---

# Algoritmos de planificaci�n de procesos

<br>

Hoy os hablar� de los algunos de los algoritmos que se utilizan para la ordenaci�n de procesos y c�mo aplica las preferencias para estos cada uno de ellos.

# �Por qu� la necesidad de estos?

Estos m�todos nacieron por la necesidad de poder ordenador los procesos de para ganar eficiencia a la hora de trarar con ellos, es decir, son los
encargados de ordenar y dirigir los procesos para asegurar que ninguno de ellos monopolice el uso de la CPU.

# Conceptos b�sicos.

Antes de ver algunos de los algorimos m�s utilizados vamos a dar a conocer algunos aspectos o conceptos para entenderlos mejor.

* **Tiempo de espera:** El tiempo de un proceso permanece en espera en la cola de ejecuci�n.
* **Tiempo de retorno:** Tiempo que va desde que se lanza un proceso hasta que finaliza.
* **Tiempo de respuesta:** Por �ltimo �ste se define por el tiempo que un proceso bloqueado tarda en entrar en ejecuci�n.
* **Uso de CPU:** Procentaje de tiempo que la CPU esta ocupada.
* **Productividad:** N�mero de procesos realizados en una unidad de tiempo.

Y por �ltimo dos tipos de algoritmos:

* **Apropiativo:** Tambi�n conocido como **expulsivo** o **expropiativo**, este tipo de algoritmo nos permite la expulsi�n de procesos para ejecutar un nuevo proceso, poniendo en cola el anterior
* **No Apropiativo:** ESte tipo no nos permite la expulsi�n, por lo que un proceso nuevo no entrar� hasta que termine el anterior.

# Tipos de algoritmo.

Una vez tenemos estos conceptos claros puedo explicaros alguno de estos algoritmos.

## FCFS (*First-Come, First-Served*)

Empezaremos hablando de FCFS o tambi�n llamado FIFO (del ingl�s *First In, First Out*).
Este algoritmo es muy sencillo y simple, pero tambi�n el que menos rendimiento ofrece, b�sicamente en este algoritmo el primer proceso que llega se ejecuta y una vez terminado 
se ejecuta el siguiente.

| Procesos | llegada  | Tiempo uso CPU |
|:--------:|:--------:|:--------------:|
| P1       | 0        | 7              |
| P2       | 2        | 4              |
| P3       | 4        | 1              |
| P4       | 5        | 4              |


## SJF (*Shortest Job First*).

Este algoritmo siempre prioriza los procesos m�s cortos primero independientemente de su llegada y en caso de que los procesos sean iguales utilizara el m�todo FIFO anterior, 
es decir, el orden seg�n entrada. Este sistema tiene el riesgo de poner siempre al final de la cola los procesos m�s largos por lo que
nunca se ejecutar�n, esto se conoce como **inanici�n**.

| Procesos | llegada  | Tiempo uso CPU |
|:--------:|:--------:|:--------------:|
| P1       | 0        | 7              |
| P2       | 2        | 4              |
| P3       | 4        | 1              |
| P4       | 5        | 4              |

SFJ es un m�todo no exclusivo pero tiene una variante llamada SRTF que explicar� m�s abajo.

## SRTF (*Short Remaining Time Next*).

A�adiendo la expulsi�n de procesos a al algoritmo SJF obtenemos SRTF, �ste ser� capaz de expulsar un proceso largo en ejecuci�n para ejecutar otros m�s cortos. El problema que
puede surgir es que un proceso largo puede llegar a expulsarse muchas veces y nunca terminarlo por la ejecuci�n de otros mas cortos.

| Procesos | llegada  | Tiempo uso CPU |
|:--------:|:--------:|:--------------:|
| P1       | 0        | 7              |
| P2       | 2        | 4              |
| P3       | 4        | 1              |
| P4       | 5        | 4              |

## *Round Robin*.

Por �ltimo os hablar� de *Round Robin*, este algoritmo de planificaci�n es uno de los m�s complejos y dif�ciles de implementar, asigna a cada proceso un
tiempo equitativo tratando a todos procesos por igual y con la misma prioridad. 

Este algoritmo es circular, volviendo siempre al primer proceso una vez terminado con el �ltimo,
para controlar este m�todo a cada proceso se le asigna un intervalo de tiempo llamado quantum o
cuanto (para definirlo se utiliza esta regla, el 80% de los procesos tienen que durar menos tiempo
que el quantum definido).

Pueden suceder dos casos con este m�todo (como se aprecia en la imagen inferior):

* El proceso es menor que el quantum: Al terminar antes se planifica un nuevo proceso.
* El proceso es mayor que el quantum: Al terminar el quantum se expulsa el proceso dando paso al siguiente proceso en la lista. Al terminar la iteraci�n se volver� para
terminar el primer proceso expulsado

| Procesos | llegada  | Tiempo uso CPU |
|:--------:|:--------:|:--------------:|
| P1       | 0        | 7              |
| P2       | 2        | 4              |
| P3       | 4        | 1              |
| P4       | 5        | 4              |

# Comparativa.

|                | FCFS  | SJF   | SRTF  | RR    |
|:--------------:|:-----:|:-----:|:-----:|:-----:|
| Tiempo Espera  | 2.75  | 1.5   | 2.75  | 0.5   |
| Tiempo Retorno | 3     | 3     | 2.5   | 4.25  |

Esta tabla comparativa de los mismos procesos llevados a cabo con los cuatro m�todos anteriores
nos muestra los diferentes tiempos promedio que han necesitado para realizarlos.

Podemos deducir gracias a esa imagen que el algoritmo SJF es el m�s centrado, ya que tiene un
buen tiempo de espera y tiempo de retorno. FCFS y SRTF tienen unos tiempos de espera
similares, pero SFJ sigue siendo mejor en este aspecto.

Round Robin tiene el mejor tiempo de espera para los procesos con much�sima diferencia, pero
por el contrario su tiempo de retorno es el m�s alto debido a la expulsi�n de procesos cuando se
termina el quantum.

Por lo tanto es muy dif�cil decidir que m�todo es mejor que el otro y todo depender� de la situaci�n
en la que estemos, siendo muy �til un estudio de lo que necesitamos para utilizar un m�todo u
otro o incluso combinarlos seg�n nuestras necesidades.

Aqu� llegamos al peque�o repaso de los los algoritmos mas utilizados, para cualquier duda no dud�is en preguntarme.

Un saludo. =)