##1 
Monolithic Architecture is a software architecture where all the application modules, such as authentication, users, products, orders and payments, are developed as part of a single application.

We can organize these modules into separate folders and services, but they belong to one backend application and are deployed together.

Modules generally communicate through direct function calls, which is known as in-process communication.

Its main advantages are simple development, easy deployment and less infrastructure complexity.

Its disadvantages include difficulty in independently scaling modules, tightly coupled components and the need to redeploy the entire application for most changes.


