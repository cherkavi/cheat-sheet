# ReactJS cheat sheet
* [react code examples/explanations](https://github.com/cherkavi/javascripting/tree/master/react)  
* [create project](https://github.com/cherkavi/javascripting/tree/master/react/README.md#create-react-app)

## cheat sheets
* https://devhints.io/react
* https://www.freecodecamp.org/news/the-react-cheatsheet-for-2020/
* https://reactjs.org/
* [react roadmap](https://github.com/adam-golab/react-developer-roadmap)

## Links
* [doc](https://create-react-app.dev/)  
* [chrome plugin](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)  


## Workplace with Visual code
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


