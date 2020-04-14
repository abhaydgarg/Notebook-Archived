# Adjancy matrix implementation

## Vertex.js

```js
module.exports = class Vertex {
  constructor (data) {
    this.data = data;
  }
};
```

## Directed.js

```js
const Vertex = require('./Vertex');

module.exports = class Directed {
  constructor () {
    this.vertices = [];
    this.matrix = [];
  }

  getVertices () {
    return this.vertices;
  }

  getMatrix () {
    return this.matrix;
  }

  addVertex (data) {
    // Matrix must be resized.
    let length = this.vertices.push(new Vertex(data));
    let row = length - 1;
    // Create a row
    this.matrix[row] = [];
    // Add new columns to existed rows
    // and to new row. Need to append column
    // to already existed rows to make a matrix
    // of size (V*V)
    for (var i = 0; i <= row; i++) {
      for (var j = 0; j <= row; j++) {
        if (typeof this.matrix[i][j] === 'undefined') {
          this.matrix[i][j] = 0;
        }
      }
    }
  }

  addEdge (i, j) {
    this.matrix[i][j] = 1;
  }

  removeVertex (index) {
    this.vertices.splice(index, 1);
    // Matrix must be resized.
    this.matrix.splice(index, 1);
    for (var i = 0; i < this.matrix.length; i++) {
      this.matrix[i].splice(index, 1);
    }
  }

  removeEdge (i, j) {
    this.matrix[i][j] = 0;
  }
};
```

## Undirected.js

```js
const Vertex = require('./Vertex');

module.exports = class Undirected {
  constructor () {
    this.vertices = [];
    this.matrix = [];
  }

  getVertices () {
    return this.vertices;
  }

  getMatrix () {
    return this.matrix;
  }

  addVertex (data) {
    // Matrix must be resized.
    let length = this.vertices.push(new Vertex(data));
    let row = length - 1;
    // Create a row
    this.matrix[row] = [];
    // Add new columns to existed rows
    // and to new row. Need to append column
    // to already existed rows to make a matrix
    // of size (V*V)
    for (var i = 0; i <= row; i++) {
      for (var j = 0; j <= row; j++) {
        if (typeof this.matrix[i][j] === 'undefined') {
          this.matrix[i][j] = 0;
        }
      }
    }
  }

  addEdge (i, j) {
    this.matrix[i][j] = 1;
    this.matrix[j][i] = 1;
  }

  removeVertex (index) {
    this.vertices.splice(index, 1);
    // Matrix must be resized.
    this.matrix.splice(index, 1);
    for (var i = 0; i < this.matrix.length; i++) {
      this.matrix[i].splice(index, 1);
    }
  }

  removeEdge (i, j) {
    this.matrix[i][j] = 0;
    this.matrix[j][i] = 0;
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

  console.dir(graph.getMatrix(), {depth: null});
  console.dir(graph.getVertices(), {depth: null});
};

module.exports.addEdge = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let edges = [[0, 1], [1, 0], [1, 2], [2, 3], [3, 4]];
  let graph = new Directed();
  arr.forEach((element) => {
    graph.addVertex(element);
  });

  edges.forEach(edge => {
    graph.addEdge(edge[0], edge[1]);
  });

  console.dir(graph.getMatrix(), {depth: null});
  console.dir(graph.getVertices(), {depth: null});
};

module.exports.removeVertex = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let edges = [[0, 1], [1, 0], [1, 2], [2, 3], [3, 4]];
  let removeVertex = 1;
  let graph = new Directed();
  arr.forEach((element) => {
    graph.addVertex(element);
  });

  edges.forEach(edge => {
    graph.addEdge(edge[0], edge[1]);
  });

  graph.removeVertex(removeVertex);

  console.dir(graph.getMatrix(), {depth: null});
  console.dir(graph.getVertices(), {depth: null});
};

module.exports.removeEdge = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let edges = [[0, 1], [1, 0], [1, 2], [2, 3], [3, 4]];
  let removeEdge = [1, 0];
  let graph = new Directed();
  arr.forEach((element) => {
    graph.addVertex(element);
  });

  edges.forEach(edge => {
    graph.addEdge(edge[0], edge[1]);
  });

  graph.removeEdge(removeEdge[0], removeEdge[1]);

  console.dir(graph.getMatrix(), {depth: null});
  console.dir(graph.getVertices(), {depth: null});
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

  console.dir(graph.getMatrix(), {depth: null});
  console.dir(graph.getVertices(), {depth: null});
};

module.exports.addEdge = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let edges = [[0, 1], [1, 2], [2, 3], [3, 4], [0, 4], [1, 3], [2, 4]];
  let graph = new Undirected();
  arr.forEach((element) => {
    graph.addVertex(element);
  });

  edges.forEach(edge => {
    graph.addEdge(edge[0], edge[1]);
  });

  console.dir(graph.getMatrix(), {depth: null});
  console.dir(graph.getVertices(), {depth: null});
};

module.exports.removeVertex = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let edges = [[0, 1], [1, 2], [2, 3], [3, 4], [0, 4], [1, 3], [2, 4]];
  let removeVertex = 1;
  let graph = new Undirected();
  arr.forEach((element) => {
    graph.addVertex(element);
  });

  edges.forEach(edge => {
    graph.addEdge(edge[0], edge[1]);
  });

  graph.removeVertex(removeVertex);

  console.dir(graph.getMatrix(), {depth: null});
  console.dir(graph.getVertices(), {depth: null});
};

module.exports.removeEdge = function () {
  let arr = ['A', 'B', 'C', 'D', 'E'];
  let edges = [[0, 1], [1, 2], [2, 3], [3, 4], [0, 4], [1, 3], [2, 4]];
  let removeEdge = [1, 0];
  let graph = new Undirected();
  arr.forEach((element) => {
    graph.addVertex(element);
  });

  edges.forEach(edge => {
    graph.addEdge(edge[0], edge[1]);
  });

  graph.removeEdge(removeEdge[0], removeEdge[1]);

  console.dir(graph.getMatrix(), {depth: null});
  console.dir(graph.getVertices(), {depth: null});
};
```
