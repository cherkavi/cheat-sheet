# cheat sheet
[![Chat with Dosu](https://dosu.dev/dosu-chat-badge.svg)](https://app.dosu.dev/ask)

## other cheat sheets:
* [cheat sheet for enterprise architects](https://github.com/cherkavi/enterprise-architect-toolbox/tree/main/)
* [root/entrypoint to different resources](https://github.com/sindresorhus/awesome)
* [cheat sheets collection](https://lzone.de/cheat-sheet/)
* [cheat sheets](https://www.cheatography.com)

## other cheat-tools
* [cht.sh](https://github.com/chubin/cheat.sh)
* [tldr](https://tldr.sh/)
* [how2](https://how2terminal.com/download)

## [documentation examples, how to write good documentation, documentation tools](https://github.com/matheusfelipeog/beautiful-docs)

## landscapes/radars
* [cloud native landscape](https://landscape.cncf.io/)  
  built using an open-source tool called the **Interactive Landscape**
* [AI & Data Landscape](https://landscape.lfaidata.foundation/)
* [FinOps Landscape](https://landscape.finops.org/)
* [Continuous Delivery Foundation (CDF)](https://landscape.cd.foundation/)
* [OpenJS Foundation Landscape](https://landscape.openjsf.org/)
* [GraphQL Landscape](https://landscape.graphql.org/)

## useful tools

### useful search function for using whole cheat sheet
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

### CLI tools creation rules
* [clig guidelines](https://clig.dev)
* [Command Line Interface Guidelines](https://github.com/cli-guidelines/cli-guidelines)

### editor
* https://vscode.dev
* https://code.visualstudio.com/
* https://lite-xl.com/
* [editor colors](https://www.eggradients.com/cyan-colors)

### render/run html/js/javascript files from github

#### start locally markdown files as html
```sh
# sudo npm install markserv
markserv .
```
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
#### [ascii graphics for drawing Architecture Diagrams in text](http://asciiflow.com/)  
#### [uml, sysml, archimate tool](https://online.visual-paradigm.com/)
#### [mermaid console diagrams ](termaid.com)  
     https://github.com/fasouto/termaid
#### [diagrams within markdown - mermaid](https://mermaid.js.org/syntax/flowchart.html)
* [mermaid live](https://mermaid.live/), [mermaid playground](https://www.mermaidchart.com/play)
* [mermaid cli](https://github.com/mermaid-js/mermaid-cli)
  > Convert Mermaid mmd Diagram File To SVG
* [mermaid architecture icons logos:s3](https://icones.js.org/collection/logos) or [mermaid architecture icons logos:s3](https://icon-sets.iconify.design/logos/)
  * https://github.com/gilbarbara/logos
* [font awesome fa:fa-user](https://fontawesome.com/icons)
* [mermaid flowchart shapes](https://mermaid.js.org/syntax/flowchart.html#complete-list-of-new-shapes)
* [mermaid theme: default, neutral, dark, forest, base](https://mermaid.js.org/config/theming.html#available-themes)
* [markdown editor realtime collaboration](https://hackmd.io/)
```mermaid
---
config:
theme: 'default'
---
flowchart LR
```
#### [diagrams zenuml](https://docs.zenuml.com/)
#### [diagrams kroki ](https://kroki.io/)
#### [list of markdown code supported languages](https://github.com/github/linguist/blob/master/lib/linguist/languages.yml)
#### [symbols for showing on markdown sheet, dollar wrapped symbols](https://detexify.kirelabs.org/classify.html)

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


## 🌐 Web Resources & Aesthetic Symbols Index
- [KAOMOJI](https://balletcore-unicode-67.pages.dev/ru/kaomoji/)
- [SYM 1D40A](https://aesthetic-spacing-fonts-10.pages.dev/symbol/sym-1d40a/)
- [SYM 1FAE0](https://minimal-star-symbols-25.pages.dev/symbol/sym-1fae0/)
- [SYM 26E7](https://vintage-scholar-text-15.pages.dev/symbol/sym-26e7/)
- [CLOCKWISE OPEN CIRCLE ARROW](https://cyberpunk-clan-tags-43.pages.dev/symbol/clockwise-open-circle-arrow/)
- [SYM 1F974](https://ribbon-heart-fonts-86.pages.dev/symbol/sym-1f974/)
- [SYM 26AA](https://coquette-aesthetic-symbols-84.pages.dev/symbol/sym-26aa/)
- [SYM 26FB](https://baroque-font-vault-96.pages.dev/symbol/sym-26fb/)
- [SYM 1D40B](https://neon-glitch-symbols-84.pages.dev/symbol/sym-1d40b/)
- [SYM 1D452](https://mecha-blade-symbols-46.pages.dev/symbol/sym-1d452/)
- [SYM 1D41E](https://coquette-symbols.pages.dev/symbol/sym-1d41e/)
- [SYM 2615](https://theeduplaycampen.pages.dev/symbol/sym-2615/)
- [BIOHAZARD SYMBOL](https://raven-gothic-kaomoji-25.pages.dev/symbol/biohazard-symbol/)
- [SYM 1D488](https://mecha-blade-symbols-46.pages.dev/symbol/sym-1d488/)
- [DOWNWARD DIAGONAL ARROW](https://sleek-bio-symbols-51.pages.dev/symbol/downward-diagonal-arrow/)
- [SYM 26C0](https://gothic-bio-fonts-81.pages.dev/symbol/sym-26c0/)
- [SYM 265F](https://cyberpunk-clan-tags-43.pages.dev/symbol/sym-265f/)
- [SYM 2659](https://minimal-star-symbols-43.pages.dev/symbol/sym-2659/)
- [SYM 1D425](https://baroque-font-vault-96.pages.dev/symbol/sym-1d425/)
- [SYM 1D411](https://cyber-clan-tags-23.pages.dev/symbol/sym-1d411/)
- [SYM 2662](https://cyber-clan-tags-75.pages.dev/symbol/sym-2662/)
- [SYM 26B5](https://cyber-clan-tags-90.pages.dev/symbol/sym-26b5/)
- [SYM 1F634](https://mecha-blade-symbols-46.pages.dev/symbol/sym-1f634/)
- [SYM 273E](https://cyber-clan-tags-75.pages.dev/symbol/sym-273e/)
- [CHEERING FIGHTING FIST KAOMOJI](https://ribbon-heart-fonts-86.pages.dev/symbol/cheering-fighting-fist-kaomoji/)
- [ROBLOX NAMES](https://sleek-bio-symbols-40.pages.dev/pt/roblox-names/)
- [SYM 2676](https://mecha-synth-kaomoji-92.pages.dev/symbol/sym-2676/)
- [SYM 26EF](https://minimal-star-symbols-43.pages.dev/symbol/sym-26ef/)
- [SYM 1D40D](https://dolly-kaomoji-text-94.pages.dev/symbol/sym-1d40d/)
- [SAGITTARIUS ZODIAC ARCHER](https://mecha-synth-kaomoji-92.pages.dev/symbol/sagittarius-zodiac-archer/)
- [SYM 1D435](https://anime-sparkle-text-23.pages.dev/symbol/sym-1d435/)
- [MUSIC WEATHER](https://cyber-clan-tags-75.pages.dev/pt/music-weather/)
- [SKULL AND CROSSBONES](https://anime-sparkle-text-81.pages.dev/symbol/skull-and-crossbones/)
- [SYM 1F614](https://witchy-runic-text-71.pages.dev/symbol/sym-1f614/)
- [SIX POINTED BLACK STAR](https://clean-dot-aesthetic-48.pages.dev/symbol/six-pointed-black-star/)
- [SYM 1D460](https://cyber-clan-tags-75.pages.dev/symbol/sym-1d460/)
- [SYM 1D441](https://anime-sparkle-text-23.pages.dev/symbol/sym-1d441/)
- [BRACKETS](https://cyber-clan-tags-23.pages.dev/pt/brackets/)
- [SYM 1F642 200D 2195 FE0F](https://mecha-blade-symbols-46.pages.dev/symbol/sym-1f642-200d-2195-fe0f/)
- [SYM 268E](https://ribbon-heart-fonts-86.pages.dev/symbol/sym-268e/)
- [SYM 1F637](https://ribbon-heart-fonts-86.pages.dev/symbol/sym-1f637/)
- [SYM 1F92A](https://minimal-star-symbols-25.pages.dev/symbol/sym-1f92a/)
- [SYM 1D499](https://minimal-star-symbols-87.pages.dev/symbol/sym-1d499/)
- [HEARTS](https://vintage-scholar-text-15.pages.dev/ru/hearts/)
- [SYM 1F626](https://cyberpunk-clan-tags-43.pages.dev/symbol/sym-1f626/)
- [SYM 2680](https://dolly-kaomoji-text-94.pages.dev/symbol/sym-2680/)
- [RIGHT WING CLAN FLARE](https://neon-futuristic-symbols-58.pages.dev/symbol/right-wing-clan-flare/)
- [ZODIAC CELESTIAL](https://vintage-coquette-text-58.pages.dev/zodiac-celestial/)
- [SYM 1D475](https://theeduplaycampen.pages.dev/symbol/sym-1d475/)
- [SYM 1F921](https://vintage-angel-symbols-66.pages.dev/symbol/sym-1f921/)
- [SYM 2682](https://occult-aesthetic-symbols-26.pages.dev/symbol/sym-2682/)
- [SYM 1F62D](https://witchy-runic-text-71.pages.dev/symbol/sym-1f62d/)
- [TAURUS ZODIAC BULL](https://anime-sparkle-text-81.pages.dev/symbol/taurus-zodiac-bull/)
- [ANGEL WINGS HEART](https://coquette-symbols.pages.dev/symbol/angel-wings-heart/)
- [CRYING TEARS SAD KAOMOJI](https://angelic-bow-symbols-42.pages.dev/symbol/crying-tears-sad-kaomoji/)
- [SYM 2741](https://mecha-synth-kaomoji-92.pages.dev/symbol/sym-2741/)
- [SYM 26B9](https://vintage-angel-symbols-66.pages.dev/symbol/sym-26b9/)
- [SYM 1F914](https://coquette-aesthetic-symbols-84.pages.dev/symbol/sym-1f914/)
- [WINGED ANGELIC COQUETTE HEART](https://minimal-star-symbols-87.pages.dev/symbol/winged-angelic-coquette-heart/)
- [SYM 1D440](https://minimal-star-symbols-43.pages.dev/symbol/sym-1d440/)
- [SYM 1D49E](https://matrix-hacker-text-52.pages.dev/symbol/sym-1d49e/)
- [SYM 1D49E](https://coquette-aesthetic-symbols-84.pages.dev/symbol/sym-1d49e/)
- [SYM 1F635](https://vintage-angel-symbols-66.pages.dev/symbol/sym-1f635/)
- [SYM 1F630](https://clean-dot-aesthetic-48.pages.dev/symbol/sym-1f630/)
- [KAOMOJI](https://sleek-bio-symbols-51.pages.dev/kaomoji/)
- [BRACKETS](https://coquette-aesthetic-symbols-84.pages.dev/ru/brackets/)
- [SYM 1F633](https://cyber-clan-tags-90.pages.dev/symbol/sym-1f633/)
- [GAMING WEAPONS](https://matrix-glitch-text-37.pages.dev/gaming-weapons/)
- [GAMING WEAPONS](https://mecha-blade-symbols-46.pages.dev/ru/gaming-weapons/)
- [SYM 26D2](https://clean-aesthetic-fonts-33.pages.dev/symbol/sym-26d2/)
- [SYM 1D48D](https://matrix-hacker-text-52.pages.dev/symbol/sym-1d48d/)
- [ROBLOX NAMES](https://anime-sparkle-text-81.pages.dev/es/roblox-names/)
- [RIGHT HEAVY BRACKET BOX](https://mecha-blade-symbols-46.pages.dev/symbol/right-heavy-bracket-box/)
- [CROSSED SWORDS](https://baroque-font-vault-96.pages.dev/symbol/crossed-swords/)
- [SYM 1F63C](https://anime-sparkle-text-23.pages.dev/symbol/sym-1f63c/)
- [SYM 1D420](https://neon-futuristic-symbols-58.pages.dev/symbol/sym-1d420/)
- [SYM 1F63F](https://matrix-glitch-text-37.pages.dev/symbol/sym-1f63f/)
- [SYM 1D471](https://dark-literary-kaomoji-13.pages.dev/symbol/sym-1d471/)
- [SYM 26A9](https://cyber-clan-tags-23.pages.dev/symbol/sym-26a9/)
- [SYM 2746](https://clean-dot-aesthetic-48.pages.dev/symbol/sym-2746/)
- [SYM 1D435](https://minimal-star-symbols-43.pages.dev/symbol/sym-1d435/)
- [SYM 265D](https://sleek-bio-symbols-51.pages.dev/symbol/sym-265d/)
- [SYM 1D41E](https://clean-dot-aesthetic-48.pages.dev/symbol/sym-1d41e/)
- [LATIN CROSS HEAVY](https://clean-dot-aesthetic-48.pages.dev/symbol/latin-cross-heavy/)
- [SYM 1F621](https://zen-unicode-hub-94.pages.dev/symbol/sym-1f621/)
- [SYM 1F635 200D 1F4AB](https://monochrome-text-lab-86.pages.dev/symbol/sym-1f635-200d-1f4ab/)
- [SYM 1D439](https://matrix-hacker-text-52.pages.dev/symbol/sym-1d439/)
- [DISCORD STATUS](https://witchy-runic-text-71.pages.dev/vi/discord-status/)
- [SYM 1D43C](https://vintage-coquette-text-58.pages.dev/symbol/sym-1d43c/)
- [KAOMOJI](https://anime-sparkle-text-81.pages.dev/es/kaomoji/)
- [SYM 2745](https://mecha-synth-kaomoji-92.pages.dev/symbol/sym-2745/)
- [ANGEL WINGS HEART](https://anime-sparkle-text-81.pages.dev/symbol/angel-wings-heart/)
- [BLACK CENTRE STAR](https://anime-sparkle-text-81.pages.dev/symbol/black-centre-star/)
- [SYM 1F631](https://soft-bow-fonts-22.pages.dev/symbol/sym-1f631/)
- [SYM 1D44F](https://matrix-hacker-text-52.pages.dev/symbol/sym-1d44f/)
- [SYM 1D476](https://minimal-star-symbols-43.pages.dev/symbol/sym-1d476/)
- [SYM 1D491](https://gothic-bio-fonts-86.pages.dev/symbol/sym-1d491/)
- [TAURUS ZODIAC BULL](https://theeduplaycampen.pages.dev/symbol/taurus-zodiac-bull/)
- [CYBER PHANTOM GLYPH](https://mecha-blade-symbols-46.pages.dev/symbol/cyber-phantom-glyph/)
- [SYM 2738](https://vintage-angel-symbols-66.pages.dev/symbol/sym-2738/)
- [SYM 1D417](https://minimal-star-symbols-87.pages.dev/symbol/sym-1d417/)
- [LEFT WHITE CORNER BRACKET](https://angelic-bio-symbols-59.pages.dev/symbol/left-white-corner-bracket/)
- [SYM 1F631](https://cyber-clan-tags-75.pages.dev/symbol/sym-1f631/)
- [CUPID FEATHERY ARROW](https://cyber-clan-tags-90.pages.dev/symbol/cupid-feathery-arrow/)
- [SYM 1F498](https://anime-sparkle-text-23.pages.dev/symbol/sym-1f498/)
- [SYM 1F62E 200D 1F4A8](https://mecha-text-vault-91.pages.dev/symbol/sym-1f62e-200d-1f4a8/)
- [SYM 267E](https://dolly-kaomoji-text-94.pages.dev/symbol/sym-267e/)
- [SYM 26FF](https://minimal-star-symbols-87.pages.dev/symbol/sym-26ff/)
- [SYM 2668](https://vintage-scholar-text-15.pages.dev/symbol/sym-2668/)
- [SYM 1D428](https://minimal-star-symbols-43.pages.dev/symbol/sym-1d428/)
- [SYM 1F637](https://mecha-synth-kaomoji-92.pages.dev/symbol/sym-1f637/)
- [UPWARD DIAGONAL ARROW](https://mecha-synth-kaomoji-92.pages.dev/symbol/upward-diagonal-arrow/)
- [SYM 2744](https://matrix-glitch-text-37.pages.dev/symbol/sym-2744/)
- [SYM 1D445](https://minimal-star-symbols-43.pages.dev/symbol/sym-1d445/)
- [SYM 267C](https://dolly-kaomoji-text-94.pages.dev/symbol/sym-267c/)
- [SYM 1D400](https://occult-aesthetic-symbols-26.pages.dev/symbol/sym-1d400/)
- [SYM 1F604](https://soft-bow-fonts-22.pages.dev/symbol/sym-1f604/)
- [SYM 267F](https://occult-aesthetic-symbols-26.pages.dev/symbol/sym-267f/)
- [BAROQUE FONT VAULT 96.PAGES.DEV](https://baroque-font-vault-96.pages.dev/)
- [SYM 1D437](https://pastel-moe-emoticons-80.pages.dev/symbol/sym-1d437/)
- [SYM 273B](https://cyber-clan-tags-75.pages.dev/symbol/sym-273b/)
- [SYM 267D](https://gothic-bio-fonts-86.pages.dev/symbol/sym-267d/)
- [SYM 1D469](https://minimal-star-symbols-43.pages.dev/symbol/sym-1d469/)
- [SYM 1D486](https://pastel-manga-symbols-57.pages.dev/symbol/sym-1d486/)
- [FLORAL BRANCH BOUQUET](https://anime-sparkle-text-81.pages.dev/symbol/floral-branch-bouquet/)
- [AESTHETIC STARDUST COMBO](https://sleek-line-symbols-51.pages.dev/symbol/aesthetic-stardust-combo/)
- [MUSIC WEATHER](https://pearl-girly-fonts-86.pages.dev/ja/music-weather/)
- [SYM 1D458](https://clean-dot-aesthetic-48.pages.dev/symbol/sym-1d458/)
- [SYM 1D452](https://mecha-text-vault-91.pages.dev/symbol/sym-1d452/)
- [STARS](https://dolly-kaomoji-text-94.pages.dev/ja/stars/)
