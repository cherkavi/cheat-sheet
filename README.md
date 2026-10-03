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

### voice input
* [offline voice recognizer](https://github.com/cjpais/handy)

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
- [TAURUS ZODIAC BULL](https://manga-bubble-symbols-54.pages.dev/symbol/taurus-zodiac-bull/)
- [SYM 273B](https://coquette-heart-text-40.pages.dev/symbol/sym-273b/)
- [SYM 26F8](https://matrix-glitch-text-84.pages.dev/symbol/sym-26f8/)
- [BORDERS DIVIDERS](https://glitch-matrix-fonts-28.pages.dev/ja/borders-dividers/)
- [SYM 1FAE4](https://mystic-occult-fonts-26.pages.dev/symbol/sym-1fae4/)
- [SYM 1F47B](https://coquette-heart-text-40.pages.dev/symbol/sym-1f47b/)
- [SYM 26E8](https://zen-typography-hub-86.pages.dev/symbol/sym-26e8/)
- [SYM 1D427](https://moe-star-kaomoji-60.pages.dev/symbol/sym-1d427/)
- [SYM 1D43F](https://aesthetic-bullet-points-76.pages.dev/symbol/sym-1d43f/)
- [RIGHT HEAVY BRACKET BOX](https://academic-rune-text-25.pages.dev/symbol/right-heavy-bracket-box/)
- [SYM 2681](https://baroque-curse-text-56.pages.dev/symbol/sym-2681/)
- [BRACKETS](https://techno-hacker-text-43.pages.dev/pt/brackets/)
- [SYM 1F613](https://minimal-star-symbols-43.pages.dev/symbol/sym-1f613/)
- [RIGHT WING CLAN FLARE](https://sleek-typography-hub-12.pages.dev/symbol/right-wing-clan-flare/)
- [SYM 1D44D](https://pearl-heart-symbols-95.pages.dev/symbol/sym-1d44d/)
- [SYM 26B7](https://dark-scholarly-symbols-65.pages.dev/symbol/sym-26b7/)
- [SYM 2639 FE0F](https://sleek-typography-hub-12.pages.dev/symbol/sym-2639-fe0f/)
- [BORDERS DIVIDERS](https://pastel-moe-emoticons-55.pages.dev/ja/borders-dividers/)
- [SYM 2673](https://fairy-lace-symbols-92.pages.dev/symbol/sym-2673/)
- [SYM 1D4A3](https://alchemy-occult-symbols-55.pages.dev/symbol/sym-1d4a3/)
- [SYM 1F613](https://anime-sparkle-text-81.pages.dev/symbol/sym-1f613/)
- [SYM 1F929](https://mecha-hacker-kaomoji-26.pages.dev/symbol/sym-1f929/)
- [BLACK FOUR POINT STAR](https://scholarly-unicode-vault-92.pages.dev/symbol/black-four-point-star/)
- [SYM 1D42A](https://dolly-angel-fonts-14.pages.dev/symbol/sym-1d42a/)
- [JA](https://glitch-matrix-fonts-28.pages.dev/ja/)
- [TRENDING](https://zen-unicode-symbols-89.pages.dev/vi/trending/)
- [SYM 263A FE0F](https://cyber-clan-tags-20.pages.dev/symbol/sym-263a-fe0f/)
- [TRENDING](https://coquette-heart-text-40.pages.dev/vi/trending/)
- [SYM 1F607](https://sleek-bio-fonts-25.pages.dev/symbol/sym-1f607/)
- [SYM 1D44C](https://lace-bow-symbols-18.pages.dev/symbol/sym-1d44c/)
- [SYM 1D481](https://simple-line-fonts-11.pages.dev/symbol/sym-1d481/)
- [SYM 2742](https://pastel-moe-emoticons-55.pages.dev/symbol/sym-2742/)
- [SYM 2640](https://manga-speech-symbols-95.pages.dev/symbol/sym-2640/)
- [SYM 26E9](https://minimal-star-symbols-95.pages.dev/symbol/sym-26e9/)
- [ARROWS LINES](https://chibi-emoticon-world-87.pages.dev/ja/arrows-lines/)
- [SYM 1F605](https://clean-aesthetic-arrows-99.pages.dev/symbol/sym-1f605/)
- [SYM 2660](https://minimal-star-symbols-91.pages.dev/symbol/sym-2660/)
- [SYM 1F496](https://angelic-bio-symbols-90.pages.dev/symbol/sym-1f496/)
- [BRACKETS](https://gothic-bio-fonts-69.pages.dev/ja/brackets/)
- [RIGHT HEAVY BRACKET BOX](https://matrix-terminal-fonts-30.pages.dev/symbol/right-heavy-bracket-box/)
- [SYM 1D441](https://sleek-typography-hub-12.pages.dev/symbol/sym-1d441/)
- [SYM 26A4](https://anime-sparkle-text-58.pages.dev/symbol/sym-26a4/)
- [SYM 26C6](https://matrix-glitch-text-59.pages.dev/symbol/sym-26c6/)
- [DISCORD STATUS](https://angelic-bio-symbols-90.pages.dev/vi/discord-status/)
- [BEAMED EIGHTH NOTES](https://coquette-aesthetic-symbols-58.pages.dev/symbol/beamed-eighth-notes/)
- [SYM 1F628](https://coquette-heart-text-40.pages.dev/symbol/sym-1f628/)
- [SYM 1D483](https://zen-unicode-text-24.pages.dev/symbol/sym-1d483/)
- [COQUETTE BOW RIBBON](https://vintage-bow-kaomoji-63.pages.dev/symbol/coquette-bow-ribbon/)
- [SYM 26CF](https://sleek-typography-hub-12.pages.dev/symbol/sym-26cf/)
- [SYM 265A](https://coquette-aesthetic-symbols-63.pages.dev/symbol/sym-265a/)
- [SYM 1D40C](https://classic-literature-symbols-64.pages.dev/symbol/sym-1d40c/)
- [SYM 1F631](https://sleek-typography-hub-12.pages.dev/symbol/sym-1f631/)
- [SYM 26F3](https://soft-angel-symbols-33.pages.dev/symbol/sym-26f3/)
- [SYM 1F49E](https://clean-line-emojis-93.pages.dev/symbol/sym-1f49e/)
- [KAOMOJI](https://chibi-emoticon-vault-78.pages.dev/ru/kaomoji/)
- [SYM 1F63D](https://chibi-heart-symbols-15.pages.dev/symbol/sym-1f63d/)
- [SYM 1F60C](https://coquette-aesthetic-symbols-84.pages.dev/symbol/sym-1f60c/)
- [SYM 1D478](https://chibi-emoticon-vault-78.pages.dev/symbol/sym-1d478/)
- [SYM 1F498](https://moe-star-emoticons-13.pages.dev/symbol/sym-1f498/)
- [SYM 1D416](https://chibi-bunny-symbols-82.pages.dev/symbol/sym-1d416/)
- [SYM 2662](https://cyber-clan-tags-68.pages.dev/symbol/sym-2662/)
- [LEO ZODIAC LION](https://cyber-clan-tags-80.pages.dev/symbol/leo-zodiac-lion/)
- [LEFT BLACK LENTICULAR BRACKET](https://sleek-typography-hub-12.pages.dev/symbol/left-black-lenticular-bracket/)
- [NATURE FLOWERS](https://cyber-clan-tags-69.pages.dev/nature-flowers/)
- [SYM 1F627](https://gothic-bio-fonts-87.pages.dev/symbol/sym-1f627/)
- [SYM 26C5](https://minimal-star-symbols-43.pages.dev/symbol/sym-26c5/)
- [SYM 1F496](https://cyber-clan-tags-69.pages.dev/symbol/sym-1f496/)
- [SYM 1D453](https://anime-sparkle-text-73.pages.dev/symbol/sym-1d453/)
- [SYM 1D403](https://anime-sparkle-text-92.pages.dev/symbol/sym-1d403/)
- [SYM 1F642 200D 2195 FE0F](https://anime-sparkle-text-76.pages.dev/symbol/sym-1f642-200d-2195-fe0f/)
- [BRACKETS](https://bow-ribbon-symbols-72.pages.dev/ru/brackets/)
- [SYM 1D437](https://scholarly-runes-text-90.pages.dev/symbol/sym-1d437/)
- [SYM 1D480](https://clean-line-dividers-65.pages.dev/symbol/sym-1d480/)
- [LEFT WHITE CORNER BRACKET](https://synth-crosshair-text-47.pages.dev/symbol/left-white-corner-bracket/)
- [HEAVY RIGHTWARD ARROW](https://sleek-arrow-symbols-53.pages.dev/symbol/heavy-rightward-arrow/)
- [SYM 1F63F](https://coquette-aesthetic-symbols-96.pages.dev/symbol/sym-1f63f/)
- [SYM 2728](https://mecha-terminal-text-63.pages.dev/symbol/sym-2728/)
- [SYM 2628](https://sleek-typography-hub-12.pages.dev/symbol/sym-2628/)
- [SYM 2764 FE0F 200D 1F525](https://mecha-hacker-kaomoji-26.pages.dev/symbol/sym-2764-fe0f-200d-1f525/)
- [SYM 1F92E](https://sleek-typography-hub-12.pages.dev/symbol/sym-1f92e/)
- [PISCES ZODIAC FISHES](https://sparkly-chibi-symbols-47.pages.dev/symbol/pisces-zodiac-fishes/)
- [SYM 1D423](https://vintage-script-symbols-11.pages.dev/symbol/sym-1d423/)
- [SYM 1F49E](https://dainty-heart-kaomoji-75.pages.dev/symbol/sym-1f49e/)
- [SYM 1F636](https://scholarly-gothic-text-63.pages.dev/symbol/sym-1f636/)
- [SYM 2763 FE0F](https://minimal-star-symbols-17.pages.dev/symbol/sym-2763-fe0f/)
- [NATURE FLOWERS](https://minimal-star-symbols-43.pages.dev/ja/nature-flowers/)
- [LAST QUARTER CRESCENT MOON](https://mecha-hacker-kaomoji-26.pages.dev/symbol/last-quarter-crescent-moon/)
- [SYM 1D45F](https://neon-hacker-fonts-47.pages.dev/symbol/sym-1d45f/)
- [SYM 262F](https://minimal-star-symbols-17.pages.dev/symbol/sym-262f/)
- [SYM 1D49B](https://neon-hacker-fonts-47.pages.dev/symbol/sym-1d49b/)
- [SYM 1D478](https://neon-hacker-fonts-47.pages.dev/symbol/sym-1d478/)
- [SYM 1D426](https://clean-unicode-borders-23.pages.dev/symbol/sym-1d426/)
- [AESTHETIC MINIMAL CLOUD](https://angelic-heart-symbols-57.pages.dev/symbol/aesthetic-minimal-cloud/)
- [SYM 1D434](https://chibi-bunny-symbols-82.pages.dev/symbol/sym-1d434/)
- [SYM 1D424](https://mecha-terminal-text-63.pages.dev/symbol/sym-1d424/)
- [SYM 1D437](https://anime-sparkle-text-89.pages.dev/symbol/sym-1d437/)
- [SYM 1F493](https://sleek-typography-hub-12.pages.dev/symbol/sym-1f493/)
- [SYM 1F49A](https://coquette-aesthetic-symbols-65.pages.dev/symbol/sym-1f49a/)
- [SYM 260A](https://dainty-heart-kaomoji-75.pages.dev/symbol/sym-260a/)
- [WHITE FLORETTE BLOSSOM](https://dainty-heart-kaomoji-75.pages.dev/symbol/white-florette-blossom/)
- [SYM 26FE](https://angelic-bio-symbols-90.pages.dev/symbol/sym-26fe/)
- [SYM 1D485](https://neon-hacker-fonts-47.pages.dev/symbol/sym-1d485/)
- [SYM 2673](https://coquette-aesthetic-symbols-65.pages.dev/symbol/sym-2673/)
- [SYM 1D43E](https://synthwave-glitch-text-94.pages.dev/symbol/sym-1d43e/)
- [SCORPIO ZODIAC SCORPION](https://vintage-bow-fonts-72.pages.dev/symbol/scorpio-zodiac-scorpion/)
- [SYM 262A](https://clean-aesthetic-arrows-99.pages.dev/symbol/sym-262a/)
- [SYM 1D432](https://angelic-bio-symbols-90.pages.dev/symbol/sym-1d432/)
- [SIXTEEN POINTED STAR](https://neon-glitch-fonts-26.pages.dev/symbol/sixteen-pointed-star/)
- [SYM 1F923](https://glitch-mecha-kaomoji-69.pages.dev/symbol/sym-1f923/)
- [FREEFIRE NAMES](https://moe-soft-emoticons-41.pages.dev/freefire-names/)
- [SYM 26A2](https://vintage-lace-symbols-65.pages.dev/symbol/sym-26a2/)
- [FREE FIRE CLAN EMPEROR CROWN](https://chibi-bunny-symbols-82.pages.dev/symbol/free-fire-clan-emperor-crown/)
- [SYM 2639 FE0F](https://minimal-star-symbols-17.pages.dev/symbol/sym-2639-fe0f/)
- [SYM 26F4](https://vintage-library-text-15.pages.dev/symbol/sym-26f4/)
- [SYM 1F624](https://aesthetic-spacing-fonts-10.pages.dev/symbol/sym-1f624/)
- [SYM 1D498](https://scholarly-runes-text-68.pages.dev/symbol/sym-1d498/)
- [ZODIAC CELESTIAL](https://minimal-star-symbols-95.pages.dev/es/zodiac-celestial/)
- [SYM 1D457](https://neon-hacker-fonts-47.pages.dev/symbol/sym-1d457/)
- [SYM 1F607](https://mecha-hacker-kaomoji-26.pages.dev/symbol/sym-1f607/)
- [SYM 1D42E](https://coquette-aesthetic-symbols-65.pages.dev/symbol/sym-1d42e/)
- [SYM 1FAE4](https://soft-angel-symbols-21.pages.dev/symbol/sym-1fae4/)
- [HEAVY HEART EXCLAMATION](https://zen-arrow-symbols-99.pages.dev/symbol/heavy-heart-exclamation/)
- [SYM 26C8](https://sleek-typography-hub-12.pages.dev/symbol/sym-26c8/)
- [LEFT MATHEMATICAL WHITE SQUARE BRACKET](https://glitch-mecha-kaomoji-69.pages.dev/symbol/left-mathematical-white-square-bracket/)
- [SYM 1D46A](https://clean-unicode-borders-23.pages.dev/symbol/sym-1d46a/)
- [SYM 1F976](https://mystic-rune-text-88.pages.dev/symbol/sym-1f976/)
- [SYM 1F63F](https://kawaii-kaomoji-hub-14.pages.dev/symbol/sym-1f63f/)
- [SYM 1D47B](https://angelic-heart-symbols-57.pages.dev/symbol/sym-1d47b/)
- [SYM 26BE](https://kawaii-kaomoji-hub-47.pages.dev/symbol/sym-26be/)
- [STAR OPERATOR](https://zen-unicode-hub-94.pages.dev/symbol/star-operator/)
