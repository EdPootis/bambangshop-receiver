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
1. In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead? <br>
   `RwLock<>` digunakan untuk mensinkronisasi akses karena `RwLock<>` membolehkan lebih dari 1 *thread* untuk mengakses data secara paralel jika hanya untuk melakukan *read*/pembacaan data. `RwLock<>` hanya akan membatasi akses *thread* jika ada 1 thread yang membutuhkan akses untuk melakukan *write*/modifikasi data. Pada sinkronisasi data notifikasi, akses data hanya digunakan untuk melakukan pembacaan data notifikasi sehingga tidak akan terjadi blocking karena ada suatu *thread* yang ingin melakukan *write*. Tidak digunakan `Mutex<>` karena tidak seperti `RwLock<>`, `Mutex<>` tidak membedakan akses data *read* dan *write*, akibatnya jika *thread* ingin melakukan *read* akan tetap dilakukan blocking. Sehingga jika terdapat banyak *thread*, akan terjadi banyak blocking dimana jika digunakan `RwLock<>` semua thread dapat mengakses data secara paralel  dengan asumsi semuanya melakukan *read*.

2. In this tutorial, we used lazy_static external library to define Vec and DashMap as a "static" variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so? <br>
   Pada Java, variabel *static* dapat dimodifikasi langsung dengan *static method*, tetapi hal ini dapat menyebabkan *race condition* yang menyebabkan inkonsistensi data jika ada beberapa *thread* yang mengakses dan memodifikasi variabel *static* tersebut. Sementara, Rust mencegah modifikasi langsung pada variabel *static* untuk menghindari *race condition*. Jika ingin melakukan modifikasi langsung pada variabel, maka diperlukan penggunaan mekanisme yang *thread-safe*, contohnya `lazy_static`. Dengan penggunaan `lazy_static`, insialisasi hanya akan terjadi sekali, yaitu saat program mengaksesnya untuk pertama kali.

#### Reflection Subscriber-2
1. Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code. <br>
   Ya, saya sudah melakukan beberapa eksplorasi hal-hal yang berada di luar tutorial. Salah satunya adalah `src/lib.srs` yang gunanya untuk melakukan konfigurasi global, manajemen dependensi, dan mekanisme *error handling* dalam masing-masing aplikasi. Contoh hal yang dapat dipelajari dari `src/lib.srs` BambangShop adalah manajemen variabel *singleton* `lazy_static!` yang berisi `REQWEST_CLIENT` dan `APP_CONFIG`. Dimana `APP_CONFIG` dapat dibuat berdasarkan file `.env` yang berada pada root folder. Lalu kegunaan file `.env` juga memungkinkan pengubahan konfigurasi tanpa melakukan *hard-coding*, karena hanya perlu mengganti nilai variabel pada file `.env`. Pada file `src/lib.rs` juga dikonfigurasikan pesan *error* yang kustom untuk mempermudah *debugging* dan *logging*.

2. Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system? <br>
   Observer Pattern menurut saya mempermudah proses penambahan *subscriber*/*receiver* baru karena setiap *instance* dari *receiver* merupakan *observer* yang saling terpisah/indenpenden. Dengan ini, penambahan *subscriber* dapat dilakukan hanya dengan *observer* tersebut mengirim *request* ke Main app agar ditambahkan ke daftar *subscriber* suatu tipe produk.<br>
   Jika ingin ditambahkan *instance* dari Main app, keduanya akan memiliki sistem notifikasi dan *subscribe* yang terpisah juga. Namun, selama mekanismenya sama maka penambahan *subscriber* tidak akan memiliki perbedaan yang jauh, perbedaan yang mungkin adalah penyesuaian URL dari *instance* Main app yang kedua dst. sehingga harus sedikit disesuaikan. Selain itu seharusnya tidak ada perubahan selama endpoint untuk servis-servis identikal.

3. Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project). <br>
   Ya, sebelum tutorial ini saya sudah beberapa kali menggunakan Postman untuk membuat tes-tes. Pada tutorial ini Postman juga digunakan sebagai dokumentasi dari *endpoint-endpoint* yang ada pada aplikasi. Postman *collection* yang ada saya juga gunakan untuk mengecek kebenaran fitur-fitur dari aplikasi, seperti `subscribe`, `publish`, dan `delete` dan untuk mengsimulasikan berbagai *instance* aplikasi *observer* dengan menggunakan *port* yang berbeda. Pada Group Project yang akan dilakukan, menurut saya Postman akan sangat berguna karena akan dilakukan banyak testing antar API modul.

