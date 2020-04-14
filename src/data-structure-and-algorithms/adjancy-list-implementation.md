# Adjancy list implementation

## Vertex.js

```js
module.exports = class Vertex {
  constructor (data) {
    this.data = data;
    this.edges = [];
  }
};
```

## Directed.js

```js
const Vertex = require('./Vertex');

module.exports = class Directed {
  constructor () {
    this.vertices = new Map();
  }

  getVertices () {
    return this.vertices;
  }

  getVerticesObject () {
    let vertices = {};
    this.vertices.forEach((value, key, map) => {
      vertices[key] = value;
    });
    return vertices;
  }

  clone () {
    let vertices = new Map();
    this.vertices.forEach((value, key, map) => {
      vertices.set(key, {
        data: value.data,
        edges: [...value.edges]
      });
    });
    return vertices;
  }

  addVertex (data) {
    this.vertices.set(data, new Vertex(data));
  }

  addEdge (from, to) {
    this.vertices.get(from).edges.push(to);
  }

  removeVertex (identifier) {
    this.vertices.delete(identifier);
    this.vertices.forEach((value, key, map) => {
      for (var i = 0; i < value.edges.length; i++) {
        if (value.edges[i] === identifier) {
          value.edges.splice(i, 1);
        }
      }
    });
  }

  removeEdge (from, to) {
    let edges = this.vertices.get(from).edges;
    for (var i = 0; i < edges.length; i++) {
      if (edges[i] === to) {
        edges.splice(i, 1);
        break;
      }
    }
  }
};
```

## Undirected.js

```js
const Vertex = require('./Vertex');

module.exports = class Undirected {
  constructor () {
    this.vertices = new Map();
  }

  getVertices () {
    return this.vertices;
  }

  getVerticesObject () {
    let vertices = {};
    this.vertices.forEach((value, key, map) => {
      vertices[key] = value;
    });
    return vertices;
  }

  clone () {
    let vertices = new Map();
    this.vertices.forEach((value, key, map) => {
      vertices.set(key, {
        data: value.data,
        edges: [...value.edges]
      });
    });
    return vertices;
  }

  addVertex (data) {
    this.vertices.set(data, new Vertex(data));
  }

  addEdge (from, to) {
    this.vertices.get(from).edges.push(to);
    this.vertices.get(to).edges.push(from);
  }

  removeVertex (identifier) {
    this.vertices.delete(identifier);
    this.vertices.forEach((value, key, map) => {
      for (var i = 0; i < value.edges.length; i++) {
        if (value.edges[i] === identifier) {
          value.edges.splice(i, 1);
        }
      }
    });
  }

  removeEdge (from, to) {
    let fromEdges = this.vertices.get(from).edges;
    let toEdges = this.vertices.get(to).edges;
    let index = 0;
    for (index = 0; index < fromEdges.length; index++) {
      if (fromEdges[index] === to) {
        fromEdges.splice(index, 1);
        break;
      }
    }
    for (index = 0; index < toEdges.length; index++) {
      if (toEdges[index] === from) {
        toEdges.splice(index, 1);
        break;
      }
    }
  }
};
```

## Usage

### Directed

```js
const Directed = require('./Directed');

module.exports.addVertex = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let graph = new Directed();
  arr.forEach((element) => {
    graph.addVertex(element);
  });

  console.dir(graph.getVerticesObject(), {depth: null});
};

module.exports.addEdge = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let edges = [['A', 'B'], ['B', 'A'], ['B', 'C'], ['C', 'D'], ['D', 'E']];
  let graph = new Directed();
  arr.forEach((element) => {
    graph.addVertex(element);
  });

  edges.forEach(edge => {
    graph.addEdge(edge[0], edge[1]);
  });

  console.dir(graph.getVerticesObject(), {depth: null});
};

module.exports.removeVertex = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let edges = [['A', 'B'], ['B', 'A'], ['B', 'C'], ['C', 'D'], ['D', 'E']];
  let removeVertex = 'B';
  let graph = new Directed();
  arr.forEach((element) => {
    graph.addVertex(element);
  });

  edges.forEach(edge => {
    graph.addEdge(edge[0], edge[1]);
  });

  graph.removeVertex(removeVertex);

  console.dir(graph.getVerticesObject(), {depth: null});
};

module.exports.removeEdge = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let edges = [['A', 'B'], ['B', 'A'], ['B', 'C'], ['C', 'D'], ['D', 'E']];
  let removeEdge = ['B', 'A'];
  let graph = new Directed();
  arr.forEach((element) => {
    graph.addVertex(element);
  });

  edges.forEach(edge => {
    graph.addEdge(edge[0], edge[1]);
  });

  graph.removeEdge(removeEdge[0], removeEdge[1]);

  console.dir(graph.getVerticesObject(), {depth: null});
};
```

### Undirected

```js
const Undirected = require('./Undirected');

module.exports.addVertex = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let graph = new Undirected();
  arr.forEach((element) => {
    graph.addVertex(element);
  });

  console.dir(graph.getVerticesObject(), {depth: null});
};

module.exports.addEdge = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let edges = [['A', 'B'], ['B', 'C'], ['C', 'D'], ['D', 'E'], ['A', 'E'], ['B', 'D'], ['C', 'E']];
  let graph = new Undirected();
  arr.forEach((element) => {
    graph.addVertex(element);
  });

  edges.forEach(edge => {
    graph.addEdge(edge[0], edge[1]);
  });

  console.dir(graph.getVerticesObject(), {depth: null});
};

module.exports.removeVertex = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let edges = [['A', 'B'], ['B', 'C'], ['C', 'D'], ['D', 'E'], ['A', 'E'], ['B', 'D'], ['C', 'E']];
  let removeVertex = 'B';
  let graph = new Undirected();
  arr.forEach((element) => {
    graph.addVertex(element);
  });

  edges.forEach(edge => {
    graph.addEdge(edge[0], edge[1]);
  });

  graph.removeVertex(removeVertex);

  console.dir(graph.getVerticesObject(), {depth: null});
};

module.exports.removeEdge = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let edges = [['A', 'B'], ['B', 'C'], ['C', 'D'], ['D', 'E'], ['A', 'E'], ['B', 'D'], ['C', 'E']];
  let removeEdge = ['A', 'B'];
  let graph = new Undirected();
  arr.forEach((element) => {
    graph.addVertex(element);
  });

  edges.forEach(edge => {
    graph.addEdge(edge[0], edge[1]);
  });

  graph.removeEdge(removeEdge[0], removeEdge[1]);

  console.dir(graph.getVerticesObject(), {depth: null});
};
```
