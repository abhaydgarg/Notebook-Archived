# Express.js - v4

## Directory Structure

### Single app structure

```
├── app.js // glue everything together
├── package.json
├── helpers
├── middlewares
├── routes
├── views
├── controllers
├── models
├── config
├── public
  ├── js
  ├── img
  └── css
├── test
├── logs
```

### Sub app structure

```
├── app.js // glue everything together
├── package.json
├── helpers
├── middlewares
├── front // sub app
  ├── routes
  ├── views
  ├── controllers
  ├── models
  ├── app.js
├── admin // sub app
  ├── routes
  ├── views
  ├── controllers
  ├── models
  ├── app.js
├── config
├── public
  ├── js
  ├── img
  └── css
├── test
├── logs
```
