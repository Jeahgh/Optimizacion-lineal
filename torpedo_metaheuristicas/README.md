# Torpedo de Metaheuristicas

Este material es para estudiar, no para memorizar sin entender.

La idea es construir un torpedo defendible: si el profesor pregunta "que hace
esto?", "por que sirve?", "que cambia entre una metaheuristica y otra?" o "como
lo adaptas a otro problema?", la respuesta tiene que salir de aca.

## Regla principal del torpedo

No estudiar cada algoritmo como si solo existiera para el TSP.

Cada metodo debe quedar explicado en version generica:

- que tipo de problema intenta resolver;
- que representa una solucion;
- que funcion objetivo se quiere minimizar o maximizar;
- como se generan soluciones nuevas;
- como decide si acepta, rechaza o mezcla soluciones;
- que parametros controlan su comportamiento;
- que equilibrio tiene entre exploracion y explotacion;
- que ventajas, riesgos y preguntas tipicas puede hacer el profesor.

## Estructura propuesta

1. `00_contexto_metaheuristicas.md`
   Entrada conceptual: que es optimizacion, diferencia entre metodos exactos,
   heuristicas y metaheuristicas, y como leer todos los algoritmos con el mismo
   lenguaje.

2. `01_branch_and_x.md`
   Metodo exacto. No es metaheuristica, pero sirve como contraste: busca optimo
   garantizado usando branching, bounding y pruning.

3. `02_tabu_search.md`
   Metaheuristica de una sola solucion. Local search con memoria para evitar
   ciclos.

4. `03_ant_colony.md`
   Metaheuristica poblacional. Muchas hormigas construyen soluciones usando
   feromonas y heuristica local.

5. `04_genetic_algorithm.md`
   Metaheuristica poblacional. Una poblacion evoluciona por seleccion,
   crossover, mutacion y elitismo.

6. `05_simulated_annealing.md`
   Metaheuristica de una sola solucion. Acepta movimientos peores al principio
   para escapar de optimos locales y luego se vuelve mas exigente.

7. `99_preguntas_orales.md`
   Preguntas probables de interrogacion con respuestas cortas, claras y
   defendibles.

## Criterio estricto

Si una explicacion solo funciona para "ciudades y rutas", todavia esta floja.
Tiene que poder decirse tambien para mochila, scheduling, asignacion,
parametros continuos u otro problema de optimizacion.

