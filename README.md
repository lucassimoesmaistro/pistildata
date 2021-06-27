# Banking Application!

The proposed solution presented here aims to meet the basic requirements involving financial transactions, such as selecting an account, viewing details such as balance and statement, and making transfers.

# Solution Architecture

To meet high availability and performance requirements, Azure Front Door allows you to create geographic routing policies in addition to offering options for monitoring.
The idea is to make use of two AKS clusters in to provision both the APIs and the SPA application and its micro front-ends. With API Managment will be created traffic policies and caching in Redis enabling an even more scalable experience.
For databases, the strategy is a combination of relational databases for each micro service, maintaining the integrity of the operation data, for reading databases and even for the Event Store, the use would be with CosmosDB each in the same region where the micro services are hosted.

![](./doc/architecture.png)

# User Experience
