# AMQP
AMQP (Advanced Message Queuing Protocol) adalah protokol yang digunakan untuk pertukaran pesan antara komponen-komponen aplikasi yang terdistribusi. Protokol ini didesain untuk mendukung sistem komunikasi berbasis antrian pesan (message queuing) yang scalable, reliable, dan interoperable.

Fitur utama dari AMQP adalah sebagai berikut:

1. Message-oriented: AMQP berfokus pada pertukaran pesan antara produsen (producer) dan konsumen (consumer) melalui antrian pesan (message queue). Pesan-pesan ini dapat berisi data atau perintah yang dikirim antar komponen aplikasi.

2. Protocol-agnostic: AMQP dirancang untuk bekerja dengan berbagai protokol jaringan yang berbeda. Ini memungkinkan klien dari berbagai bahasa pemrograman dan platform untuk berkomunikasi melalui AMQP menggunakan protokol yang sesuai, seperti TCP/IP, HTTP, atau WebSocket.

3. Pub-sub model: AMQP mendukung pola komunikasi publikasi-subskripsi (publish-subscribe), di mana produsen (publisher) mengirimkan pesan ke pertukaran (exchange) dan konsumen (subscriber) menerima pesan dari antrian (queue) yang terikat ke pertukaran tersebut.

4. Routing: AMQP menyediakan mekanisme routing yang fleksibel untuk mengarahkan pesan ke antrian yang sesuai berdasarkan kriteria tertentu, seperti header, topik (topic), atau routing key.

5. Reliable messaging: AMQP menyediakan mekanisme untuk memastikan pengiriman pesan yang handal (reliable). Ini termasuk konfirmasi penerimaan pesan (message acknowledgment), antrian tahan bencana (durable queues), dan pengiriman pesan ulang (message redelivery).


# Chanel. Exchange, Queue

AMQP telah menjadi standar industri yang diterima secara luas dan digunakan oleh banyak sistem messaging yang populer, seperti RabbitMQ, Apache ActiveMQ, dan Apache Qpid. Protokol ini cocok untuk skenario yang melibatkan pertukaran pesan antara komponen-komponen yang terdistribusi, misalnya dalam sistem mikroserbaya, komunikasi antara layanan, atau sistem antrian pesan.

Harap dicatat bahwa AMQP adalah protokol yang mandiri dan dapat digunakan dalam berbagai bahasa pemrograman dan platform dengan implementasi yang sesuai.

Dalam protokol AMQP (Advanced Message Queuing Protocol), terdapat tiga konsep utama yang digunakan untuk mengatur aliran pesan antara pengirim dan penerima: channel (saluran), exchange (pertukaran), dan queue (antrian). Berikut adalah penjelasan singkat tentang setiap konsep tersebut beserta perbedaannya:

1. Channel (Saluran):
   - Channel adalah saluran komunikasi yang dibuka antara klien (penerbit atau konsumen) dan server AMQP.
   - Setiap koneksi AMQP dapat memiliki beberapa channel yang beroperasi secara independen di dalamnya.
   - Saluran memungkinkan pemisahan dan pengaturan pesan secara paralel dalam satu koneksi.
   - Dalam implementasi AMQP seperti `github.com/streadway/amqp` di Go, channel dideklarasikan dan digunakan untuk menyampaikan perintah-perintah AMQP ke server.

2. Exchange (Pertukaran):
   - Exchange adalah entitas yang bertindak sebagai perantara untuk menerima pesan dari penerbit dan mengarahkannya ke antrian yang tepat.
   - Penerbit mengirim pesan ke exchange, dan exchange memutuskan ke mana pesan harus dikirim berdasarkan aturan pengikatan tertentu.
   - Terdapat beberapa jenis pertukaran, seperti direct, topic, fanout, dan headers, yang menentukan logika pengiriman pesan ke antrian.

3. Queue (Antrian):
   - Queue adalah tempat penyimpanan pesan yang menunggu untuk dikonsumsi oleh konsumen.
   - Konsumen mengambil pesan dari antrian untuk memprosesnya.
   - Antrian juga dapat dikonfigurasi dengan berbagai atribut, seperti tahan (durable) atau tidak, eksklusif, dan otomatis terhapus (auto-delete).

Perbedaan utama antara ketiga konsep tersebut adalah sebagai berikut:
- Channel adalah saluran komunikasi dalam satu koneksi AMQP yang digunakan untuk mengirim perintah ke server.
- Exchange bertindak sebagai perantara untuk menerima pesan dari penerbit dan mengarahkannya ke antrian yang tepat berdasarkan aturan pengikatan.
- Queue adalah tempat penyimpanan pesan yang menunggu untuk dikonsumsi oleh konsumen.

Dalam aliran pesan, penerbit mengirim pesan ke exchange, exchange mengarahkannya ke antrian yang sesuai, dan konsumen mengambil pesan dari antrian. Konsep ini memungkinkan skenario yang lebih fleksibel dalam pengiriman pesan, di mana pesan dapat dikirim ke satu atau lebih antrian berdasarkan logika yang didefinisikan oleh exchange dan pengikatan (binding) yang ada.


# Type Exchange
Dalam protokol AMQP, terdapat empat jenis pertukaran (exchange) yang berbeda: direct, topic, fanout, dan headers. Mari kita jelaskan masing-masing jenis pertukaran dan perbedaannya:

1. Direct Exchange:
   - Pertukaran direct mengirim pesan ke antrian berdasarkan routing key yang cocok secara langsung dengan routing key yang ditentukan oleh penerbit.
   - Saat penerbit mengirim pesan dengan routing key tertentu, pertukaran direct akan mengirim pesan tersebut langsung ke antrian yang memiliki binding dengan routing key yang cocok.
   - Ini adalah pola pertukaran yang sederhana dan langsung.

2. Topic Exchange:
   - Pertukaran topik mengirim pesan ke antrian berdasarkan pola routing key yang cocok dengan pola yang ditentukan oleh penerbit.
   - Routing key dalam pertukaran topik biasanya menggunakan pola wildcard dengan menggunakan karakter '*' (mewakili satu kata kunci) dan '#' (mewakili nol atau lebih kata kunci).
   - Misalnya, jika routing key adalah "animal.*", maka pesan akan dikirim ke antrian yang memiliki binding dengan routing key seperti "animal.cat" atau "animal.dog".
   - Pertukaran topik memungkinkan pengiriman pesan yang lebih fleksibel berdasarkan pola routing key yang cocok.

3. Fanout Exchange:
   - Pertukaran fanout mengirim pesan ke semua antrian yang terikat tanpa memperhatikan routing key.
   - Ketika penerbit mengirim pesan ke pertukaran fanout, pesan tersebut akan dikirim ke semua antrian yang terikat pada pertukaran tersebut.
   - Pertukaran fanout secara efektif menyebarkan pesan ke banyak antrian tanpa perlu memperhatikan pola routing key.

4. Headers Exchange:
   - Pertukaran headers menggunakan atribut tambahan di header pesan untuk mengarahkan pesan ke antrian yang sesuai.
   - Routing key tidak digunakan dalam pertukaran headers. Sebaliknya, pertukaran ini mencocokkan atribut-atribut dalam header pesan, seperti tipe konten, dengan kriteria yang didefinisikan oleh binding untuk menentukan antrian yang menerima pesan.
   - Pertukaran headers dapat memberikan fleksibilitas yang tinggi dalam memilih antrian berdasarkan atribut-atribut pesan.

Perbedaan utama antara keempat jenis pertukaran tersebut adalah bagaimana pesan dikirim dan diteruskan ke antrian:
- Direct exchange menggunakan routing key untuk memilih antrian yang tepat.
- Topic exchange menggunakan pola routing key yang lebih fleksibel dengan wildcard.
- Fanout exchange mengirim pesan ke semua antrian yang terikat tanpa memperhatikan routing key.
- Headers exchange menggunakan atribut dalam header pesan untuk memilih antrian.

Anda dapat memilih jenis pertukaran yang sesuai dengan kebutuhan dan skenario pengiriman pesan yang Anda inginkan dalam aplikasi AMQP Anda.


# Routing Key
Routing key adalah nilai yang ditentukan oleh penerbit saat mengirimkan pesan ke pertukaran (exchange) dalam protokol AMQP (Advanced Message Queuing Protocol). Routing key digunakan oleh pertukaran untuk mengarahkan pesan ke antrian yang sesuai berdasarkan aturan pengikatan (binding).

Ketika seorang penerbit mengirim pesan ke pertukaran, ia menentukan routing key yang sesuai dengan karakteristik pesan atau tujuan pengiriman tertentu. Pertukaran kemudian menggunakan routing key ini untuk memutuskan ke mana pesan tersebut harus dikirim.

Routing key dapat memiliki berbagai format tergantung pada jenis pertukaran yang digunakan. Misalnya, dalam pertukaran direct, routing key adalah nilai yang digunakan untuk mencocokkan dengan routing key yang ditentukan dalam binding untuk mengirimkan pesan ke antrian yang sesuai secara langsung. Dalam pertukaran topik, routing key dapat berupa pola wildcard yang mencocokkan pola routing key yang ditentukan dalam binding.

Perbedaan dalam penggunaan dan interpretasi routing key bergantung pada jenis pertukaran yang digunakan. Penerbit dan konsumen harus menggunakan routing key yang sesuai dan memiliki pemahaman yang konsisten tentang bagaimana routing key akan digunakan oleh pertukaran untuk memastikan pesan dikirim ke antrian yang tepat.