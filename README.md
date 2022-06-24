## Other cheat sheets:
* [cheat sheets collection](https://lzone.de/cheat-sheet/)
* [cheat sheets](https://www.cheatography.com)

## useful tools:
* [ascii graphics for drawing Architecture Diagrams in text](http://asciiflow.com/)  
* [list of markdown code supported languages](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml)  

## search function
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
