# cheat sheet

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

## other cheat-tools
* [cht.sh](https://github.com/chubin/cheat.sh)
* [tldr](https://tldr.sh/)
* [how2](https://how2terminal.com/download)

## other cheat sheets:
* [root/entrypoint to different resources](https://github.com/sindresorhus/awesome)
* [cheat sheets collection](https://lzone.de/cheat-sheet/)
* [cheat sheets](https://www.cheatography.com)

## [documentation examples, how to write good documentation, documentation tools](https://github.com/matheusfelipeog/beautiful-docs)

## useful tools:
### [free online resources for developers](https://github.com/ripienaar/free-for-dev?tab=readme-ov-file#web-hosting)
### [list of online tools](https://github.com/goabstract/Awesome-Design-Tools)
### [free hosted applications, locally started applications](https://github.com/awesome-selfhosted/awesome-selfhosted)
### collaboration whiteboard drawing
* [miro: white board for collaboration](https://webwhiteboard.com/)
* [draw chat: white board with chat](https://draw.chat)
* [excalidraw](https://excalidraw.com/)
    * [using razer-keypad with excalidraw](https://github.com/cherkavi/solutions/blob/master/razer-keypad/README.md)
    * [your hand-made drawing convert to vector svg](https://github.com/cherkavi/bash-example/blob/master/image-convert-to-vector.sh)
    * [vector svg to excalidraw online](https://svgtoexcalidraw.com/)
    * [vector svg to excalidraw src](https://github.com/excalidraw/svg-to-excalidraw)

* [local drawing tool](https://github.com/tldraw/tldraw)

### tools for developers on localhost
* [sdkman](https://sdkman.io/)
  > switch SDK of you favorite language fastly   
  > show additional info in your console prompt  
* [flox](https://github.com/flox/flox)
  > create virtual environment with dependencies    

### editor
* https://vscode.dev
* https://code.visualstudio.com/
* https://lite-xl.com/

### render/run html/js/javascript files from github

#### githack.com
* Development
`https://raw.githack.com/[user]/[repository]/[branch]/[filename.ext]`
* Production (CDN)
`https://rawcdn.githack.com/[user]/[repository]/[branch]/[filename.ext]`
example:
`https://raw.githack.com/cherkavi/javascripting/master/d3/d3-bar-chart.html`

#### github.io
`http://htmlpreview.github.io/?[full path to html page]`
example
`http://htmlpreview.github.io/?https://github.com/cherkavi/javascripting/blob/master/d3/d3-bar-chart.html`  
`http://htmlpreview.github.io/?https://github.com/twbs/bootstrap/blob/gh-pages/2.3.2/index.html`

#### rawgit
`https://rawgit.com/[user]/[repository]/master/index.html`
`https://rawgit.com/cherkavi/javascripting/master/d3/d3-bar-chart.html`

### diagram drawing 
* [ascii graphics for drawing Architecture Diagrams in text](http://asciiflow.com/)  
* [uml, sysml, archimate tool](https://online.visual-paradigm.com/)

### markdown
* [diagrams within markdown](https://mermaid.js.org/syntax/flowchart.html)
  * [mermaid live](https://mermaid.live/)
    * [mermaid cli](https://github.com/mermaid-js/mermaid-cli)
    * [mermaid architecture icons logos:s3](https://icones.js.org/collection/logos) or [mermaid architecture icons logos:s3](https://icon-sets.iconify.design/logos/)
      * https://github.com/gilbarbara/logos
    * [mermaid flowchart shapes](https://mermaid.js.org/syntax/flowchart.html#complete-list-of-new-shapes)
  * [fond awesome fa:fa-user](https://fontawesome.com/icons)
  * [zenuml](https://docs.zenuml.com/)
  * [kroki - collection of diagrams](https://kroki.io/)
* [markdown editor realtime collaboration](https://hackmd.io/)
* [list of markdown code supported languages](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml)

### regular expressions
* [regular expressions regexp](https://regex101.com)

### stream editor
* [sed escape, sed online escape](https://dwaves.de/tools/escape/)

### online coding
* [online editor with prepared UI frameworks](https://stackblitz.com/)
* [code compiler/editor online](https://www.jdoodle.com/)
* [code compiler/editor online](https://onecompiler.com/)
* [typescript sandbox](https://www.typescriptlang.org/)
* [repl online](https://replit.com/)
* [code sandbox](https://codesandbox.io/)

### code analyser
* [code lines counter](https://github.com/XAMPPRocky/tokei)
* [code secrets finder code passwords checker](https://github.com/sirwart/ripsecrets)

### code changer
* [auto code refactoring](https://docs.openrewrite.org/running-recipes/getting-started)

### database GUI client 
* [Azure Data Studio](https://azure.microsoft.com/products/data-studio)
* [DbGate](https://dbgate.org/)
* [Sqlectron](https://sqlectron.github.io/)
* [Antares SQL](https://antares-sql.app/)
* [Beekeeper Studio](https://www.beekeeperstudio.io/)

### database cli clients, sql cli tools, db connect from command line 
> https://java-source.net/open-source/sql-clients
* https://hsqldb.org
  [doc](https://hsqldb.org/doc/2.0/util-guide/sqltool-chapt.html#sqltool_sqlswitch-sect)
  [download](https://hsqldb.org/)

* [sqlshell](https://sqlshell.sourceforge.net/)
  [download](https://sourceforge.net/projects/sqlshell/)

* [henplus](https://github.com/neurolabs/henplus)
  
* [sqlline](https://github.com/julianhyde/sqlline)
  [sqlline doc](https://julianhyde.github.io/sqlline/manual.html)
  installation from source
  ```sh
  git clone https://github.com/julianhyde/sqlline.git
  cd sqlline
  git tag
  git checkout sqlline-1.12.0
  mvn package  
  ```
  download from maven 
  ```sh
  ver=1.12.0
  wget https://repo1.maven.org/maven2/sqlline/sqlline/$ver/sqlline-$ver-jar-with-dependencies.jar
  ```
  usage 
  ```sh
  java -cp "*" sqlline.SqlLine \
  -n myusername 
  -p supersecretpassword \
  -u "jdbc:oracle:thin:@my.host.name:1521:my-sid"
  ```
  usage with db2
  ```sh
  JDBC_SERVER_NAME=cat.zur
  JDBC_DATABASE=TOM_CAT
  JDBC_PORT=8355
  JDBC_USER=jerry
  JDBC_PASSWORD_PLAIN=mousejerry
  JDBC_DRIVER='com.ibm.db2.jcc.DB2Driver'
  java -cp "*" sqlline.SqlLine -n ${JDBC_USER} -p ${JDBC_PASSWORD_PLAIN} -u "jdbc:db2://${JDBC_SERVER_NAME}:${JDBC_PORT}/${JDBC_DATABASE}" -d $JDBC_DRIVER
  ```

### password storage
* [one time password storage](https://onetimesecret.com/)

### text/password exchange
* [online clipboard](https://copypaste.me/)

### [list of opensource api tools](https://openapi.tools/)

### Software test containers/emulators for development
* [Test Instances for communication with "prod-like containers"](https://testcontainers.com/modules/)

### REST api test frameworks
* [karate](https://github.com/karatelabs/karate)
    * [tutorial](https://www.softwaretestinghelp.com/api-testing-with-karate-framework/)
    * [how to start with](https://software-that-matters.com/2020/11/25/the-definitive-karate-api-testing-framework-getting-started-guide/)
* [k6](https://k6.io/docs/test-types/load-testing/)
* [cypress](https://step.exense.ch/resources/load-testing-with-cypress)
* [gatling](https://gatling.io/)
* junit
    * [how to junit](https://dzone.com/articles/how-we-do-performance-testing-easily-efficiently-a)
    * [junit how to](https://medium.com/@igorvlahek1/load-testing-with-junit-393a83261745)
* [locust](https://docs.locust.io/en/stable/writing-a-locustfile.html)
    * [how to locust](https://www.blazemeter.com/blog/locust-load-testing)
* [performance testing with traffic re-play](https://github.com/buger/goreplay)
