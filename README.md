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
-   **STAGE 2: Implement services and controllers**
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
1. In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?

    Penggunaan RwLock<> diperlukan pada kasus ini karena dapat memfasilitasi akses data yang sama secara bersamaan, namun kita ingin membatasi akses tersebut. RwLock<> memungkinkan kita untuk mengakses data secara bersamaan, namun hanya satu thread yang dapat mengakses data tersebut pada satu waktu. Mutex<> tidak digunakan karena hanya memungkinkan satu thread untuk mengakses data pada satu waktu tertentu. Dengan demikian, Mutex<> tidak cocok untuk digunakan pada kasus ini karena vecttor notifications akan dibaca oleh banyak thread.

2. In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?

    Berbeda dengan java, rust secara default tidak mengizinkan variabel static untuk diubah datanya (immutable). Hal ini bertujuan untuk mencegah terjadinya race condition. Untuk mengatasi ini, Rust menggunakan lazy static yang memungkinkan definisi variabel static yang mutable dengan aman menggunakan mekanisme seperti Mutex atau RwLock, yang mengelola akses ke variabel tersebut. Lazy static memungkinkan penggunaan variabel static dengan cara yang mirip dengan java, namun tetap aman sesuai dengan model concurrency pada rust.

#### Reflection Subscriber-2
1. Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

    Saya telah melihat bagian kode src/lib.rs. Di dalam src/lib.rs, terdapat beberapa elemen penting seperti konfigurasi dan dependensi aplikasi. Selain itu terdapat inisialisasi variablel static di lazy_static, misalnya REQWEST_CLIENT dan APP_CONFIG yang dipakai dalam mengimplementasi kode. Selain itu, ada fungsi compose_error_response untuk pesan error kustom. Informasi yang diperlukan untuk aplikasi, seperti manajemen error dengan Error sebagai default untuk tipe Result dan ErrorResponse untuk respons kesalahan, juga telah disediakan. Jadi secara keseluruhan, src/lib.rs berfungsi untuk mendefinisikan komponen-komponen yang diperlukan dalam aplikasi.

2. Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?

    Observer Pattern memudahkan saya dalam menambahkan subscriber ke sistem notifikasi dengan cara mendaftarkan mereka menggunakan method subscribe dan memasukkan mereka ke dalam daftar subscriber, yang membuat code architecture menjadi lebih fleksibel dan scalable. Hal ini karena di dalam Observer pattern dilakukan pemisahan antara publisher dan observernya. Spawning lebih dari 1 instance Main app akan mudah dilakukan karena terdapat parallel processing untuk notificationnya, sehingga membuat kode lebih efisien. Ketika ada subscriber baru yang berlangganan, mereka akan ditambahkan ke dalam daftar observer. Misalnya, jika instance dari Main app di spawn, tidak akan ada masalah karena proses pendaftaran subscriber akan disesuaikan dengan API yang relevan.


3. Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

    Ya, saya sudah mencobanya. Menurut saya, Postman dapat menjadi tools yang sangat bermanfaat dalam proses testing API dengan mengirimkan HTTP request dan memverifikasi responsenya. Dengan menggunakan Postman collection, saya dapat dengan mudah mengatur data yang dikirimkan dalam HTTP request dan memastikan bahwa program memberikan response yang benar. Keuntungan utama menggunakan postman adalah kemampuan untuk testing API tanpa harus menggunakan data aktual dari program, sehingga mempermudah proses development dan testing. Fitur-fitur pada postman kemungkinan besar akan sangat berguna kedepannya untuk mengerjakan TK karena mempermudah dalam melakukan testing API pada suatu endpoint dan responsenya secara otomatis.