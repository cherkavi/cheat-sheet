Workplace with Visual code
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
