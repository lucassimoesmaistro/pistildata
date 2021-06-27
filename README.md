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

![](./doc/Backend-architecture.png)

 1. Person:
 Onboarding process, KYC, segmentation.

 2. Account:
 Account management, one customer may have many account in this scenario

 3. Transation:
 Responsible for the SAGA process of the transfer an asset to another account. 

 In this SAGA is necessary check the balance of the source account, check how to exchange source currency to the destination currency, compute fee, the liquidit necessary to make this exchange.

![](./doc/saga.png)

Another point is the date of the transaction, all date are stores in timestamp type on UTC, but displayed based on the location of the user.

 4. Currency Exchange:
 Responsible for the flow, tax, rate, liquidity account and rules to convert a currency in another currency.

 5. Reconciliation:
 Responsible for compute the balance and limits for each account based on the transactions.

# HTTP route strategy

 1. Get Customer Data:
```http
GET: /customer/{hashencoded}
```

 2. Get Customer Accounts:
```http
GET: /customer/{hashencoded}/account
```

 3. Get Customer Account Data:
```http
GET: /customer/{hashencoded}/account/{hashencoded}
```

 4. Get Customer Account Statement:
```http
GET: /customer/{hashencoded}/account/{hashencoded}/statement
```

 5. Get Customer Account Deposit Data:
```http
GET: /customer/{hashencoded}/account/{hashencoded}/Deposit
```

 6. Get Initial Transfer Data:
```http
GET: /customer/{hashencoded}/account/{hashencoded}/transfer
```
response:
```json
    {
        "balances": [
            {
                "currency": "USD", "amount": 100.00
            },
            {
                "currency": "CAD", "amount": 100.00
            }
        ]
    }
```

 7. Validate exchange is possible:
```http
PATCH: /customer/{hashencoded}/account/{hashencoded}/transfer
```

response:

```json
    {
        "op": "exchange",
        "path": "",
        "value":{
            "transaction": {
                "currencySource": "USD",
                "currencyDestionation": "USD",
                "amount": 100.00,
            }
        }
    }
```

 8. Create a Transfer:

```http
POST: /customer/{hashencoded}/account/{hashencoded}/transfer
```
response:

```json
    {
        "currencySource": "USD",
        "accountDestionation": "e4d539db-c22f-493c-a4df-456f3798b023",
        "amount": 100.00,
    }
```

# Database Schema
```c#
    //todo
```

# Epics
```c#
    //todo
```
# Code Snipped
The code I would like to show are my domain classes. I understand that the domain classes should be responsible for their state and for that I use TDD as a development strategy so that I can guarantee that the code meets the rules that were first mapped.

[Domain Class](https://github.com/lucassimoesmaistro/Keezag.HHSurge/blob/master/src/Keezag.HHSurge.Domain/User.cs)

Unfortunately, I cannot present projects that I have developed professionally for obvious reasons of intellectual property and confidentiality, as they are vastly more complex than what I have to show.