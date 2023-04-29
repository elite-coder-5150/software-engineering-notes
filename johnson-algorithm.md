
The Johnson algorithm is a graph-based algorithm for finding the shortest path between all pairs of nodes in a weighted
graph. It works by first transforming the original graph into a new graph with non-negative edge weights, then using
Dijkstra's algorithm to find the shortest path between all pairs of nodes.

The transformation involves adding a new source node to the graph and connecting it to all other nodes with zero-weight
edges. Then, the Bellman-Ford algorithm is used to calculate a weight adjustment for each node, which is based on the
shortest path from the new source node to that node. The adjustment values are then used to transform the original edge
weights into non-negative edge weights.

Once the transformation is complete, Dijkstra's algorithm is used to find the shortest path between all pairs of nodes
in the transformed graph. The final result is a matrix of shortest path distances between all pairs of nodes in the
original graph.

In summary, the Johnson algorithm uses a combination of the Bellman-Ford and Dijkstra's algorithms to efficiently find
the shortest path between all pairs of nodes in a weighted graph. It is particularly useful when the graph has negative
edge weights, as it can handle them by transforming the graph into a new one with non-negative edge weights.

Here is the code for the Johnson algorithm in javascript

```javascript
function bellmanFord(adjList, s) {
  const V = adjList.length;
  const dist = Array(V).fill(Infinity);
  dist[s] = 0;

  for (let i = 0; i < V - 1; i++) {
    for (let u = 0; u < V; u++) {
      for (const [v, w] of adjList[u]) {
        if (dist[u] != Infinity && dist[u] + w < dist[v]) {
          dist[v] = dist[u] + w;
        }
      }
    }
  }

  return dist;
}

function dijkstra(adjList, s, t, adjustment) {
  const V = adjList.length;
  const dist = Array(V).fill(Infinity);
  const pq = [[0, s]];
  dist[s] = 0;

  while (pq.length > 0) {
    const [du, u] = pq.shift();
    if (du > dist[u]) continue;

    for (const [v, w] of adjList[u]) {
      const dv = dist[u] + w + adjustment[u] - adjustment[v];
      if (dv < dist[v]) {
        dist[v] = dv;
        pq.push([dist[v], v]);
      }
    }
  }

  return dist[t] + adjustment[t] - adjustment[s];
}

function johnson(graph) {
  const V = graph.length;
  const dist = Array(V).fill().map(() => Array(V).fill(Infinity));
  const adjList = Array(V + 1).fill().map(() => []);

  for (let u = 0; u < V; u++) {
    for (let v = 0; v < V; v++) {
      if (graph[u][v] !== Infinity) {
        adjList[u].push([v, graph[u][v]]);
      }
    }
  }

  adjList[V] = Array(V).fill().map((_, i) => [i, 0]);

  // const adjustment = bellmanFord(adjList, V);

  for (let u = 0; u < V; u++) {
    for (let v = 0; v < V; v++) {
      if (graph[u][v] !== Infinity) {
        //dist[u][v] = dijkstra(adjList, u, v, adjustment);
          dist[u][v] = dijkstra(adjList, u, v, bellmanFord(adjList, V))
      }
    }
  }

  return dist;
}

const graph = [
  [0, 1, 4, Infinity, Infinity],
  [1, 0, 2, 7, 5],
  [4, 2, 0, 1, Infinity],
  [Infinity, 7, 1, 0, 3],
  [Infinity, 5, Infinity, 3, 0],
];
const dist = johnson(graph);
const V = graph.length;
```
