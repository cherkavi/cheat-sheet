## Other cheat sheets:
* [cheat sheets collection](https://lzone.de/cheat-sheet/)
* [cheat sheets](https://www.cheatography.com)

## useful tools:
### collaboration whiteboard drawing
* [white board for collaboration](https://sketchtogether.com/)
* [white board for collaboration](https://miro.com/)
* [drawing tool](https://excalidraw.com/)

### diagram drawing 
* [ascii graphics for drawing Architecture Diagrams in text](http://asciiflow.com/)  
* [uml, sysml, archimate tool](https://online.visual-paradigm.com/)

### markdown
* [list of markdown code supported languages](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml)  

### regular expressions
* [regular expressions regexp](https://regex101.com)

### stream editor
* [sed escape, sed online escape](https://dwaves.de/tools/escape/)

### online coding
* [code compiler/editor online](https://www.jdoodle.com/)
* [code compiler/editor online](https://onecompiler.com/)
* [typescript sandbox](https://www.typescriptlang.org/)
* [repl online](https://replit.com/)
* [code sandbox](https://codesandbox.io/)

### visual database ide
* [Azure Data Studio](https://azure.microsoft.com/products/data-studio)
* [DbGate](https://dbgate.org/)
* [Sqlectron](https://sqlectron.github.io/)
* [Antares SQL](https://antares-sql.app/)
* [Beekeeper Studio](https://www.beekeeperstudio.io/)

### password storage
* [one time password storage](https://onetimesecret.com/)
---
## useful search function for using whole cheat sheet
```sh
function cheat-grep(){
    if [[ $1 == "" ]]; then
        echo "nothing to search"
        return;
    fi

    search_line=""
    for each_input_arg in "$@"; do
        if [[ $search_line == "" ]]; then
            search_line=$each_input_arg
        else
            search_line=$search_line".*"$each_input_arg
        fi
    done

    grep -r $search_line -i -A 2 $HOME_PROJECTS/cheat-sheet/*.md $HOME_PROJECTS/bash-example/*
}
```
