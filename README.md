# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create SubscriberRequest model struct.`
    -   [x] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [x] Commit: `Implement add function in Notification repository.`
    -   [x] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Commit: `Implement receive_notification function in Notification service.`
    -   [x] Commit: `Implement receive function in Notification controller.`
    -   [x] Commit: `Implement list_messages function in Notification service.`
    -   [x] Commit: `Implement list function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

1. Q: In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?  
   A: The use of RwLock<> is crucial in this context because it allows multiple threads to read from the notifications vector concurrently, which improves performance during read-heavy operations. This concurrency is essential as the application often needs to display or log notifications without requiring modification. When a write operation is necessary, RwLock<> automatically ensures exclusive access, preventing any data races. In contrast, a Mutex<> would force a single thread to access the vector at any time, even for simple read operations, leading to unnecessary performance bottlenecks. This design follows the principle of optimizing for the common case, which in our app is reading. Overall, RwLock<> provides a balanced approach by enhancing read performance while still ensuring thread safety during writes.

2. Q: In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?  
   A: Rust enforces strict safety and concurrency rules, and as a result, static variables are immutable by default to prevent data races and undefined behaviors. This immutability ensures that once data is set, it remains consistent across the entire program unless explicitly managed. To allow mutation, Rust requires the use of safe concurrency primitives such as RwLock or Mutex, ensuring that any access to mutable static data is properly synchronized. Unlike Java, which permits inherent mutability on static variables, Rust necessitates these extra steps to maintain memory safety and prevent potential runtime errors. The lazy_static library aids by providing a safe way to initialize global data once while still enforcing these safety constraints. In summary, Rust’s approach results in more robust and predictable code even when dealing with shared mutable state.

#### Reflection Subscriber-2

1. Q: Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.
   A: I've tried exploring files beyond the basic tutorial steps, including src/lib.rs and other related modules. By reviewing these files, I gained a deeper understanding of how the project initializes its global configuration and how the various components such as models, repositories, and services interconnect. I learned about the use of lazy_static for safely initializing global state and how Rocket’s built-in functionality is leveraged to set up the server efficiently. Additionally, this exploration exposed me to the clear separation of concerns implemented in the architecture, which has significantly enhanced my comprehension of scalable application design. This process inspired me to continually seek out ways to improve project extensibility through careful code organization. Overall, the insights I gained have positively influenced my approach to building robust web applications.

2. Q: Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?
   A: The Observer pattern significantly simplifies the process of adding more subscribers by keeping the publisher and subscriber implementations decoupled. Each Receiver instance can subscribe independently via its own configuration, meaning I only need to update the .env file to run instances on ports 8001, 8002, and 8003. This modular approach ensures that the Main app does not need alterations irrespective of the increasing number of subscribers. Even with the deployment of multiple Main app instances, each one interacts with subscribers consistently due to the clear separation of concerns provided by the Observer pattern. This flexibility makes horizontal scalability straightforward as new instances can seamlessly integrate into the system without impacting its core functionality. Ultimately, this design pattern fosters a highly maintainable and scalable architecture.

3. Q: Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).
   A: I've tried creating my own tests and enhancing the documentation within the Postman collection to better understand and validate the endpoints. Building automated tests allowed me to repeatedly verify that the notification service behaved correctly under a variety of scenarios, which proved to be invaluable for ensuring system reliability. Improving the Postman documentation further clarified the purpose and usage of each endpoint, making it easier for team members to collaborate and maintain the project. This effort also served as a useful reference during debugging and while integrating additional features. The process of testing and documentation enhancement has boosted my confidence in the system's overall stability and has contributed significantly to a smoother development workflow. Overall, these initiatives have shown great promise in improving both tutorial work and potential group project contributions.