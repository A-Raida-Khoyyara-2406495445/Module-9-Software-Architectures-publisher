# Understanding Publisher and Message Broker

## a. Berapa banyak data yang dikirim publisher ke message broker dalam satu kali run?
Setiap kali program publisher dijalankan dengan `cargo run`, publisher akan mengirimkan **5 event** ke RabbitMQ(message broker). Masing-masing event berupa objek `UserCreatedEventMessage` yang memiliki dua field: `user_id` dan `user_name`. Kelima pesan tersebut adalah:

| user_id | user_name |
|---------|-----------|
| 1 | 2406495445-Amir |
| 2 | 2406495445-Budi |
| 3 | 2406495445-Cica |
| 4 | 2406495445-Dira |
| 5 | 2406495445-Emir |

Setiap pesan diserialkan menggunakan *Borsh serialization* lalu data tersebut dikirim ke queue bernama `user_created` pada RabbitMQ.

## b. URL `amqp://guest:guest@localhost:5672` sama dengan subscriber, apa artinya?

Kesamaan URL tersebut menunjukkan bahwa publisher dan subscriber terhubung ke Message Broker yang sama, yaitu RabbitMQ yang berjalan pada `localhost:5672`. Hal ini merupakan implementasi fundamental dari arsitektur *Event-Driven*, di mana komunikasi antar komponen bersifat tidak langsung (decoupled). 
- Publisher hanya bertanggung jawab mengirimkan pesan ke broker tanpa perlu mengetahui lokasi atau keberadaan penerima.
- Broker (RabbitMQ) berperan sebagai perantara yang mengatur perutean pesan ke antrean (queue) yang sesuai, dalam hal ini user_created.
- Subscriber cukup mendaftarkan diri pada broker untuk menerima pesan tersebut.

## Screenshot Implementasi

### 1. Publisher Berjalan dengan RabbitMQ
Menunjukkan publisher berhasil terhubung ke RabbitMQ dan mengirimkan event.
![Running with RabbitMQ](assets/images/runningwithrabbitmq.png)

### 2. Subscriber Terminal
Menunjukkan output pada terminal subscriber saat menerima pesan dari publisher.
![Subscriber Terminal](assets/images/subscriberterminal.png)

### 3. Koneksi Subscriber
Menunjukkan proses subscriber saat melakukan koneksi ke message broker.
![Subscriber Connection](assets/images/onesubscribermakingconnection%20-%20Copy.png)


