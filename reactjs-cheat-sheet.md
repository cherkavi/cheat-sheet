# ReactJS cheat sheet

## cheat sheets
* https://devhints.io/react
* https://www.freecodecamp.org/news/the-react-cheatsheet-for-2020/
* https://reactjs.org/

## Links
* [my projects with examples](https://github.com/cherkavi/javascripting/tree/master/react)  
* [doc](https://create-react-app.dev/)  
* [chrome plugin](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)  


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

## create project
```sh
npx create-react-app my-app
```

## create component
```sh
npm install --save-dev create-react-component-folder
npx crcf myComponent
```

![type of components](https://i.postimg.cc/RhWJ08B2/ksnip-20210211-230707.png)

![lifecycle - creation](https://i.postimg.cc/5y6kP6F9/lifecycle-creation-learning-card.png)
![lifecycle - update](https://i.postimg.cc/wxGr1cS1/lifecycle-update-external-learning-card.png)

## style pseudo selector
```js
# npm install --save radium

import Radium from 'radium';
style={
   ':hover': {
   	backgroundColor:"red"
   },
   '@media (min-witdh: 480px)':{
   	width: "350px"
   }
}

export default Radium(MyComponent);

// for media - wrap root component with:
// import { StyleRoot } from 'radium';
// <StyleRoot> </StyleRoot>
```

![NextJS ServerSideRendering](https://i.postimg.cc/L6nxk6BP/nextjs-ssr.png)

