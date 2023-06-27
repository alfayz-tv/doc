# Mqtt
MQTT (Message Queuing Telemetry Transport) adalah protokol komunikasi ringan yang dirancang untuk pertukaran pesan antara perangkat-perangkat dalam jaringan yang memiliki kebutuhan bandwidth dan penggunaan energi yang rendah. MQTT awalnya dikembangkan oleh IBM dan sekarang menjadi standar terbuka yang dikelola oleh Organisasi Oasis.

Fitur utama dari MQTT adalah sebagai berikut:

1. Publikasi-Subskripsi (Publish-Subscribe): MQTT mengadopsi model komunikasi publikasi-subskripsi di mana perangkat-perangkat dikategorikan menjadi penerbit (publisher) dan pelanggan (subscriber). Penerbit mengirim pesan ke topik (topic) tertentu, sedangkan pelanggan yang tertarik dengan topik tersebut menerima pesan tersebut.

2. Lightweight: MQTT didesain untuk menjadi protokol yang ringan, sederhana, dan memiliki overhead yang rendah. Ini membuatnya cocok untuk perangkat-perangkat dengan sumber daya terbatas seperti sensor-sensor IoT (Internet of Things) yang menggunakan daya dan bandwidth yang terbatas.

3. Bandwidth Efisien: Protokol MQTT menggunakan header yang ringan dan kompresi data yang minimal, sehingga mengurangi penggunaan bandwidth jaringan. Hal ini penting dalam lingkungan dengan keterbatasan bandwidth, koneksi jaringan yang tidak stabil, atau biaya transmisi data yang tinggi.

4. QoS (Quality of Service): MQTT menyediakan tiga level QoS yang memungkinkan pengaturan tingkat keandalan pengiriman pesan. Level QoS 0 menjamin pengiriman satu kali tanpa adanya mekanisme pengiriman ulang. Level QoS 1 memastikan pengiriman setidaknya satu kali. Level QoS 2 memastikan pengiriman tepat satu kali dengan mekanisme pengiriman ulang yang kuat.

5. Retained Messages (Pesan Dipertahankan): MQTT mendukung fitur pesan yang dipertahankan, di mana server MQTT dapat menyimpan pesan terakhir yang dikirim ke topik dan mengirimkannya kepada pelanggan yang baru bergabung.

MQTT sering digunakan dalam skenario Internet of Things (IoT), sensor monitoring, dan komunikasi antara perangkat-perangkat yang memiliki sumber daya terbatas. Protokol ini mendukung konektivitas yang andal, konsumsi daya yang rendah, dan komunikasi efisien dalam lingkungan jaringan yang terdistribusi.