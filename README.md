# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
> In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?
- For this case, I think it's not necessary to use an interface (trait) and it's enough to use a single model struct. Why? Because there's only one observer, which is the Subscriber class. Using an interface would be useful when we have many observers of different types.
> id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?
- In my opinion, using DashMap is the right choice. If we were to use a Vec, we would need to create two separate arrays to store different id and url values, and we would have to iterate through the array to find the matching id-url pair. By using DashMap, we can store id and url in a single structure, making it easier to use. Lastly, DashMap supports concurrent access, making it safer to use if the application is deployed in a multi-threaded environment in the future.
> When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?
- The BambangShop application uses multi-threading, which is why we should use DashMap instead of the Singleton pattern. Why? Because DashMap is a thread-safe HashMap that supports concurrent access. This allows the SUBSCRIBERS data to be accessed concurrently without any issues. If we use the Singleton pattern approach, the object only has a single instance throughout the program’s execution. In a multi-threaded environment, this would require us to implement locking on the object. However, this adds complexity to the program and increases the risk of deadlock.
#### Reflection Publisher-2
> In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?
- To ensure easier maintainability and promote modularity and scalability, we follow the Single Responsibility principle, where a class should have only one responsibility. By separating the Service and Repository from the Model, we can distinguish between business logic and data access, making the code easier to test and debug.
> What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?
- The impact is that the code will become more complex and harder to maintain. The coupling between the two classes will increase, meaning that if a change is made, it could lead to many other changes, unlike when we separate business logic from data access.
> Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.
- With Postman, I can perform API Testing. I can view the results of the requests I send and ensure that the responses I receive match my expectations. One of its supporting features is Collection, which allows me to group requests into folders, and Environment lets me store frequently used variables.

#### Reflection Publisher-3
> Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?
- The variation of the Observer Pattern used in this tutorial is the Push Model because the Publisher sends notifications to all of its Subscribers whenever a CRUD operation occurs on a Product, using the notify method in the NotificationService.
> What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)
- For the Pull Model case:
- Advantages: The Observer/Subscriber has the ability to determine what notifications they want to receive from the Publisher, providing greater flexibility.
- Disadvantages: The Observer/Subscriber needs to be active. If not, they might miss out on desired notifications because they have to perform a manual pull themselves (manual pull).
> Explain what will happen to the program if we decide to not use multi-threading in the notification process.
- In my opinion, the process will run sequentially and will take a long time. The bottleneck lies in the Publisher sending notifications to all Subscribers in the NotificationService class through the notify() method. As a result, the notification delivery must be completed first before any other processes can continue.
