# Points to note

## Convert promise into observable

> **fromPromise** takes Promise as an argument not an function that return Promise.

```js
//# Wrong
function obs(state) {
  return Observable.fromPromise(() => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        if (state === false) {
          reject('Obs - Rejected!');
        }
        resolve('Obs - Resolved!');
      }, 100);
    });
  });
}


//# Correct - Takes promise
function obs(state) {
  return Observable.fromPromise(
    new Promise((resolve, reject) => {
      setTimeout(() => {
        if (state === false) {
          reject('Obs - Rejected!');
        }
        resolve('Obs - Resolved!');
      }, 100);
    })
  );
}
```

## Importing rxjs components

```js
import { Observable } from 'rxjs/Observable';

import 'rxjs/add/observable/of';
import 'rxjs/add/observable/forkJoin';
import 'rxjs/add/observable/fromPromise';

import 'rxjs/add/operator/concat';
import 'rxjs/add/operator/mergeMap';
import 'rxjs/add/operator/switchMap';
import 'rxjs/add/operator/catch';
import 'rxjs/add/operator/map';
```
