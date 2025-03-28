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
-   [v] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [v] Commit: `Create Notification model struct.`
    -   [v] Commit: `Create SubscriberRequest model struct.`
    -   [v] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [v] Commit: `Implement add function in Notification repository.`
    -   [v] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [v] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [v] Commit: `Create Notification service struct skeleton.`
    -   [v] Commit: `Implement subscribe function in Notification service.`
    -   [v] Commit: `Implement subscribe function in Notification controller.`
    -   [v] Commit: `Implement unsubscribe function in Notification service.`
    -   [v] Commit: `Implement unsubscribe function in Notification controller.`
    -   [v] Commit: `Implement receive_notification function in Notification service.`
    -   [v] Commit: `Implement receive function in Notification controller.`
    -   [v] Commit: `Implement list_messages function in Notification service.`
    -   [v] Commit: `Implement list function in Notification controller.`
    -   [v] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

1. Penggunaan RwLock<> dalam kasus ini penting karena kita ingin mengizinkan multiple reader secara bersamaan saat hanya membaca data, tetapi tetap memastikan akses eksklusif saat ada writer yang ingin memodifikasi ```Vec<Notification>```. Jika kita menggunakan Mutex<> sebagai gantinya, hanya satu thread yang dapat mengakses data dalam satu waktu, baik untuk membaca maupun menulis, yang dapat menyebabkan bottleneck dalam sistem dengan banyak pembacaan. RwLock<> memungkinkan kita untuk meningkatkan efisiensi dengan membiarkan beberapa thread membaca data secara bersamaan tanpa harus saling menunggu, kecuali jika ada thread yang sedang melakukan operasi tulis.

2. Rust tidak mengizinkan kita untuk langsung memodifikasi konten dari variabel static seperti di Java karena Rust mengutamakan memory safety dan thread safety tanpa harus bergantung pada garbage collector. Jika static mutable diperbolehkan secara langsung, kita akan menghadapi race condition karena beberapa thread bisa mengakses dan mengubahnya secara bersamaan tanpa mekanisme sinkronisasi yang aman. Oleh karena itu, kita menggunakan library lazy_static bersama RwLock<> atau DashMap untuk memastikan akses yang aman terhadap shared state. Rust mewajibkan kita untuk secara eksplisit menyatakan kapan kita ingin membaca atau menulis ke dalam data yang dibagikan, sehingga memastikan bahwa akses terhadap data bersifat terkontrol dan aman dari race conditions yang sering terjadi dalam bahasa lain seperti Java.

#### Reflection Subscriber-2

1. Saya melakukan sedikit eksplorasi di luar langkah-langkah dalam tutorial, dengan fokus utama pada pemahaman sistem dan interaksi antar bagian kode. Saya melihat bahwa RwLock<> digunakan untuk mengelola daftar notifikasi secara thread-safe, memastikan akses data tanpa race condition. Selain itu, saya juga memperhatikan penggunaan serde dalam proses serialisasi dan deserialisasi data JSON, yang penting untuk komunikasi antar komponen sistem. Namun, saya belum banyak mengeksplorasi src/lib.rs, karena tutorial lebih berfokus pada pengembangan controller dan service. Saya memilih untuk menyelesaikan bagian utama terlebih dahulu sebelum mendalami bagian lain dari kode.

2. Observer Pattern mempermudah proses penambahan subscriber baru dalam sistem ini. Dengan pola ini, publisher tidak perlu mengetahui secara spesifik siapa saja yang menjadi subscriber, melainkan cukup mengelola daftar subscriber yang terdaftar. Subscriber hanya perlu berlangganan dan akan otomatis menerima notifikasi saat ada perubahan. Hal ini sangat fleksibel karena kita dapat dengan mudah menambah atau menghapus subscriber tanpa harus mengubah kode pada publisher. Namun, jika kita ingin menambah lebih dari satu instance Main app, tantangannya akan lebih besar. Kita harus memastikan bahwa komunikasi antar instance berjalan dengan baik, terutama jika ada lebih dari satu publisher yang mengelola produk yang sama. Jika tidak dikelola dengan baik, bisa saja notifikasi yang dikirim tidak sesuai atau subscriber menerima pesan dari publisher yang salah. Oleh karena itu, sistem perlu dirancang agar mendukung komunikasi antar instance secara efektif.

3. Ya, saya telah mencoba menambahkan beberapa tes pada Postman untuk memastikan bahwa sistem notifikasi bekerja sesuai dengan harapan. Saya menguji berbagai skenario seperti subscribe, unsubscribe, dan penerimaan notifikasi dengan variasi produk yang berbeda. Tes ini sangat membantu dalam memastikan bahwa setiap endpoint berjalan dengan baik sebelum diintegrasikan lebih lanjut. 