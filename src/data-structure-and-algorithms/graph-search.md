# Code

## Search.js

```js
module.exports = class Search {
  static bfs (startVertex, graph) {
    let order = [];
    let visited = {};
    let queue = [];

    for (let [key, value] of graph) {
      visited[key] = false;
    }

    queue.push(graph.get(startVertex));
    visited[startVertex] = true;

    while (queue.length > 0) {
      let current = queue.shift();
      order.push(current.data);
      for (var i = 0; i < current.edges.length; i++) {
        if (visited[current.edges[i]] === false) {
          visited[current.edges[i]] = true;
          queue.push(graph.get(current.edges[i]));
        }
      }
    }
    return order;
  }

  static dfs (startVertex, graph) {
    let order = [];
    let visited = {};
    let stack = [];

    for (let [key, value] of graph) {
      visited[key] = false;
    }

    stack.push(graph.get(startVertex));
    visited[startVertex] = true;

    while (stack.length > 0) {
      let current = stack.pop();
      order.push(current.data);
      for (var i = 0; i < current.edges.length; i++) {
        if (visited[current.edges[i]] === false) {
          visited[current.edges[i]] = true;
          stack.push(graph.get(current.edges[i]));
        }
      }
    }
    return order;
  }
};
```

### Usage

```js
const Graph = require('./UndirectedAdjancyList');
const Algo = require('./Search');

module.exports.bfs = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let edges = [['A', 'B'], ['B', 'C'], ['C', 'D'], ['D', 'E'], ['A', 'E'], ['B', 'D'], ['C', 'E']];
  let startVertex = 'A';
  let graph = new Graph();
  arr.forEach((element) => {
    graph.addVertex(element);
  });
  edges.forEach(edge => {
    graph.addEdge(edge[0], edge[1]);
  });

  let order = Algo.bfs(startVertex, graph.getVertices());

  console.dir(order, {depth: null});
};

module.exports.dfs = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let edges = [['A', 'B'], ['B', 'C'], ['C', 'D'], ['D', 'E'], ['A', 'E'], ['B', 'D'], ['C', 'E']];
  let startVertex = 'A';
  let graph = new Graph();
  arr.forEach((element) => {
    graph.addVertex(element);
  });
  edges.forEach(edge => {
    graph.addEdge(edge[0], edge[1]);
  });

  let order = Algo.dfs(startVertex, graph.getVertices());

  console.log(order, {depth: null});
};
```
