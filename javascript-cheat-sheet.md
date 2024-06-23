# JavaScript
## [url to main cheat sheet with examples](https://github.com/cherkavi/javascripting)
## [Browser Graphics](d3-cheat-sheet.md)
## [from JS to any language with single codebase](https://github.com/aws/jsii)

## Typescript cheat sheet
### start new typescript application
```sh
### create folder
mkdir sandbox-typescript
cd sandbox-typescript

# init npm 
npm init

# sudo apt install node-typescript
tsc --init
# {
#   "compilerOptions": {
#     "target": "es6",
#     "module": "commonjs",
#     "esModuleInterop": true,
#     "moduleResolution": "node",
#     "sourceMap": true,
#     "noImplicitAny": false,
#     "outDir": "dist"
#   },
#   "lib": ["es2015"]
# }
vim tsconfig.json


### install dependencies
## typescript should have the same version `tsc --version` - like 3.8.3 
npm install -D typescript
npm install -D tslint
## local server 
npm install -S express
npm install -D @types/express
## custom libraries 
npm install -D json-bigint
npm install -D bignumber.js

# tslint init
./node_modules/.bin/tslint --init

# "rules": {"no-console": false},
vim tslint.json

# "start": { "tsc && node dist/app.js", ...
vim package.json

### application start point
# import express from 'express';
# const app = express();
# const port = 3000;
# app.get('/', (req, res) => {res.send('up and running');});
# app.listen(port, () => {console.error(`server started on port ${port}`);});

mkdir src
vim src/app.ts

# start app
npm start
```

## Terminal sh/bash replacement
```sh
npm install -g bun
```

```javascript
import { $ } from "bun";

const output = await $`ls *.js`.arrayBuffer();
```

## JS Obfuscator
```sh
npm install --save-dev javascript-obfuscator

javascript-obfuscator input_file_name.js [options]
javascript-obfuscator input_file_name.js --output output_file_name.js [options]
```
