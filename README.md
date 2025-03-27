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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create SubscriberRequest model struct.`
    -   [ ] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Notification repository.`
    -   [ ] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Commit: `Implement receive_notification function in Notification service.`
    -   [ ] Commit: `Implement receive function in Notification controller.`
    -   [ ] Commit: `Implement list_messages function in Notification service.`
    -   [ ] Commit: `Implement list function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

1) In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?

Jawaban:
   `RwLock<>` digunakan karena memungkinkan beberapa *thread* untuk membaca data secara bersamaan (*multiple readers*), sementara hanya satu *thread* yang dapat menulis (*single writer*). Dalam kasus ini, kita sering membaca daftar notifikasi, sehingga `RwLock<>` lebih efisien dibandingkan `Mutex<>`, yang hanya mengizinkan satu *thread* untuk mengakses data, baik untuk membaca maupun menulis.  

   Jika kita menggunakan `Mutex<>`, setiap *thread* yang hanya ingin membaca tetap harus menunggu akses eksklusif, yang dapat menyebabkan bottleneck dan menurunkan kinerja. Dengan `RwLock<>`, beberapa *thread* dapat membaca secara paralel tanpa saling menghambat, sehingga meningkatkan efisiensi dalam kasus yang lebih sering membaca daripada menulis.  


2) In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?

Jawaban:

   Rust memiliki sistem kepemilikan (*ownership system*) dan aturan keamanan *thread* yang ketat untuk mencegah kondisi *race* (*data race*). Variabel `static` di Rust bersifat *immutable* secara default untuk menghindari masalah terkait akses bersamaan oleh beberapa *thread*.  

   Di Java, variabel `static` dapat diubah secara langsung melalui fungsi `static` karena Java menggunakan *garbage collection* dan tidak secara ketat menegakkan kepemilikan data. Sementara itu, Rust tidak memiliki *garbage collector*, sehingga perlu memastikan bahwa akses ke variabel `static` aman dan bebas dari kondisi *race*. Oleh karena itu, untuk memodifikasi variabel `static`, kita harus menggunakan mekanisme sinkronisasi seperti `Mutex<>`, `RwLock<>`, atau `DashMap`, yang menjamin keamanan akses dalam lingkungan *multi-threaded*.


#### Reflection Subscriber-2
1) Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.


Jawaban:
    Apakah Anda telah mengeksplorasi hal-hal di luar langkah-langkah dalam tutorial, seperti src/lib.rs? Jika belum, jelaskan alasannya. Jika ya, jelaskan hal-hal yang Anda pelajari dari bagian kode lainnya.

    Saya belum mulai mengeksplorasi bagian lain. Saya lebih fokus pada file yang secara langsung terkait dengan langkah-langkah dalam tutorial, seperti modul spesifik (controller, service, repository, dan model) yang disebutkan. Karena lib.rs tidak disebutkan dalam langkah-langkah tutorial, saya lebih memprioritaskan menyelesaikan tugas yang sudah diarahkan sebelum menjelajahi file tambahan.

2) Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?

Jawaban:
    Setelah menyelesaikan tutorial dan menguji sistem notifikasi dengan menjalankan beberapa instance dari Receiver, jelaskan bagaimana pola Observer mempermudah penambahan subscriber. Bagaimana jika menjalankan lebih dari satu instance dari aplikasi Main, apakah tetap mudah untuk menambahkannya ke sistem?

    Pola Observer mempermudah penambahan subscriber karena mereka cukup mengirim permintaan subscribe ke suatu topik. Hal ini secara otomatis menambahkan subscriber ke dalam daftar (dictionary) untuk topik tersebut. Ketika ada notifikasi yang harus dikirim, sistem akan secara otomatis melakukan iterasi ke semua subscriber dari topik tersebut dan mengirim permintaan notifikasi. Dengan cara ini, kode yang sudah ada dapat menangani subscriber baru tanpa perubahan tambahan.

    Menjalankan beberapa instance dari aplikasi Main juga cukup mudah. Setiap Receiver dapat dikonfigurasi untuk terhubung ke instance tertentu dengan mengatur variabel APP_PUBLISHER_ROOT_URL. Alternatifnya, sistem routing dapat mengarahkan Receiver ke instance tertentu, yang kemudian akan diingat oleh Receiver. Namun, karena setiap instance dari aplikasi Main menggunakan daftar subscriber statisnya sendiri, daftar subscriber antar instance bisa berbeda kecuali jika digunakan basis data terpadu.


3) Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

Jawaban:
    Apakah Anda sudah mencoba membuat Test sendiri atau meningkatkan dokumentasi pada koleksi Postman Anda? Jika sudah, apakah fitur tersebut berguna untuk pekerjaan Anda (baik dalam tutorial maupun proyek kelompok)?

    Saya belum mencoba membuat Test sendiri atau memperbarui dokumentasi di koleksi Postman. Saya lebih fokus menyelesaikan tugas-tugas tutorial dan mengimplementasikan fungsionalitas yang diperlukan. Namun, saya memahami bahwa fitur-fitur tersebut dapat sangat berguna untuk memastikan API bekerja dengan benar dan dapat membantu dalam pengembangan proyek kelompok di masa depan.
