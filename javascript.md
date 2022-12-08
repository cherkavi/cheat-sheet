## Typescript
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

## ReactJS
### cheat sheets
* https://devhints.io/react
* https://www.freecodecamp.org/news/the-react-cheatsheet-for-2020/
* https://reactjs.org/


### Workplace with Visual code
* download addon: firefox-devtools.vscode-firefox-debug
* .vscode/launch.json
```
{
    "version": "0.2.0",
    "configurations": [
		{
			"name": "d3-population-born.html",
			"type": "firefox",
            "request": "launch",
            "reAttach": true,
            "file": "${workspaceFolder}/d3-population-born.html",
        }
    ],
	"compounds": [
		{
			"name": "server & extension",
			"configurations": [
                "d3-population-born.html"
			]
		}
	]
}
```
* user settings -> find "firefox" -> "Firefox: Executable", write path to "Firefox Developer Edition"

* create file in the root: jsconfig.json
```json
{
    "compilerOptions": {
        "target": "ES6"
    },    
}
```

### check variables
```js
  <script>
    'use strict';
```

### import alias
```js
import { Location as LocationModel } from 'src/app/core/models/location.model';
```
