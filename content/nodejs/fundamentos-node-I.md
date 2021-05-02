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


## Node : orígenes y filosifía 🚀



**NodeJS** es un entorno de ejecución de JavaScript fuera del navegador. Se crea en el año 2009,por [Ryan Dahl][Ryan Dahl]. Es un entorno orientado a servidores , lo cual es  muy importante ya que  el hecho de que esté fuera del navegador independiza a **JavaScript**.

### Características principales de NodeJS:

- **Concurrencia**: Es monohilo, con entradas y salidas asíncronas.

- **Motor V8**: Creado por Google en 2008 para Chrome. Escrito en C++. Convierte JS en código máquina en lugar de interpretarlo en tiempo real.

- **Módulos** : Son piezas de código muy pequeñas que modularizan nuestros sistemas y ayudan a entender mejor el código.

- **Orientación a Eventos**, existe un bucle de eventos que se ejecuta constantemente. Lo que nos permite programar de forma reactiva, lo que quiere decir que podemos programar con la lógica de “Cuando sucede algo, se ejecuta esta parte de mi código y eso a su vez dispara otra parte”.



[Ryan Dahl]: https://en.wikipedia.org/wiki/Ryan_Dahl


## EventLoop: 🔁  
  
![alt text](https://i.ibb.co/j42621b/event-loop.png "EventLoop")    
El bucle de eventos es lo que permite a Node.js realizar operaciones de E / S sin bloqueo, a pesar de que JavaScript es de un solo subproceso, descargando operaciones al kernel del sistema siempre que sea posible.

Dado que la mayoría de los núcleos modernos son multiproceso, pueden manejar múltiples operaciones que se ejecutan en segundo plano. Cuando se completa una de estas operaciones, el kernel le dice a Node.js para que se pueda agregar la devolución de llamada adecuada a la cola de sondeo para que finalmente se ejecute.  
## Bucle de eventos explicado
Cuando Node.js se inicia, inicializa el bucle de eventos, procesa el script de entrada proporcionado (o cae en el REPL , que no se trata en este documento) que puede realizar llamadas API asíncronas, programar temporizadores o llamar ***process.nextTick()***, luego comienza a procesar el **"event loop"**.
En el siguiente diagrama se muestra una descripción general simplificada del orden de operaciones del bucle de eventos.

```c
   ┌───────────────────────────┐
┌─>│           timers          │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     pending callbacks     │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │       idle, prepare       │
│  └─────────────┬─────────────┘      ┌───────────────┐
│  ┌─────────────┴─────────────┐      │   incoming:   │
│  │           poll            │<─────┤  connections, │
│  └─────────────┬─────────────┘      │   data, etc.  │
│  ┌─────────────┴─────────────┐      └───────────────┘
│  │           check           │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
└──┤      close callbacks      │
   └───────────────────────────┘
```  
***Cada cuadro se denominará una "fase" del bucle de eventos.***   

Cada fase tiene una cola FIFO de devoluciones de llamada para ejecutar. Si bien cada fase es especial a su manera, generalmente, cuando el bucle de eventos ingresa a una fase determinada, realizará cualquier operación específica para esa fase, luego ejecutará devoluciones de llamada en la cola de esa fase hasta que la cola se haya agotado o el número máximo de devoluciones de llamada. ha ejecutado. Cuando se agote la cola o se alcance el límite de devolución de llamada, el bucle de eventos pasará a la siguiente fase, y así sucesivamente.  
### Resumen de las fases  
- **timers** : esta fase ejecuta devoluciones de llamada programadas por setTimeout()y setInterval().
- **pending callbacks** : ejecuta devoluciones de llamada de E / S diferidas hasta la siguiente iteración del ciclo.
- **idle, prepare** : solo se usa internamente.
- **poll** : recupera nuevos eventos de E / S; ejecutar devoluciones de llamada relacionadas con E / S (casi todas con la excepción de devoluciones de llamada de cierre, las programadas por temporizadores y setImmediate()); El nodo se bloqueará aquí cuando sea apropiado.
- **check** : las setImmediate()devoluciones de llamada se invocan aquí.
- **close callbacks**: algunas devoluciones de llamada cercanas, por ejemplo socket.on('close', ...).

Entre cada ejecución del bucle de eventos, Node.js comprueba si está esperando alguna ***E/S*** asincrónica o timers y se apaga limpiamente si no hay ninguno

