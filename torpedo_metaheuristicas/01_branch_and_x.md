# 01 - Branch and X

## Metodo exacto

Branch and X pertenece a los algoritmos exactos de optimizacion. Su objetivo no
es solamente encontrar una solucion buena, sino encontrar el optimo global y
justificar que ninguna otra solucion factible es mejor.

Por eso no se considera una metaheuristica. Una metaheuristica acepta perder la
garantia de optimalidad para buscar soluciones de buena calidad en menos tiempo.
Branch and X mantiene una logica distinta: explora el espacio de busqueda con
una garantia matematica, aunque en algunos problemas eso pueda ser caro.

La idea no es revisar todas las soluciones una por una. La idea es organizar la
busqueda en subproblemas y descartar grupos completos de soluciones cuando se
puede demostrar que no pueden mejorar la mejor solucion conocida.

## Modelo de optimizacion

Un problema de optimizacion se construye con variables de decision, funcion
objetivo y restricciones.

Las variables de decision son lo que el algoritmo debe escoger. En un problema
binario pueden valer 0 o 1; en uno entero pueden tomar valores discretos; en uno
continuo pueden tomar valores reales.

La funcion objetivo mide la calidad de una solucion. Si el problema es de
minimizacion, valores menores son mejores. Si es de maximizacion, valores
mayores son mejores.

Las restricciones definen que soluciones son factibles. Una solucion con buen
valor objetivo no sirve si rompe alguna condicion del problema.

Branch and X trabaja especialmente bien cuando el problema puede separarse en
decisiones parciales. Cada decision parcial reduce el conjunto de soluciones que
todavia tiene sentido considerar.

## Arbol de busqueda

Branch and X representa la busqueda como un arbol.

La raiz contiene el problema completo. Cada nodo representa un subproblema, es
decir, una version del problema donde algunas decisiones ya fueron fijadas.

Al bajar por el arbol, el algoritmo agrega decisiones. Eso hace que cada nodo
represente un conjunto mas pequeno de soluciones posibles.

Esta estructura permite razonar por grupos: si un nodo no puede producir una
mejor solucion, entonces tampoco pueden hacerlo sus hijos. En ese caso se poda
toda la rama.

## Branching

Branching es la regla que divide un nodo en subproblemas hijos.

En variables binarias, una division comun es fijar una variable en 0 y en 1. En
variables enteras, una division comun es separar una variable segun el piso y el
techo de un valor fraccionario obtenido desde una relajacion.

Lo importante es que el branching no pierda soluciones factibles relevantes. Los
hijos deben cubrir las posibilidades del nodo padre, porque si una solucion
optima desaparece por la division, el metodo deja de ser exacto.

Una buena regla de branching puede reducir mucho el numero de nodos explorados.
No cambia el peor caso teorico, pero si puede cambiar fuertemente el rendimiento
practico.

## Bounding

Bounding es el calculo de una cota optimista para un subproblema.

En minimizacion, la cota es inferior: indica el mejor valor minimo que ese nodo
podria llegar a alcanzar. En maximizacion, la cota es superior: indica el mejor
valor maximo que ese nodo podria alcanzar.

La cota suele venir de una relajacion. Una relajacion es una version mas facil
del problema, por ejemplo permitir valores fraccionarios donde antes solo habia
variables enteras.

La relajacion puede entregar una solucion que no sea factible para el problema
original, pero sirve para estimar que tan prometedor es un nodo.

Mientras mas ajustada sea la cota, mas facil es podar. Una cota muy debil obliga
al algoritmo a explorar mas nodos.

## Incumbente

El incumbente es la mejor solucion factible encontrada hasta el momento.

En minimizacion, es la solucion con menor valor conocida. En maximizacion, es la
solucion con mayor valor conocida.

El incumbente es importante porque transforma una solucion encontrada en un
criterio de descarte. Si la cota de un nodo muestra que ese subproblema no puede
mejorar al incumbente, entonces no vale la pena seguir explorandolo.

Por eso encontrar un buen incumbente temprano ayuda mucho: mientras mejor sea la
referencia, mas ramas se pueden podar.

## Pruning

Pruning significa podar un nodo del arbol.

Un nodo se puede podar si es infactible, si su cota no puede mejorar al
incumbente o si ya representa una solucion completa que no necesita seguir
dividiendose.

La poda es la diferencia entre una busqueda inteligente y una enumeracion bruta.
Branch and X sigue pudiendo ser exponencial en el peor caso, pero en la practica
su rendimiento depende de cuantas ramas logra descartar antes de abrirlas.

## Estructura en Python

La forma tecnica de entender Branch and Bound es verlo como una funcion general
que no conoce el problema especifico. El algoritmo recibe funciones externas que
definen como se comporta el problema.

En el notebook, esa estructura se implementa con estas piezas:

- `root`: nodo inicial.
- `branch(node)`: genera los hijos de un nodo.
- `bound(node)`: calcula la cota optimista del nodo.
- `is_feasible(node)`: indica si el nodo todavia puede contener soluciones validas.
- `is_complete(node)`: indica si el nodo ya representa una solucion completa.
- `objective(node)`: calcula el valor real de una solucion completa.
- `incumbent`: mejor solucion factible encontrada hasta ahora.

Esta separacion es importante: Branch and Bound no cambia de idea cuando cambia
el problema. Lo que cambia son las funciones que definen la representacion, la
cota y la forma de ramificar.

## Lectura del codigo

El codigo implementa una estructura general de Branch and Bound.

La clase `BranchAndBoundResult` solo ordena la salida del algoritmo: guarda la
mejor solucion, su valor, cuantos nodos se exploraron y cuantas podas ocurrieron.

La funcion `branch_and_bound` recibe un nodo raiz y varias funciones del
problema. Esto permite que el mismo algoritmo sirva para distintos modelos. Si
el problema cambia, no se reescribe Branch and Bound completo; se cambian las
funciones que describen el problema.

La variable `best_solution` representa el incumbente. `best_value` guarda su
valor objetivo. Si el problema es de minimizacion, el algoritmo busca disminuir
ese valor; si es de maximizacion, usa un cambio de signo para mantener la misma
logica interna.

La lista `pending` guarda los nodos que todavia pueden explorarse. Se maneja con
una cola de prioridad, de modo que el algoritmo puede revisar primero los nodos
mas prometedores segun su cota.

En cada iteracion se extrae un nodo, se revisa si es factible, se compara su cota
con el incumbente y se decide si conviene podarlo. Si el nodo ya es una solucion
completa, se evalua con `objective`. Si mejora al incumbente, se actualiza la
mejor solucion.

Si el nodo no se poda y todavia no es completo, se generan sus hijos con
`branch(node)`. Cada hijo vuelve a pasar por la misma logica: factibilidad, cota,
poda o exploracion.

La idea importante es que el codigo no enumera soluciones a ciegas. Cada nodo se
abre solo si todavia tiene posibilidad de mejorar lo mejor encontrado hasta el
momento.

## Branch and X

La letra X indica que la estructura base de ramificar y podar puede combinarse
con distintas tecnicas.

Branch and Bound usa cotas para decidir que ramas descartar.

Branch and Cut agrega cortes, que son restricciones validas para fortalecer la
relajacion. Un corte elimina soluciones artificiales de la relajacion, pero no
elimina soluciones factibles del problema original.

Branch and Price genera variables o columnas durante la busqueda. Esto se usa
cuando el modelo completo tendria demasiadas variables para construirlas todas
desde el inicio.

Las variantes cambian la herramienta matematica, pero mantienen la misma idea
base: dividir el problema y usar informacion optimista para evitar explorar
partes inutiles del arbol.

## Complejidad

Branch and X puede tener complejidad exponencial en el peor caso. Si las cotas
son debiles o el branching es malo, el arbol puede crecer demasiado y acercarse a
una enumeracion completa.

En la practica, el rendimiento depende de cuatro factores: que tan buena es la
cota, que tan rapido aparece un buen incumbente, que variable se elige para
ramificar y en que orden se exploran los nodos.

Por eso no basta decir que el metodo es exacto. Tambien hay que entender que la
eficiencia depende de la calidad de la informacion usada para podar.

## Cuando se usa

Branch and X se usa cuando la garantia de optimalidad es importante o cuando el
problema tiene una estructura que permite buenas cotas.

Es comun en optimizacion combinatoria y programacion entera: asignacion,
scheduling, rutas, seleccion de proyectos, diseno de redes y problemas con
decisiones discretas.

Cuando el problema es muy grande y no se necesita una prueba de optimalidad, una
heuristica o metaheuristica puede ser mas conveniente. Por eso este metodo sirve
como contraste: muestra lo que se gana y lo que se paga al exigir garantia.

## Resumen

Branch and X es una familia de algoritmos exactos. Su objetivo es encontrar el
optimo global y demostrar que lo encontro.

Su estructura se entiende con tres acciones: ramificar el problema, calcular
cotas y podar ramas que no pueden mejorar al incumbente.

No es una metaheuristica porque no se conforma con una solucion buena sin
prueba. Su fortaleza es la garantia; su debilidad es que puede volverse costoso
cuando el arbol de busqueda crece demasiado.
