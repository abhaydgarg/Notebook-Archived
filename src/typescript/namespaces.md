# Namespace

Also called **Internal Modules**

A namespace is a way to logically group related code. This is inbuilt into TypeScript unlike in JavaScript where variables declarations go into a global scope and if multiple JavaScript files are used within same project there will be possibility of overwriting or misconstruing the same variables, which will lead to the “global namespace pollution problem” in JavaScript.

```typescript
namespace Utility {
  export function log(msg) {
    console.log(msg);
  }
  export function error(msg) {
    console.error(msg);
  }
}

// usage
Utility.log('Call me');
Utility.error('maybe!');
```

## Namespace from different file

Use `/// <reference path="filename.ts" />` to refer namespace from different file.

```typescript
//# IShape.ts
namespace Drawing {
  export interface IShape {
    draw();
  }
}

//# Circle.ts
namespace Drawing {
  export class Circle implements IShape {
    public draw() {
      console.log("Circle is drawn");
    }
  }
}

//# Test.ts

/// <reference path = "IShape.ts" />
/// <reference path = "Circle.ts" />

function drawAllShapes(shape: Drawing.IShape) {
  shape.draw();
}
drawAllShapes(new Drawing.Circle());
```

## Nested namespace

```typescript
//# FileName: Invoice.ts
namespace tutorialPoint {
  export namespace invoiceApp {
    export class Invoice {
      public calculateDiscount(price: number) {
        return price * .40;
      }
    }
  }
}

//# FileName: InvoiceTest.ts

/// <reference path = "Invoice.ts" />
var invoice = new tutorialPoint.invoiceApp.Invoice();
console.log(invoice.calculateDiscount(500));
```
