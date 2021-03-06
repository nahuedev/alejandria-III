+++
title = "Fundamentos de NodeJS - I"
description = ""
draft= false
type = ["posts","post"]
tags = [
    "nodejs",
    "js"
    
]
date = 2021-05-02T19:08:16-03:00
categories = [
    "NodeJS",
]
series = ["Hugo 101"]
[ author ]
  name = "Nahue Dev"
[ github ]
  url = "https://github.com/nahuedev"  
+++

![alt text](https://upload.wikimedia.org/wikipedia/commons/d/d9/Node.js_logo.svg "NodeJs")


## Node : or铆genes y filosif铆a 馃殌



**NodeJS** es un entorno de ejecuci贸n de JavaScript fuera del navegador. Se crea en el a帽o 2009,por [Ryan Dahl][Ryan Dahl]. Es un entorno orientado a servidores , lo cual es  muy importante ya que  el hecho de que est茅 fuera del navegador independiza a **JavaScript**.

### Caracter铆sticas principales de NodeJS:

- **Concurrencia**: Es monohilo, con entradas y salidas as铆ncronas.

- **Motor V8**: Creado por Google en 2008 para Chrome. Escrito en C++. Convierte JS en c贸digo m谩quina en lugar de interpretarlo en tiempo real.

- **M贸dulos** : Son piezas de c贸digo muy peque帽as que modularizan nuestros sistemas y ayudan a entender mejor el c贸digo.

- **Orientaci贸n a Eventos**, existe un bucle de eventos que se ejecuta constantemente. Lo que nos permite programar de forma reactiva, lo que quiere decir que podemos programar con la l贸gica de 鈥淐uando sucede algo, se ejecuta esta parte de mi c贸digo y eso a su vez dispara otra parte鈥?.



[Ryan Dahl]: https://en.wikipedia.org/wiki/Ryan_Dahl


## EventLoop: 馃攣  
  
![alt text](https://i.ibb.co/j42621b/event-loop.png "EventLoop")    
El bucle de eventos es lo que permite a Node.js realizar operaciones de E / S sin bloqueo, a pesar de que JavaScript es de un solo subproceso, descargando operaciones al kernel del sistema siempre que sea posible.

Dado que la mayor铆a de los n煤cleos modernos son multiproceso, pueden manejar m煤ltiples operaciones que se ejecutan en segundo plano. Cuando se completa una de estas operaciones, el kernel le dice a Node.js para que se pueda agregar la devoluci贸n de llamada adecuada a la cola de sondeo para que finalmente se ejecute.  
## Bucle de eventos explicado
Cuando Node.js se inicia, inicializa el bucle de eventos, procesa el script de entrada proporcionado (o cae en el REPL , que no se trata en este documento) que puede realizar llamadas API as铆ncronas, programar temporizadores o llamar ***process.nextTick()***, luego comienza a procesar el **"event loop"**.
En el siguiente diagrama se muestra una descripci贸n general simplificada del orden de operaciones del bucle de eventos.

```c
   鈹屸攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?
鈹屸攢>鈹?           timers          鈹?
鈹?  鈹斺攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?
鈹?  鈹屸攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹粹攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?
鈹?  鈹?     pending callbacks     鈹?
鈹?  鈹斺攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?
鈹?  鈹屸攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹粹攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?
鈹?  鈹?       idle, prepare       鈹?
鈹?  鈹斺攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?      鈹屸攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?
鈹?  鈹屸攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹粹攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?      鈹?   incoming:   鈹?
鈹?  鈹?           poll            鈹?<鈹?鈹?鈹?鈹?鈹?鈹?  connections, 鈹?
鈹?  鈹斺攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?      鈹?   data, etc.  鈹?
鈹?  鈹屸攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹粹攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?      鈹斺攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?
鈹?  鈹?           check           鈹?
鈹?  鈹斺攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?
鈹?  鈹屸攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹粹攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?
鈹斺攢鈹?鈹?      close callbacks      鈹?
   鈹斺攢鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?鈹?
```  
***Cada cuadro se denominar谩 una "fase" del bucle de eventos.***   

Cada fase tiene una cola FIFO de devoluciones de llamada para ejecutar. Si bien cada fase es especial a su manera, generalmente, cuando el bucle de eventos ingresa a una fase determinada, realizar谩 cualquier operaci贸n espec铆fica para esa fase, luego ejecutar谩 devoluciones de llamada en la cola de esa fase hasta que la cola se haya agotado o el n煤mero m谩ximo de devoluciones de llamada. ha ejecutado. Cuando se agote la cola o se alcance el l铆mite de devoluci贸n de llamada, el bucle de eventos pasar谩 a la siguiente fase, y as铆 sucesivamente.  
### Resumen de las fases  
- **timers** : esta fase ejecuta devoluciones de llamada programadas por setTimeout()y setInterval().
- **pending callbacks** : ejecuta devoluciones de llamada de E / S diferidas hasta la siguiente iteraci贸n del ciclo.
- **idle, prepare** : solo se usa internamente.
- **poll** : recupera nuevos eventos de E / S; ejecutar devoluciones de llamada relacionadas con E / S (casi todas con la excepci贸n de devoluciones de llamada de cierre, las programadas por temporizadores y setImmediate()); El nodo se bloquear谩 aqu铆 cuando sea apropiado.
- **check** : las setImmediate()devoluciones de llamada se invocan aqu铆.
- **close callbacks**: algunas devoluciones de llamada cercanas, por ejemplo socket.on('close', ...).

Entre cada ejecuci贸n del bucle de eventos, Node.js comprueba si est谩 esperando alguna ***E/S*** asincr贸nica o timers y se apaga limpiamente si no hay ninguno

