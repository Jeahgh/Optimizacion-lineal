# 00 - Metaheuristicas

## Optimizacion

Optimizar es elegir la mejor solucion posible segun un criterio medible. Ese
criterio se llama funcion objetivo y permite comparar soluciones de manera
ordenada: una solucion no es "buena" solo porque parece conveniente, sino porque
obtiene un mejor valor que otras alternativas.

La funcion objetivo puede representar distancia, costo, tiempo, ganancia,
energia, error u otra medida del problema. Si el objetivo es minimizar, se busca
el valor mas bajo. Si el objetivo es maximizar, se busca el valor mas alto.

Lo importante es que el problema queda expresado como una busqueda entre
alternativas posibles.

Ejemplos:

- En rutas, una solucion puede ser un orden de visita y el objetivo puede ser
  minimizar la distancia total.
- En mochila, una solucion puede ser elegir objetos y el objetivo puede ser
  maximizar el valor sin superar la capacidad.
- En planificacion, una solucion puede ser ordenar tareas y el objetivo puede
  minimizar atrasos o tiempo total.

## Espacio de busqueda

El espacio de busqueda es el conjunto de todas las soluciones posibles.

El problema aparece cuando ese conjunto es demasiado grande. Aunque evaluar una
solucion individual sea facil, evaluar todas puede ser impracticable.

Por eso los metodos de optimizacion no solo necesitan evaluar soluciones:
necesitan una forma inteligente de recorrer el espacio de busqueda sin revisar
todo de manera exhaustiva.

## Metodos exactos

Los metodos exactos buscan encontrar el optimo global y demostrar que no existe
una solucion mejor.

Su principal ventaja es la garantia de optimalidad. Cuando terminan
correctamente, entregan una solucion optima y una justificacion de que ninguna
otra solucion factible la supera.

Su desventaja es el costo computacional. En problemas grandes pueden tardar
demasiado, porque la cantidad de soluciones crece muy rapido. Por eso suelen ser
mas adecuados para instancias pequenas, problemas muy estructurados o casos
donde la garantia es mas importante que el tiempo.

Ejemplos:

- Branch and Bound.
- Branch and Cut.
- Branch and Price.
- Programacion dinamica.
- Constraint Programming.

La idea clave es que un metodo exacto privilegia la garantia. Esa garantia es
valiosa, pero puede volverse costosa cuando el espacio de busqueda crece.

## Metodos aproximados

Los metodos aproximados buscan soluciones buenas en un tiempo razonable.

No prometen encontrar el optimo global. A cambio, pueden trabajar mejor en
problemas grandes o dificiles, donde un metodo exacto seria muy lento o
directamente impracticable.

Esta idea no significa buscar "al azar" ni conformarse con cualquier solucion.
Significa aceptar que, en ciertos problemas, una solucion de alta calidad puede
ser mas util que una solucion optima que llega demasiado tarde.

## Heuristicas

Una heuristica es una regla practica para encontrar una solucion aceptable.

Normalmente esta muy ligada a un problema especifico. Por ejemplo, en un
problema de rutas, una regla podria ser ir siempre a la ciudad mas cercana que
todavia no se ha visitado.

La heuristica suele ser simple y rapida, pero no necesariamente general. Puede
funcionar bien para un caso y no tener sentido en otro.

Esta diferencia prepara la idea de metaheuristica: pasar de una regla puntual a
una estrategia mas general de busqueda.

## Metaheuristicas

Una metaheuristica es un algoritmo de optimizacion aproximado que guia la
busqueda de buenas soluciones en problemas dificiles, especialmente cuando el
espacio de busqueda es grande, irregular o lleno de optimos locales.

A diferencia de una heuristica especifica, una metaheuristica no depende de un
unico problema. Funciona como un marco general que se adapta definiendo como se
representa una solucion, como se evalua y como se generan nuevas alternativas.

Se puede aplicar a rutas, mochila, planificacion, asignacion, parametros
continuos u otros casos. La logica general se mantiene, pero la representacion y
los operadores cambian segun el problema.

Una metaheuristica no garantiza encontrar el optimo global. Su proposito es
organizar la busqueda para encontrar soluciones de buena calidad sin recorrer
todo el espacio de soluciones.

En una frase: una metaheuristica es una estrategia general de optimizacion que
equilibra exploracion y explotacion para buscar buenas soluciones en problemas
donde revisar todas las posibilidades es demasiado costoso.

## Componentes

Para aplicar una metaheuristica hay que definir:

| Componente | Pregunta que responde |
|---|---|
| Representacion | Como se escribe una solucion? |
| Funcion objetivo | Como se mide si una solucion es buena o mala? |
| Vecindario u operadores | Como se generan soluciones nuevas? |
| Criterio de decision | Como se acepta, rechaza o combina una solucion? |
| Parametros | Que valores controlan el comportamiento del metodo? |
| Restricciones | Que condiciones debe cumplir una solucion valida? |

Si uno de estos componentes no esta claro, el algoritmo queda dificil de
defender. Por eso no basta con decir "aplique Tabu" o "use un genetico"; hay que
explicar que significa una solucion, como se mide su calidad y que movimientos
permiten recorrer el espacio de busqueda.

## Optimo local y optimo global

Un optimo global es la mejor solucion de todo el espacio de busqueda.

Un optimo local es una solucion que parece la mejor si solo se compara con sus
vecinas, pero puede existir una solucion mejor en otra zona del espacio.

Esta diferencia es central. Muchas metaheuristicas existen porque una busqueda
demasiado local puede quedarse atrapada en un optimo local.

## Exploracion y explotacion

La explotacion consiste en aprovechar una zona que ya parece prometedora.

La exploracion consiste en probar zonas nuevas para no quedar atrapado demasiado
pronto.

Una buena metaheuristica equilibra ambas ideas. Si explota demasiado, puede
quedarse en un optimo local. Si explora demasiado, puede avanzar sin direccion.

## Metodos que se estudiaran

Con estas ideas ya se puede ubicar cada metodo del curso. Algunos trabajan con
una sola solucion actual, otros con una poblacion de soluciones, y Branch and X
queda aparte porque es exacto.

| Metodo | Tipo | Idea principal |
|---|---|---|
| Branch and X | Exacto | Divide el problema y descarta ramas que no pueden mejorar. |
| Tabu Search | Metaheuristica de una solucion | Usa memoria para no volver inmediatamente a movimientos recientes. |
| Ant Colony Optimization | Metaheuristica poblacional | Construye soluciones guiadas por feromonas e informacion heuristica. |
| Genetic Algorithm | Metaheuristica poblacional | Evoluciona una poblacion usando seleccion, cruce y mutacion. |
| Simulated Annealing | Metaheuristica de una solucion | Acepta algunos movimientos peores al inicio y se vuelve mas estricto al enfriarse. |

Branch and X no es una metaheuristica. Se estudia porque sirve como contraste:
busca garantia de optimalidad, mientras que las metaheuristicas buscan buenas
soluciones sin prometer optimalidad.

## Resumen

Una metaheuristica sirve cuando el espacio de busqueda es grande y no conviene
probar todas las soluciones. Para usarla hay que definir como representar una
solucion, como evaluarla y como generar nuevas alternativas.

La idea central es buscar bien sin revisar todo. Cada metodo lo hace de una
forma distinta: memoria, temperatura, poblacion, feromonas o evolucion.
