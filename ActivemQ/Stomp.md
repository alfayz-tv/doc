# STOMP
STOMP (Simple/Streaming Text Oriented Messaging Protocol) adalah protokol komunikasi yang digunakan untuk pertukaran pesan antara klien dan server yang mendukung protokol messaging, seperti Apache ActiveMQ, RabbitMQ, atau Apache Kafka. STOMP dirancang untuk menjadi protokol yang sederhana dan mudah digunakan, terutama dalam lingkungan aplikasi berbasis pesan (messaging) yang terdistribusi.

Fitur utama dari STOMP antara lain:

- Simplicity (Kesederhanaan): STOMP didesain agar mudah diimplementasikan dan digunakan. Protokol ini menggunakan pesan berbasis teks yang terdiri dari kepala (header) dan isi (body) yang dapat dibaca oleh manusia.

- Protocol-agnostic (Protokol-agnostik): STOMP dapat digunakan dengan berbagai protokol messaging, seperti AMQP (Advanced Message Queuing Protocol) atau MQTT (Message Queuing Telemetry Transport).

- Language-independent (Tidak tergantung bahasa pemrograman): STOMP tidak terikat pada bahasa pemrograman tertentu, sehingga dapat digunakan oleh klien yang ditulis dalam berbagai bahasa pemrograman.

- Pub-sub model: STOMP mendukung pola komunikasi publikasi-subskripsi (publish-subscribe), di mana klien dapat berlangganan (subscribe) ke topik (topic) tertentu untuk menerima pesan yang dikirimkan ke topik tersebut.

- Message acknowledgment (Pengakuan pesan): STOMP memungkinkan klien untuk mengakui (acknowledge) penerimaan pesan, sehingga server dapat mengetahui apakah pesan telah berhasil diterima oleh klien.

STOMP dapat digunakan dalam berbagai kasus penggunaan, seperti sistem komunikasi real-time, antrian pesan, dan integrasi antara berbagai aplikasi yang menggunakan protokol messaging yang berbeda.

Harap dicatat bahwa STOMP hanyalah salah satu dari banyak protokol messaging yang tersedia, dan pemilihan protokol messaging yang tepat tergantung pada kebutuhan dan lingkungan aplikasi Anda.


# Queueu dan Topic
Dalam protokol STOMP (Simple Text Oriented Messaging Protocol), terdapat perbedaan antara Queue dan Topic:

1. Queue (Antrian):
   - Dalam Queue, pesan dikirim dari pengirim ke satu penerima tunggal (1-1). Pesan yang dikirim ke antrian akan diterima oleh penerima tunggal yang pertama kali siap menerima pesan tersebut.
   - Setiap pesan hanya akan dikonsumsi oleh satu penerima saja. Jika ada beberapa penerima yang terhubung pada saat yang sama, pesan akan dikonsumsi oleh salah satu penerima secara acak.
   - Antrian biasanya digunakan untuk tujuan point-to-point (1-1) di mana pengirim ingin memastikan bahwa pesan mereka dikirim dan diterima oleh penerima tunggal.

2. Topic (Topik):
   - Dalam Topic, pesan dikirim dari pengirim ke satu atau lebih penerima (1-N). Pesan yang dikirim ke topik akan diterima oleh semua penerima yang terhubung pada saat itu.
   - Setiap pesan akan dikonsumsi oleh semua penerima yang terhubung pada saat itu. Pesan akan disampaikan kepada setiap penerima yang terhubung ke topik.
   - Topik biasanya digunakan untuk publikasi-subskripsi (publish-subscribe) di mana pengirim ingin menyebarkan pesan kepada banyak penerima yang tertarik pada topik tersebut.

Dengan menggunakan Queue, pesan dikirim ke satu penerima tunggal yang pertama kali siap menerima pesan, sementara dengan menggunakan Topic, pesan dikirim ke semua penerima yang terhubung saat itu.

Pilihan antara menggunakan Queue atau Topic tergantung pada kebutuhan aplikasi Anda. Jika Anda ingin pesan hanya dikonsumsi oleh satu penerima, maka Queue lebih sesuai. Namun, jika Anda ingin pesan disampaikan kepada banyak penerima yang tertarik pada topik tersebut, maka menggunakan Topic lebih tepat.