# 05 - Simulated Annealing

## Idea general

Simulated Annealing, o Recocido Simulado, es una metaheuristica de una sola
solucion.

El algoritmo parte con una solucion inicial y propone cambios pequenos para
moverse por el espacio de busqueda. A diferencia de una busqueda greedy, no
acepta solamente mejoras. Tambien puede aceptar soluciones peores con cierta
probabilidad.

Esa probabilidad depende de una variable llamada temperatura. Al inicio, la
temperatura es alta y el algoritmo explora con mas libertad. Con el tiempo, la
temperatura baja y el algoritmo se vuelve mas estricto.

La idea central es explorar bastante al comienzo y explotar mas al final.

## Energia y objetivo

La analogia viene de la fisica. En un material caliente, las particulas se
mueven con libertad. Si se enfria lentamente, puede llegar a una configuracion de
baja energia.

En optimizacion, la energia se interpreta como el valor de la funcion objetivo.
En un problema de minimizacion, menor energia significa mejor solucion.

No es necesario quedarse con la analogia fisica. Lo importante es la estructura:
una solucion actual, un vecino propuesto, una diferencia de calidad y una regla
para aceptar o rechazar el movimiento.

## Vecindario

Simulated Annealing necesita una forma de generar una solucion vecina.

El vecindario depende del problema. En TSP puede ser invertir un segmento de una
ruta. En mochila puede ser agregar o quitar un item. En optimizacion continua
puede ser sumar un pequeno ruido a las variables.

La calidad del vecindario es importante. Si los cambios son demasiado pequenos,
el algoritmo puede moverse lento. Si son demasiado grandes, puede saltar sin
aprovechar informacion local.

## Temperatura

La temperatura controla que tan dispuesto esta el algoritmo a aceptar soluciones
peores.

Con temperatura alta, aceptar un empeoramiento es mas probable. Esto ayuda a
explorar y escapar de optimos locales.

Con temperatura baja, aceptar un empeoramiento se vuelve poco probable. El
algoritmo se parece mas a una busqueda local greedy.

Por eso la temperatura conecta exploracion y explotacion. Al inicio domina la
exploracion. Al final domina la explotacion.

## Regla de aceptacion

Cuando el vecino mejora la solucion actual, se acepta.

Cuando el vecino empeora la solucion actual, no se descarta automaticamente. Se
acepta con una probabilidad que depende de cuanto empeora y de la temperatura.

Si el empeoramiento es pequeno, es mas facil aceptarlo. Si el empeoramiento es
grande, es mas dificil. Si la temperatura es alta, el algoritmo tolera mas
empeoramientos. Si la temperatura es baja, tolera menos.

Esta regla permite salir de optimos locales sin usar memoria tabu. Tabu Search
escapa usando restricciones temporales sobre movimientos recientes. Simulated
Annealing escapa usando probabilidad controlada por temperatura.

## Enfriamiento

El enfriamiento define como baja la temperatura.

Una regla comun es el enfriamiento geometrico: en cada iteracion, la temperatura
se multiplica por un factor menor que uno.

Si el enfriamiento es muy rapido, el algoritmo se vuelve greedy demasiado pronto
y puede quedar atrapado. Si es muy lento, puede explorar mejor, pero demora mas.

La eleccion del esquema de enfriamiento es uno de los puntos mas importantes del
metodo.

## Estructura en Python

La forma tecnica de implementar Simulated Annealing es separar el motor del
problema especifico.

En el notebook, la estructura se implementa con estas piezas:

- `initial`: solucion inicial.
- `objective(solution)`: mide la calidad de una solucion.
- `neighbor(solution, rng)`: propone una solucion vecina.
- `initial_temperature`: temperatura inicial.
- `cooling_rate`: factor de enfriamiento.
- `iterations`: cantidad de iteraciones.
- `sense`: indica si el problema es de minimizacion o maximizacion.

El algoritmo no necesita saber que representa la solucion. Solo necesita poder
evaluarla y generar vecinos.

## Lectura del codigo

El codigo implementa un motor general de Simulated Annealing.

La clase `SimulatedAnnealingResult` ordena la salida del algoritmo. Guarda la
mejor solucion encontrada, su valor, el historial de mejores valores, el
historial de valores actuales, las temperaturas y estadisticas de aceptacion.

La funcion `simulated_annealing` recibe una solucion inicial, una funcion
objetivo y una funcion para generar vecinos. Esto hace que el mismo motor pueda
usarse en distintos problemas.

La variable `current` representa la solucion actual. La variable `best_solution`
guarda la mejor solucion encontrada durante toda la busqueda.

En cada iteracion se genera un candidato usando `neighbor`. Luego se calcula la
diferencia entre el candidato y la solucion actual.

Si el candidato mejora, se acepta. Si empeora, el algoritmo calcula una
probabilidad de aceptacion. Esa probabilidad disminuye cuando el empeoramiento
es grande o cuando la temperatura es baja.

Despues de decidir si acepta o rechaza el candidato, el algoritmo actualiza la
mejor solucion si corresponde y luego enfria la temperatura.

La idea importante es que el algoritmo no evita todos los malos movimientos. Los
permite al comienzo para explorar, pero los acepta cada vez menos a medida que la
temperatura baja.

## Ejemplo 1 - TSP

En TSP, Simulated Annealing parte desde una ruta y propone vecinos con movimientos 2-opt.

Al comienzo puede aceptar rutas peores porque la temperatura es alta. Al final se vuelve mas estricto y se parece mas a una busqueda local.

## Lectura del ejemplo TSP

El objetivo es minimizar distancia. El vecino se genera invirtiendo un segmento de la ruta.

Este ejemplo permite ver la diferencia con Tabu: aqui no hay memoria tabu. El escape de optimos locales se logra aceptando empeoramientos con probabilidad controlada por temperatura.

## Ejemplo 2 - Rastrigin

La funcion Rastrigin es un ejemplo continuo con muchos optimos locales. Por eso es buena para Simulated Annealing.

La solucion es un vector de numeros reales y el vecino se genera agregando un pequeno ruido aleatorio.

## Lectura del ejemplo Rastrigin

El minimo global esta cerca de cero, pero la superficie tiene muchos valles locales. Una busqueda greedy puede quedar atrapada rapidamente.

Simulated Annealing puede aceptar movimientos peores al inicio, lo que le permite saltar entre valles antes de enfriarse.

## Parametros

Los parametros principales son la temperatura inicial, la tasa de enfriamiento,
el numero de iteraciones y el vecindario.

Una temperatura inicial muy baja hace que el algoritmo sea casi greedy desde el
principio. Una temperatura demasiado alta puede aceptar demasiados movimientos
malos.

Una tasa de enfriamiento cercana a uno enfria lento y permite mas exploracion.
Una tasa mas baja enfria rapido y reduce el tiempo, pero puede afectar la
calidad.

El vecindario tambien es decisivo, porque define que caminos puede recorrer el
algoritmo dentro del espacio de busqueda.

## Complejidad

El costo total depende del numero de iteraciones y del costo de evaluar cada
vecino.

Simulated Annealing suele usar poca memoria, porque mantiene principalmente una
solucion actual, la mejor solucion y algunos historiales.

Su costo por iteracion puede ser bajo si generar y evaluar vecinos es barato.
Por eso es una metaheuristica simple y flexible, aunque puede necesitar muchas
iteraciones para obtener buenas soluciones.

## Cuando se usa

Simulated Annealing se usa cuando existe una forma natural de generar vecinos y
cuando el problema tiene optimos locales que pueden atrapar una busqueda greedy.

Es util en rutas, scheduling, asignacion, diseno, seleccion de configuraciones y
optimizacion continua.

Funciona especialmente bien cuando aceptar empeoramientos temporales ayuda a
salir de zonas malas del espacio de busqueda.

## Resumen

Simulated Annealing es una metaheuristica de una sola solucion que usa
temperatura para controlar la aceptacion de movimientos peores.

Al comienzo explora mas porque la temperatura es alta. Al final explota mas
porque la temperatura baja y el algoritmo se vuelve mas exigente.

No garantiza el optimo global en la practica, pero puede escapar de optimos
locales de forma simple. Su rendimiento depende mucho del vecindario, la
temperatura inicial, el enfriamiento y el numero de iteraciones.

