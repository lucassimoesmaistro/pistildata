# Banking Application!

The proposed solution presented here aims to meet the basic requirements involving financial transactions, such as selecting an account, viewing details such as balance and statement, and making transfers.

# Solution Architecture

To meet high availability and performance requirements, Azure Front Door allows you to create geographic routing policies in addition to offering options for monitoring.
The idea is to make use of two AKS clusters in to provision both the APIs and the SPA application and its micro front-ends. With API Managment will be created traffic policies and caching in Redis enabling an even more scalable experience.
For databases, the strategy is a combination of relational databases for each micro service, maintaining the integrity of the operation data, for reading databases and even for the Event Store, the use would be with CosmosDB each in the same region where the micro services are hosted.

![](./doc/architecture.png)

# User Experience

The application aims to provide quick access to features.

 1. Main Page

![](./doc/wireframes/wireframe_main.PNG)

 2. Deposit

![](./doc/wireframes/wireframe_deposit.PNG)

 3. Withdraw

![](./doc/wireframes/wireframe_withdraw.PNG)
 
 4. Transfer

![](./doc/wireframes/wireframe_transfer.PNG)
 
 5. Statement

![](./doc/wireframes/wireframe_statement.PNG)

# Front-End Macro Architecture

The front-end solution should be divided into a micro front-end orchestrator and the micro front-ends that will compose the SPA application.

 1. Orchestrator
 - verticalComponent: spot for menu
 - horizontalComponent: spot for breadcrumb and header
 - mainComponent: spot for render micro front-ends

 2. Menu (Micro Front-end)
 - itemComponent
 - listItemsComponent

 3. Statement (Micro Front-end)
 - itemComponent
 - listItemsComponent
 - filterComponent

 4. Deposit (Micro Front-end)
 - qrCodeComponent
 - dataComponent

 5. Withdraw (Micro Front-end)
 - qrCodeComponent
 - dataComponent
 - cameraComponent
 - amountComponent

 5. Tranfer (Micro Front-end)
 - balanceComponent
 - fromComponent (must include balanceComponent)
 - toComponent
 - confirmationComponent

# Back-End Architecture
In the backend 5 domains will compose the ecosystem of APIs and a 6th one will work as an anti-corruption layer meeting only the demands to load the Query Stack.

 1. Person:
 Onboarding process, KYC, segmentation.

 2. Account:
 Account management, one customer may have many account in this scenario

 2. Transation:
 Responsible for the SAGA process of the transfer an asset to another account. 
 In this SAGA is necessary check the balance of the source account, check how to exchange source currency to the destination currency, compute fee, the liquidit necessary to make this exchange.
 ```mermaid
graph LR
A((Validate)) --> B{Check Balance}
B -- Enough --> C{Currencies is convertible}
B --> D((Finish Saga))
C -- Yes --> E[Start Saga]
C -- No --> D((Finish Saga))
E[Start Saga] --> F[Registry Statement]
E[Start Saga] --> G[Exchange Currency]
G[Exchange Currency] --> H[Complete Transaction]
H[Complete Transaction] --> I((Finish Saga))
```
Another point is the date of the transaction, all date are stores in timestamp type on UTC, but displayed based on the location of the user.

# HTTP route strategy

# Database Schema

# Epics

# Code Snipped