# Modules

Also called **external modules.**

Same as ECMASCRIPT 6.

```typescript
//# IShape.ts
export interface IShape {
  draw();
}

//# Circle.ts
import shape = require("./IShape");
export class Circle implements shape.IShape {
  public draw() {
    console.log("Cirlce is drawn (external module)");
  }
}

//# Triangle.ts
import shape = require("./IShape");
export class Triangle implements shape.IShape {
  public draw() {
    console.log("Triangle is drawn (external module)");
  }
}

//# TestShape.ts
import shape = require("./IShape");
import circle = require("./Circle");
import triangle = require("./Triangle");

function drawAllShapes(shapeToDraw: shape.IShape) {
  shapeToDraw.draw();
}

drawAllShapes(new circle.Circle());
drawAllShapes(new triangle.Triangle());
```
