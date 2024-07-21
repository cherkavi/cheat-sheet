# Architecture cheat sheet

## Useful links
* [EIP](https://www.enterpriseintegrationpatterns.com/ramblings.html)
* [C4](https://leanpub.com/visualising-software-architecture)
* [C4 model](https://c4model.com/)
* 97 things every architect should know
* [value proposition template](https://www.strategyzer.com/canvas/value-proposition-canvas)
* [12 factors app](https://12factor.net/)
* [system design descriptions](https://github.com/ByteByteGoHq/system-design-101)
* [design patterns](https://github.com/DovAmir/awesome-design-patterns)
* Quality control ( intentionally in russian due to lack of information in other languages )
  **[семь инструментов контроля качества](https://stump.ru/types/sem-instrumentov-kachestva-sem-novyh-metodov-upravleniya-kachestvom/)**
  * [диаграмма ishikawa](https://ru.wikipedia.org/wiki/%D0%94%D0%B8%D0%B0%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B0_%D0%98%D1%81%D0%B8%D0%BA%D0%B0%D0%B2%D1%8B)
  * [контрольная карта](https://ru.wikipedia.org/wiki/%D0%9A%D0%BE%D0%BD%D1%82%D1%80%D0%BE%D0%BB%D1%8C%D0%BD%D0%B0%D1%8F_%D0%BA%D0%B0%D1%80%D1%82%D0%B0_%D0%A8%D1%83%D1%85%D0%B0%D1%80%D1%82%D0%B0)
  * [диаграмма Парето](https://ru.wikipedia.org/wiki/%D0%94%D0%B8%D0%B0%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B0_%D0%9F%D0%B0%D1%80%D0%B5%D1%82%D0%BE)
  * [гистограмма](https://ru.wikipedia.org/wiki/%D0%93%D0%B8%D1%81%D1%82%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B0)
  * [контрольный лист](https://ru.wikipedia.org/wiki/%D0%9A%D0%BE%D0%BD%D1%82%D1%80%D0%BE%D0%BB%D1%8C%D0%BD%D1%8B%D0%B9_%D1%81%D0%BF%D0%B8%D1%81%D0%BE%D0%BA)
  * [расслоение](https://ru.wikipedia.org/w/index.php?title=%D0%A0%D0%B0%D1%81%D1%81%D0%BB%D0%BE%D0%B5%D0%BD%D0%B8%D0%B5_(%D1%81%D1%82%D1%80%D0%B0%D1%82%D0%B8%D1%84%D0%B8%D0%BA%D0%B0%D1%86%D0%B8%D1%8F)&action=edit&redlink=1)
  * [диаграмма рассеяния](https://ru.wikipedia.org/wiki/%D0%94%D0%B8%D0%B0%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B0_%D1%80%D0%B0%D1%81%D1%81%D0%B5%D1%8F%D0%BD%D0%B8%D1%8F)
  * [дополнительные 7 инструментов контроля качества](https://dzen.ru/a/ZaQAos7VpGMZqodh)
    * диаграмма сродства
    * диаграмма зависимостей ( связей )
    * системная (древовидная) диаграмма
    * матричная диаграмма
    * стрелочная диаграмма
    * диаграмма планирования оценки процесса
    * анализ матричных данных
    * схема потока

## [books](https://www.goodreads.com/shelf/show/software-architecture)
* Clean Architecture ( Robet C. Martin )
* Domain-Driven Design: Tackling Complexity in the Heart of Software
* Software Architecture in Practice ( Len Bass )
* Patterns of Enterprise Application Architecture ( Martin Fowler )
* Enterprise Integration Patterns ( Gregor Hohpe )
* The Software Architect Elevator ( Grehor Hohpe )
* 97 Things Every Software Architect Should Know ( Richard Monson )
* 37 Things One Architect Knows ( Gregor Hohpe )
* Software Architecture Patterns ( Mark Richards )

## What architecture is:
* Architecture != Tools & Frameworks
* Postponing decisions about Tooling and frameworks
* Focus on customer, not environment

## Architecture trade-offs
* Build vs Buy
* Coding vs Configuration
* Product customisation

## Why to make a documentation:
```mermaid
mindmap
documentation)Documentation(
    {{one common vision}}
        common bird-eye view
        naming conventions
        decrease sketching time for each meeting
    {{part of DefinitionOfDone}}
        visibility
        knowledge sharing
    {{one point of truth}}
        new team member
        existing team memeber
        external teams
    {{interchange responsibility}}
        split hard tasks
        real Agile
```

![architecture phases](https://i.postimg.cc/brdDyd37/architecture-phases.png)
![architecture context](https://i.postimg.cc/3rG2VYKf/architecture-context.png)
![architecture styles](https://i.postimg.cc/5yD8PZcn/architecture-types.png)
![software architecture](https://i.postimg.cc/D0cMGPPc/software-architecture.png)
![architecture antipatterns](https://i.postimg.cc/Kz3gQFy8/architecture-antipatterns.png)


## Architecture cycle
![architecture cycle](https://i.postimg.cc/VNXSFVb1/architecture-cycle.png)

## Waterfall++ - [V-Model system design](https://en.wikipedia.org/wiki/V-model_(software_development))
![v-model](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e8/Systems_Engineering_Process_II.svg/599px-Systems_Engineering_Process_II.svg.png)

## Design thinking
* emphasize
  * hot points
  * who is asking
  * no tech discussions
  * goals ( business, tech )
* define
  * deep understanding
  * questions
  * use cases
* idea
  * how to 
  * solution architecture
* prototype
  * implementation
* test


## [Antipatterns](https://sourcemaking.com/antipatterns/software-architecture-antipatterns)

## Software architecture patterns
![architecture patterns](https://i.postimg.cc/Gm8T42L4/architecture-patterns.png)
> TODO: [cell-based Architecture](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md) (cellular architecture)

## Architecture Decision Records
* Priority
* Stakeholders
* Status
* Owners ( lead of activities )
* Assumptions
* Risks

## Togaf
[togaf phases](https://pubs.opengroup.org/togaf-standard/adm/chap03.html)    
![togaf phases](https://pubs.opengroup.org/togaf-standard/adm/Figures/a_vis.png)  
![base-target](https://i.postimg.cc/SxzC9f9z/togaf-base-target.png)  
![togaf layers](https://i.postimg.cc/904Rb3GK/archimate3-layers.png)    
![togaf aspects](https://i.postimg.cc/BbmmWbML/archimate-01.png)    
![togaf metamodel](https://i.postimg.cc/fLZCJmPS/archimate-04.png)    

## Archimate
### useful links
* [specification](https://pubs.opengroup.org/architecture/archimate3-doc/)
* [guide](https://www.visual-paradigm.com/guide/archimate/full-archimate-viewpoints-guide/)
* [how to draw diagrams](https://www.visual-paradigm.com/support/documents/vpuserguide/4455/4409/86421_howtodrawarc.html)
* [blog about modeling](http://renewableplus.blogspot.com/2017/03/modeling-applications-technology-in.html)
* [blog examples](https://www.hosiaisluoma.fi/blog/archimate-examples/)

### resilient architecture
* Timeouts
* Graceful Degradation
* Retries
* Exponential Backoff
* Circuit Breakers

### Archimate with Sequence diagram
![image](https://user-images.githubusercontent.com/8113355/206921007-21643c2b-da3f-4930-a79c-4462562017e1.png)

### Archimate with Class diagram 
![image](https://github.com/cherkavi/cheat-sheet/assets/8113355/a567541c-1535-4ad6-a837-087f4d807b8d)


### Examples
![Big Data cluster](https://github.com/cherkavi/cheat-sheet/assets/8113355/e44f9068-97d1-4e96-a2b7-acbf4f0edac5)


## UML diagrams
### Structure 
### Behavior
#### Behavior of itself
#### Behavior with other / Interaction 

## Application general schema, application demands
* development
* operations
* support
  * L1
  * L2
  * L3
* data migration ( for evolution/revolution )


## Architecture style: Microservice
### Microservices Benefits:
* Independent Deployments
* Fault Isolation
* Enchanced Scalability
### Microservices importance of design patterns:
* Scalability
* Reducing Complexity
* Distributed Data Management
* Enhancing Communication
### Microservices Design patterns:
* API Gateway
  > like GoF.facade
  > single entry point for all client requests
* Database per Service
  > like a GoF.memento ( partially )
  > single databank per service, data isolation
* Circuit Breaker
  > prevent overwhelming of the calls to outdated external resource
* Event-Driven
  > like GoF.observer
  > publish event ( notify ), when own state has changed
* Saga
  > like a GoF.command + GoF.proxy
  > for list of the external call will make undo in case of fail

## Communication between applications
### communication: messaging
![2git-messaging](https://github.com/cherkavi/cheat-sheet/assets/8113355/e3062328-7afb-4a8c-b9a7-b458689c5ed0)

