# 04 - Genetic Algorithm

## Idea general

Un algoritmo genetico es una metaheuristica poblacional inspirada en la evolucion
natural.

La idea central es mantener un conjunto de soluciones candidatas, llamado
poblacion, y hacerlo evolucionar durante varias generaciones. Las soluciones con
mejor desempeno tienen mas probabilidad de influir en la siguiente generacion,
pero tambien se introduce variacion para seguir explorando.

En este metodo, una sola solucion no recorre el espacio por si misma. La busqueda
se reparte entre muchos individuos. Eso permite explorar varias zonas al mismo
tiempo.

Como toda metaheuristica, no garantiza encontrar el optimo global. Busca buenas
soluciones combinando seleccion, recombinacion y cambio aleatorio controlado.

## Individuo y poblacion

Un individuo es una solucion candidata del problema.

La poblacion es el conjunto de individuos que el algoritmo mantiene en una
generacion.

La forma de representar un individuo se llama codificacion. En TSP puede ser una
permutacion de ciudades. En mochila puede ser un vector binario. En optimizacion
continua puede ser un vector de numeros reales.

La codificacion es una decision importante, porque define que operadores tienen
sentido. No se cruza igual una permutacion que un vector binario o uno continuo.

## Fitness

El fitness mide que tan bueno es un individuo.

En un algoritmo genetico normalmente se habla de maximizar fitness. Si el
problema original es de minimizacion, se transforma el objetivo para que una
mejor solucion tenga mayor fitness. Por ejemplo, si se minimiza costo, se puede
usar fitness negativo del costo.

El fitness no es solo una medida de calidad. Tambien guia la seleccion: los
individuos con mejor fitness tienen mas probabilidad de reproducirse o pasar
informacion a la siguiente generacion.

## Seleccion

La seleccion decide que individuos se usan como padres.

El objetivo es dar mas oportunidades a las soluciones buenas, sin eliminar por
completo la diversidad. Si la seleccion es demasiado fuerte, la poblacion puede
converger muy rapido a una zona mediocre. Si es demasiado debil, el algoritmo
avanza sin direccion.

Una seleccion comun es torneo. Se eligen algunos individuos al azar y gana el de
mejor fitness dentro de ese grupo.

La seleccion produce presion evolutiva: empuja la poblacion hacia soluciones mas
prometedoras.

## Crossover

El crossover, o cruce, combina informacion de dos padres para generar nuevos
hijos.

La idea es que dos soluciones buenas pueden contener partes utiles, y que al
recombinarlas podria aparecer una solucion mejor.

El tipo de crossover depende de la codificacion. En vectores binarios se pueden
intercambiar segmentos. En variables reales se pueden mezclar valores. En
permutaciones, como rutas, se necesitan cruces especiales para no repetir ni
perder elementos.

Crossover es principalmente una herramienta de explotacion: usa informacion de
soluciones buenas que ya existen.

## Mutacion

La mutacion introduce cambios aleatorios pequenos en un individuo.

Su funcion principal es mantener diversidad y evitar que toda la poblacion se
vuelva demasiado parecida.

En un vector binario, mutar puede ser cambiar un 0 por 1. En una permutacion,
puede ser intercambiar dos posiciones o invertir un segmento. En variables
continuas, puede ser sumar ruido.

Mutacion es principalmente una herramienta de exploracion: permite visitar zonas
que no aparecen solo combinando padres.

## Elitismo

El elitismo copia algunos de los mejores individuos directamente a la siguiente
generacion.

Esto evita perder la mejor solucion encontrada por culpa de un cruce o una
mutacion desfavorable.

Un poco de elitismo suele ser util. Demasiado elitismo puede reducir diversidad
y hacer que la poblacion se estanque.

## Estructura en Python

La forma tecnica de implementar un algoritmo genetico general es separar el motor
evolutivo del problema especifico.

En el notebook, la estructura se implementa con estas piezas:

- `initial_population`: poblacion inicial.
- `fitness(individual)`: mide la calidad del individuo.
- `crossover(parent1, parent2, rng)`: genera hijos combinando padres.
- `mutate(individual, rng)`: modifica un individuo.
- `generations`: cantidad de generaciones.
- `elite_size`: numero de mejores individuos que pasan directo.
- `crossover_rate`: probabilidad de aplicar cruce.
- `tournament_size`: intensidad de la seleccion por torneo.

El motor genetico no necesita saber si el individuo representa una ruta, una
mochila o un vector real. Eso queda definido por la codificacion y los
operadores que se le entregan.

## Lectura del codigo

El codigo implementa un algoritmo genetico general que maximiza fitness.

La clase `GeneticAlgorithmResult` guarda la mejor solucion encontrada, su
fitness, el historial de mejora, la poblacion final y el numero de generaciones.

La funcion `tournament_selection` realiza la seleccion por torneo. Toma algunos
individuos al azar y devuelve una copia del que tiene mejor fitness dentro de
ese grupo.

La funcion `genetic_algorithm` recibe una poblacion inicial y las funciones que
definen el problema: fitness, crossover y mutacion. Con eso puede trabajar sobre
distintas codificaciones.

En cada generacion, primero se evalua el fitness de toda la poblacion. Luego se
copian los mejores individuos como elites.

Despues se crean hijos hasta completar la nueva poblacion. Para eso se eligen
padres por torneo, se aplica crossover con cierta probabilidad y finalmente se
aplica mutacion.

Al terminar la generacion, la nueva poblacion reemplaza a la anterior. Si aparece
un individuo con mejor fitness que el mejor conocido, se actualiza el
incumbente.

La idea importante es que el algoritmo no mejora una solucion aislada, sino que
hace evolucionar una poblacion completa.

## Parametros

Los parametros principales son tamano de poblacion, generaciones, tasa de cruce,
tasa de mutacion, tamano del torneo y elitismo.

Una poblacion mas grande explora mas, pero cuesta mas evaluar. Mas generaciones
dan mas tiempo para mejorar, pero aumentan el costo total.

Una tasa de cruce alta favorece recombinacion. Una tasa de mutacion alta aumenta
exploracion, pero si es excesiva puede convertir la busqueda en algo casi
aleatorio.

El tamano del torneo controla la presion selectiva. Torneos grandes hacen que
ganen casi siempre los mejores, pero pueden reducir diversidad demasiado rapido.

## Complejidad

El costo principal esta en evaluar la poblacion.

De forma general, el costo depende del tamano de la poblacion, el numero de
generaciones y el costo de calcular el fitness de cada individuo.

Si el fitness es caro, el algoritmo genetico tambien lo sera. Por eso muchas
implementaciones intentan paralelizar evaluaciones o reducir calculos repetidos.

## Cuando se usa

Los algoritmos geneticos se usan cuando existen muchas soluciones posibles y se
puede definir una buena codificacion junto con operadores de cruce y mutacion.

Son utiles en problemas combinatorios, seleccion de subconjuntos, rutas,
calibracion de parametros, diseno y optimizacion continua.

Funcionan especialmente bien cuando combinar partes de buenas soluciones puede
generar soluciones aun mejores.

## Resumen

Un algoritmo genetico es una metaheuristica poblacional que hace evolucionar una
poblacion de soluciones.

Sus ideas centrales son fitness, seleccion, crossover, mutacion y elitismo.

La seleccion y el crossover explotan soluciones prometedoras. La mutacion
mantiene exploracion y diversidad. El elitismo protege las mejores soluciones.

Su fortaleza es la flexibilidad: puede adaptarse a muchos problemas cambiando la
codificacion y los operadores. Su debilidad es que depende mucho de parametros y
puede converger prematuramente si pierde diversidad.

