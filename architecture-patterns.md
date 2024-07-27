# Architecture patterns
## [Antipatterns](https://sourcemaking.com/antipatterns/software-architecture-antipatterns)
![architecture antipatterns](https://i.postimg.cc/Kz3gQFy8/architecture-antipatterns.png)

## Software architecture patterns
![architecture patterns](https://i.postimg.cc/Gm8T42L4/architecture-patterns.png)
> TODO: [cell-based Architecture](https://github.com/wso2/reference-architecture/blob/master/reference-architecture-cell-based.md) (cellular architecture)

## Architecture cycle/phases
![architecture phases](https://i.postimg.cc/brdDyd37/architecture-phases.png)

# Architecture styles
![architecture styles](https://i.postimg.cc/5yD8PZcn/architecture-types.png)
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


# Communication between applications
## File System 
## Shared DB
## RMI
## messaging
![2git-messaging](https://github.com/cherkavi/cheat-sheet/assets/8113355/e3062328-7afb-4a8c-b9a7-b458689c5ed0)

