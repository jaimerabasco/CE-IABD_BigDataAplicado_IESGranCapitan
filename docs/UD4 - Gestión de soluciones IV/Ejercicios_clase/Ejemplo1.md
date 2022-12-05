# Big Data APlicado
## UD 4 - Gestión de Soluciones IV
### Ejemplo 1 Neo4j

1. Instalar Neo4j. Para ello puedes optar por diferentes opciones
    - Instalarla en tu máquina local (junto con Graph Data Science). Neo4j Community => https://neo4j.com/download-center/?ref=subscription#community
    - Usar SandBox Neo4j.com: https://sandbox.neo4j.com/
      - Necesitamos usar Graph Data Science => https://neo4j.com/docs/graph-data-science/current/
      - Lo necesitamos para aplicar los algoritmos a nuestros grafos
      - Si usas esta opción, tendrás que instalar Neo4j Desktop (sigue las instrucciones una vez creado en sandbox)
    - Crear un contendor docker. Yo voy a usar este caso

2. Neo4j en Docker
   1. Vamos a la [imagen oficial de mondodb en docker](https://hub.docker.com/_/neo4j)

    ```
    docker pull neo4j
    ```

   2. Debemos generar un contenedor que incluya Graph Data Science (https://neo4j.com/docs/graph-data-science/current/installation/installation-docker/)
   3. Vamos a configurarlo en modo desarrollo, para no necesitar usuario y contraseña
   
   ```
    docker run -it --rm -d \
    --name neo4j_1 \
    -p=7474:7474 -p=7687:7687 \
    --user="$(id -u):$(id -g)" \
    -e NEO4J_AUTH=none \
    -e NEO4J_PLUGINS='["graph-data-science"]' \
    neo4j
    ```

3. Vamos a realizar cada una de los código de ejemplo que aparecen en la teoría

4. Toda la documnetación sobre Graph Databases la encontramos en la documnetación oficial para desarrolladores de neo4j => https://neo4j.com/developer/graph-database/

5. Entramos en neo4j => http://localhost:7474/

6. Importamos el grafo de ejemplo

```
// Importamos los nodos
WITH "https://github.com/neo4j-graph-analytics/book/raw/master/data/transport-nodes.csv" AS uri
LOAD CSV WITH HEADERS FROM uri AS row
MERGE (place:Place {id:row.id})
SET place.latitude = toFloat(row.latitude),
place.longitude = toFloat(row.latitude),
place.population = toInteger(row.population)

//Importamos las relaciones
WITH "https://github.com/neo4j-graph-analytics/book/raw/master/data/transport-relationships.csv" AS uri
LOAD CSV WITH HEADERS FROM uri AS row
MATCH (origin:Place {id: row.src})
MATCH (destination:Place {id: row.dst})
MERGE (origin)-[:EROAD {distance: toInteger(row.cost)}]->(destination)
```

7. Comprobamos

```
match(n) return n
```

8. Algoritmo [BFS](https://neo4j.com/docs/graph-data-science/current/algorithms/bfs/) (Breadth First Search)

    Creamos el grafo. **Sólo una vez**
```
// Creamos el grafo. 
CALL gds.graph.project(
    'myGraph_BFR',
    'Place',
    'EROAD'
)
```

Búsqueda en anchura teniendo como inicio el nodo "Doncaster"

```
//Realizamos la búsqueda en anchura
MATCH (a:Place{id:'Doncaster'})
WITH id(a) AS startNode
CALL gds.bfs.stream('myGraph_BFR', {sourceNode: startNode})
YIELD path
UNWIND [ n in nodes(path) | n.id ] AS tags
RETURN tags
```

Podemos probar cambiando el nodo de inicio

```
//Realizamos la búsqueda en anchura
MATCH (a:Place{id:'Amsterdam'})
WITH id(a) AS startNode
CALL gds.bfs.stream('myGraph_BFR', {sourceNode: startNode})
YIELD path
UNWIND [ n in nodes(path) | n.id ] AS tags
RETURN tags
```

9. Algoritmo [DFS](https://neo4j.com/docs/graph-data-science/current/algorithms/dfs/) (Depth First Search)

    Creamos el grafo. **Sólo una vez**
```
// Creamos el grafo. 
CALL gds.graph.project(
    'myGraph_DFS',
    'Place',
    'EROAD'
)
```

Búsqueda en anchura teniendo como inicio el nodo "Doncaster"

```
//Realizamos la búsqueda en Profundidad
MATCH (a:Place{id:'Doncaster'})
WITH id(a) AS startNode
CALL gds.dfs.stream('myGraph_BFR', {sourceNode: startNode})
YIELD path
UNWIND [ n in nodes(path) | n.id ] AS tags
RETURN tags
```

10. Caminos Mínimos. [Algoritmo de Dijkstra: Single-Source Shortest Path](https://neo4j.com/docs/graph-data-science/current/algorithms/dijkstra-single-source/) _(de un nodo a tolos los demás ()_

```
// Creamos el grafo. 
CALL gds.graph.project(
    'myGraph_Dijkstra',
    'Place',
    'EROAD',
    {
        relationshipProperties: 'distance'
    }
)
```

```
MATCH (source:Place {id: 'Amsterdam'})
CALL gds.allShortestPaths.dijkstra.stream('myGraph_Dijkstra', {
    sourceNode: source,
    relationshipWeightProperty: 'distance'
})
YIELD index, sourceNode, targetNode, totalCost, nodeIds, costs, path
RETURN
    index,
    gds.util.asNode(sourceNode).id AS sourceNodeName,
    gds.util.asNode(targetNode).id AS targetNodeName,
    totalCost,
    [nodeId IN nodeIds | gds.util.asNode(nodeId).id] AS nodeNames,
    costs,
    nodes(path) as path
ORDER BY index
```

11. Caminos Mínimos. [Algoritmo de Dijkstra: Single Source-Target Shortest Path](https://neo4j.com/docs/graph-data-science/current/algorithms/dijkstra-source-target/) _(de un nodo origen a otro concreto)_

```
MATCH (source:Place {id: 'Amsterdam'}), (target:Place {id: 'London'})
CALL gds.shortestPath.dijkstra.stream('myGraph_Dijkstra', {
    sourceNode: source,
    targetNode: target,
    relationshipWeightProperty: 'distance'
})
YIELD index, sourceNode, targetNode, totalCost, nodeIds, costs, path
RETURN
    index,
    gds.util.asNode(sourceNode).id AS sourceNodeName,
    gds.util.asNode(targetNode).id AS targetNodeName,
    totalCost,
    [nodeId IN nodeIds | gds.util.asNode(nodeId).id] AS nodeNames,
    costs,
    nodes(path) as path
ORDER BY index
```

12.  Caminos Mínimos. [Algoritmo A*](https://neo4j.com/docs/graph-data-science/current/algorithms/astar/)

```
// Creamos el grafo. 
CALL gds.graph.project(
    'myGraph_A',
    'Place',
    'EROAD',
    {
        nodeProperties: ['latitude', 'longitude', 'population'],
        relationshipProperties: 'distance'
    }
)
```

```
MATCH (source:Place {id: 'Amsterdam'}), (target:Place {id: 'London'})
CALL gds.shortestPath.astar.stream('myGraph_A', {
    sourceNode: source,
    targetNode: target,
    latitudeProperty: 'latitude',
    longitudeProperty: 'longitude',
    relationshipWeightProperty: 'distance'
})
YIELD index, sourceNode, targetNode, totalCost, nodeIds, costs, path
RETURN
    index,
    gds.util.asNode(sourceNode).id AS sourceNodeName,
    gds.util.asNode(targetNode).id AS targetNodeName,
    totalCost,
    [nodeId IN nodeIds | gds.util.asNode(nodeId).id] AS nodeNames,
    costs,
    nodes(path) as path
ORDER BY index
```

13. Medidas de centralidad. Importamos el grafo de ejemplo

```
// Importamos los nodos
WITH "https://github.com/neo4j-graph-analytics/book/raw/master/data/" AS base
WITH base + "social-nodes.csv" AS uri
LOAD CSV WITH HEADERS FROM uri AS row
MERGE (:User {id: row.id})

//Importamos las relaciones
WITH "https://github.com/neo4j-graph-analytics/book/raw/master/data/" AS base
WITH base + "social-relationships.csv" AS uri
LOAD CSV WITH HEADERS FROM uri AS row
MATCH (source:User {id: row.src})
MATCH (destination:User {id: row.dst})
MERGE (source)-[:FOLLOWS]->(destination)
```

14. Medidas de centralidad. [Centralidad de grado (Degree Centrality)](https://neo4j.com/docs/graph-data-science/current/algorithms/degree-centrality/)

```
CALL gds.graph.project(
'myGraph_centralidad_grado',
'User',
    {
        FOLLOWS: {
        orientation: 'REVERSE'
        }
    }
)
```

Como no tenemos ninguna propiedad/coste/puntución en la relación, no hay que indicarla. Si la quisieramos tener en cuenta habría que añadirla dentro de follow de la siguiente forma:

```
CALL gds.graph.project(
'myGraph_centralidad_grado',
'User',
    {
        FOLLOWS: {
        orientation: 'REVERSE',
        properties: ['score']
        }
    }
)
```

La centralidad de grado de este grafo sería

```
CALL gds.degree.stream('myGraph_centralidad_grado')
YIELD nodeId, score
RETURN gds.util.asNode(nodeId).id AS id, score AS followers
ORDER BY followers DESC, id DESC
```

15. Medidas de centralidad. [Cercanía (Closeness Centrality)](https://neo4j.com/docs/graph-data-science/current/algorithms/closeness-centrality/)

```
CALL gds.graph.project(
    'myGraph_cercania',
    'User',
    'FOLLOWS'
)
```

El cálculo de la cercanía sería

```
CALL gds.beta.closeness.stream('myGraph_cercania')
YIELD nodeId, score
RETURN gds.util.asNode(nodeId).id AS id, score
ORDER BY score DESC
```

16. Medidas de centralidad. [Intermediación (Betweenness Centrality)](https://neo4j.com/docs/graph-data-science/current/algorithms/betweenness-centrality/)

```
CALL gds.graph.project(
    'myGraph_intermediacion',
    'User',
    'FOLLOWS'
)
```

Como no tenemos ninguna propiedad/coste/puntución en la relación, no hay que indicarla. Si la quisieramos tener en cuenta habría que añadirla dentro de follow de la siguiente forma:

```
CALL gds.graph.project(
    'myGraph_intermediacion',
    'User',
    {
        FOLLOWS:
        {
            properties: 'weight'
        }
    }
)
```

El cálculo de la Intermediación sería

```
CALL gds.betweenness.stream('myGraph_intermediacion')
YIELD nodeId, score
RETURN gds.util.asNode(nodeId).id AS id, score
ORDER BY score DESC
```

17.  Detección de comunidades. [Conteo de triángulos(Triangle Count)](https://neo4j.com/docs/graph-data-science/current/algorithms/triangle-count/)

```
CALL gds.graph.project(
    'myGraph_conteo_triangulo',
    'User',
    {
        FOLLOWS: {
        orientation: 'UNDIRECTED'
        }
    }
)

CALL gds.triangleCount.stream('myGraph_conteo_triangulo')
YIELD nodeId, triangleCount
RETURN gds.util.asNode(nodeId).id AS id, triangleCount
ORDER BY triangleCount DESC
```

18. Detección de comunidades. [Coeficiente local de clustering (Local Clustering Coefficient)](https://neo4j.com/docs/graph-data-science/current/algorithms/local-clustering-coefficient/)

```
CALL gds.graph.project(
    'myGraph_LCC',
    'User',
    {
        FOLLOWS: {
        orientation: 'UNDIRECTED'
        }
    }
)
```
```
CALL gds.localClusteringCoefficient.stream('myGraph_LCC')
YIELD nodeId, localClusteringCoefficient
RETURN gds.util.asNode(nodeId).id AS id, localClusteringCoefficient
ORDER BY localClusteringCoefficient DESC
```

19. Detección de comunidades. [Componentes fuertemente conexas (Strongly Connected Components)](https://neo4j.com/docs/graph-data-science/current/algorithms/strongly-connected-components/)
_alpha a 09-11-2022_

```
CALL gds.graph.project(
    'myGraph_SCC',
    'User',
    'FOLLOWS'
)
```
```
CALL gds.alpha.scc.stream('myGraph_SCC', {})
YIELD nodeId, componentId
RETURN gds.util.asNode(nodeId).id AS Name, componentId AS Component
ORDER BY Component DESC
```

20. Predicción de enlaces. [Vecinos comunes (Common Neighbors)](https://neo4j.com/docs/graph-data-science/current/alpha-algorithms/common-neighbors/)
_alpha a 09-11-2022_

```
//No funciona ni en la documentación oficial (09-11-2022)
MATCH (x:User{id:'Charles'})
MATCH (y:User{id:'Bridget'})
RETURN gds.alpha.linkprediction.commonNeighbors(x,y) AS score
```

21. Predicción de enlaces. [Adhesión preferencial (Preferential Attachment)](https://neo4j.com/docs/graph-data-science/current/alpha-algorithms/preferential-attachment/)
_alpha a 09-11-2022_

```
MATCH (x:User{id:'Charles'})
MATCH (y:User{id:'Bridget'})
RETURN gds.alpha.linkprediction.preferentialAttachment(x, y) AS score
```

22. Predicción de enlaces. [Asignación de recursos (Resource Allocation)](https://neo4j.com/docs/graph-data-science/current/alpha-algorithms/resource-allocation/)
_alpha a 09-11-2022_

```
//No funciona ni en la documentación oficial (09-11-2022)
MATCH (x:User{id:'Charles'})
MATCH (y:User{id:'Bridget'})
RETURN gds.alpha.linkprediction.resourceAllocation(x, y) AS score
```