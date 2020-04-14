# Declaration files

`xxx.d.ts`, declaration files are created for declaring type annotation for files written in javascript, coffeescript etc. The idea behind this is to avoid rewriting say jquery lib all over again in typescript and just create `jquery.d.ts` declaration file containing type annotation definitions. Because typescript does not allow or ask for type annotation for javascript files if flag `allowjs` is `false` in `tsconfig.json` file.
