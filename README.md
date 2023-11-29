# Práctica II: Búsqueda heurística sin adversarios.


## Manual de usuario

1. **Clonar el Repositorio**
   - Abre tu terminal y ejecuta el siguiente comando:
   
     ```
     git clone https://github.com/Dulibeth/IA-practica2.git
     ```
2. **Ejecutar**
    - Una vez dentro del proyecto, ingresa el siguiente comando en tu terminal.
    
      ```
        ant run_main
      ```
3. **Resultado**
    - Dado un grafo, el programa lo analizará utilizando el algoritmo A* y arrojará por terminal el camino mas óptimo.
    
      ```
      run_main:
      [java] [ 1(0) ] -> [ 3(0) ] = 9
      [java]
      [java] [ 3(0) ] -> [ 4(0) ] = 11
      [java]

      BUILD SUCCESSFULL
       
      ```
    
## Preguntas

1. ¿Qué variable representa la lista **ABIERTA**?
    
    En el código, la variable **"openSet"** actúa como la lista **ABIERTA**, ya que es donde se almacenan los nodos pendientes de evaluación. La lista **openSet** se actualiza a lo largo del algoritmo, agregando nuevos nodos descubiertos, eliminando nodos evaluados y reordenando la lista.

    ```
     final List<Graph.Vertex<T>> openSet = new ArrayList<Graph.Vertex<T>>(size);
    ```
2. ¿Qué variable representa la función **g**?

     La variable que representa la función **g** es **"gScore"**, esta mantiene un seguimiento de los costos acumulados desde el nodo inicial hasta cada nodo en el grafo que estamos evaluando. Se actualiza a medida que el algoritmo avanza.

    ```
    final Map<Graph.Vertex<T>,Integer> gScore = new HashMap<Graph.Vertex<T>,Integer>();
    ```
3. ¿Qué variable representa la función **f** ?  

    La variable que representa la funcion **f** es **"fScore"**, debido a que combina el costo acumulado desde el inicio hasta un nodo específico ( **gScore**) y la estimación heurística del costo restante hasta el objetivo. En el código, **fScore** se calcula y actualiza para cada nodo evaluado, de esta manera ayuda a elegir los mejores nodos para seguir buscando la mejor ruta.

    ```
    final Map<Graph.Vertex<T>,Integer> fScore = new HashMap<Graph.Vertex<T>,Integer>();
    ```
4. ¿Qué método habría que modificar para que la heurística representarala **distancia aérea** entre vértices?

    El método que habría que modificar es **"heuristicCostEstimate"**. Este método proporciona una estimación heurística del costo restante desde un nodo hasta el objetivo. Actualmente, devuelve un valor estático de 1 para cada nodo. La modificación necesaria consistiría en reconfigurar este método para implementar la fórmula de la distancia euclidiana y así poder calcular la distancia aérea entre los nodos, empleando las coordenadas espaciales de los vértices (x, y).

    ```
    @SuppressWarnings("unused") 
    protected int heuristicCostEstimate(Graph.Vertex<T> start, Graph.Vertex<T> goal) {
        return 1;
    }
    ```

5. ¿Realiza este método reevaluación de nudos cuando se encuentra una nueva ruta a un determinado vértice? Justifique la respuesta.

    Sí, en el método **"reconstructPath"** se realiza una reevaluación parcial de los nodos cuando se encuentra una nueva ruta hacia un vértice específico. 
    
    Esta función se encarga de actualizar el camino hacia el nodo objetivo utilizando la información almacenada en **cameFrom**, la cual registra los nodos previos a lo largo del camino más corto. De esta manera, recompone parcialmente el camino desde el nodo objetivo hacia el nodo inicial, utilizando estos registros para trazar la secuencia de nodos que componen la ruta más óptima hasta el momento. Al seguir este rastro de nodos y las conexiones entre ellos, actualiza estratégicamente el camino afectado por la nueva ruta descubierta. 

    ```
    private List<Graph.Edge<T>> reconstructPath(Map<Graph.Vertex<T>,Graph.Vertex<T>>       cameFrom, Graph.Vertex<T> current) {
        final List<Graph.Edge<T>> totalPath = new ArrayList<Graph.Edge<T>>();

        while (current != null) {
            final Graph.Vertex<T> previous = current;
            current = cameFrom.get(current);
            if (current != null) {
                final Graph.Edge<T> edge = current.getEdge(previous);
                totalPath.add(edge);
            }
        }
        Collections.reverse(totalPath);
        return totalPath;
    }
    
    ```