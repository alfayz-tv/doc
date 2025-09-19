# TCP (Transmission Control Protocol) - RFC 9293

RFC 9293 adalah dokumen resmi yang mendefinisikan **Transmission Control Protocol (TCP)** sebagai bagian dari protokol inti dalam Internet Protocol Suite. RFC ini menggantikan RFC 793 dan dokumen-dokumen pembaruan sebelumnya. Salah satu bagian penting dari RFC 9293 adalah penjelasan tentang **TCP flow (alur komunikasi TCP)** yang mencakup bagaimana koneksi TCP dibentuk, dikelola, dan diakhiri.

---

## 🔁 Flow TCP dalam RFC 9293

### 1. **Establishing a Connection (Three-Way Handshake)**

TCP adalah protokol **connection-oriented**, sehingga sebelum data dikirim, koneksi harus dibangun dulu menggunakan proses **three-way handshake**:

#### Langkah-langkah:

| Tahap | Host A (Client) | Host B (Server) | Penjelasan                                                                                        |
| ----- | --------------- | --------------- | ------------------------------------------------------------------------------------------------- |
| 1     | SYN             |                 | Client mengirim **SYN** dengan initial sequence number (ISN)                                      |
| 2     |                 | SYN-ACK         | Server merespons dengan **SYN-ACK**, menyertakan ISN miliknya dan acknowledgment untuk ISN client |
| 3     | ACK             |                 | Client mengirim **ACK** untuk menyetujui ISN server. Koneksi terbentuk.                           |

#### Setelah langkah ke-3, status kedua sisi berubah ke `ESTABLISHED`.

---

### 2. **Data Transfer**

Setelah koneksi terbentuk, data dapat ditransmisikan secara dua arah (full duplex). TCP menjamin:

* **Reliability**: Data dikirim ulang jika tidak diterima (dikonfirmasi dengan ACK).
* **In-order delivery**: Data disusun ulang jika tiba tidak sesuai urutan.
* **Flow control**: Menggunakan **window size** agar pengirim tidak membanjiri penerima.
* **Congestion control**: Menghindari kemacetan di jaringan.

#### Mekanisme utama:

* **ACK**: Receiver mengirim pengakuan atas data yang diterima.
* **Window-based Flow Control**:

  * Setiap segment TCP mencantumkan `Window` yang menunjukkan kapasitas buffer penerima.
  * Sender membatasi jumlah data dalam "flight" berdasarkan window ini.
* **Sequence Number**:

  * Semua byte dalam TCP memiliki urutan.
  * ACK menyatakan byte berikutnya yang diharapkan (cumulative acknowledgment).

---

### 3. **Connection Termination (Four-Way Teardown)**

Proses pengakhiran koneksi TCP dilakukan dengan 4 langkah (biasanya):

| Tahap | Host A | Host B | Penjelasan                                           |
| ----- | ------ | ------ | ---------------------------------------------------- |
| 1     | FIN    |        | Host A ingin mengakhiri komunikasi. Mengirim **FIN** |
| 2     |        | ACK    | Host B mengakui FIN dengan **ACK**                   |
| 3     |        | FIN    | Host B juga ingin menutup koneksi. Mengirim **FIN**  |
| 4     | ACK    |        | Host A mengakui FIN Host B dengan **ACK**            |

* Setelah tahap ini, kedua sisi masuk ke status `CLOSED`.
* Ada status peralihan seperti `FIN_WAIT_1`, `TIME_WAIT`, `CLOSE_WAIT`, dll.

---

### 4. **TCP State Diagram** (Ringkasan Status)

```
CLOSED
  |
  | (Active Open)
  v
SYN_SENT <------\
  |               \ (SYN Received)
  v                v
ESTABLISHED <--> FIN_WAIT_1 <--> FIN_WAIT_2 --> TIME_WAIT
  ^                                          ^
  |                                          |
  | (Passive Open)          CLOSE_WAIT --> LAST_ACK
  |                                          |
  \-----------------------------------------/
```

---

## 🔐 Fitur Lain dalam RFC 9293

* **Selective Acknowledgment (SACK)**: Pengakuan data sebagian untuk efisiensi retransmisi.
* **Options Field**: Bisa digunakan untuk Window Scale, MSS, Timestamps.
* **Urgent Pointer**: Untuk menandai data mendesak (jarang digunakan sekarang).
* **Checksum**: Untuk verifikasi integritas data TCP dan pseudo-header IP.

---

## 📚 Ringkasan

| Aspek                | Penjelasan                                                     |
| -------------------- | -------------------------------------------------------------- |
| **Connection Setup** | Menggunakan Three-Way Handshake                                |
| **Reliability**      | Dijamin lewat ACK, retransmission, sequence number             |
| **Flow Control**     | Berbasis Window Size                                           |
| **Termination**      | Menggunakan Four-Way Handshake (FIN/ACK)                       |
| **Status TCP**       | Dikelola melalui TCP state machine                             |
| **RFC**              | RFC 9293 adalah dokumentasi resmi dan terkini TCP (April 2022) |

---

```flow
                            +---------+ ---------\      active OPEN
                            |  CLOSED |            \    -----------
                            +---------+<---------\   \   create TCB
                              |     ^              \   \  snd SYN
                 passive OPEN |     |   CLOSE        \   \
                 ------------ |     | ----------       \   \
                  create TCB  |     | delete TCB         \   \
                              V     |                      \   \
          rcv RST (note 1)  +---------+            CLOSE    |    \
       -------------------->|  LISTEN |          ---------- |     |
      /                     +---------+          delete TCB |     |
     /           rcv SYN      |     |     SEND              |     |
    /           -----------   |     |    -------            |     V
+--------+      snd SYN,ACK  /       \   snd SYN          +--------+
|        |<-----------------           ------------------>|        |
|  SYN   |                    rcv SYN                     |  SYN   |
|  RCVD  |<-----------------------------------------------|  SENT  |
|        |                  snd SYN,ACK                   |        |
|        |------------------           -------------------|        |
+--------+   rcv ACK of SYN  \       /  rcv SYN,ACK       +--------+
   |         --------------   |     |   -----------
   |                x         |     |     snd ACK
   |                          V     V
   |  CLOSE                 +---------+
   | -------                |  ESTAB  |
   | snd FIN                +---------+
   |                 CLOSE    |     |    rcv FIN
   V                -------   |     |    -------
+---------+         snd FIN  /       \   snd ACK         +---------+
|  FIN    |<----------------          ------------------>|  CLOSE  |
| WAIT-1  |------------------                            |   WAIT  |
+---------+          rcv FIN  \                          +---------+
  | rcv ACK of FIN   -------   |                          CLOSE  |
  | --------------   snd ACK   |                         ------- |
  V        x                   V                         snd FIN V
+---------+               +---------+                    +---------+
|FINWAIT-2|               | CLOSING |                    | LAST-ACK|
+---------+               +---------+                    +---------+
  |              rcv ACK of FIN |                 rcv ACK of FIN |
  |  rcv FIN     -------------- |    Timeout=2MSL -------------- |
  |  -------            x       V    ------------        x       V
   \ snd ACK              +---------+delete TCB          +---------+
     -------------------->|TIME-WAIT|------------------->| CLOSED  |
                          +---------+                    +---------+
```

## 🧭 Daftar Lengkap TCP States

TCP memiliki **11 state utama**, yang mewakili status koneksi antara dua host. Setiap state merepresentasikan langkah atau fase tertentu dalam alur koneksi TCP.

| No | State          | Penjelasan Singkat                                                              |
| -- | -------------- | ------------------------------------------------------------------------------- |
| 1  | `CLOSED`       | Tidak ada koneksi (default awal atau setelah koneksi ditutup)                   |
| 2  | `LISTEN`       | Server menunggu koneksi masuk (passive open)                                    |
| 3  | `SYN-SENT`     | Client telah mengirim SYN (active open) dan menunggu SYN-ACK                    |
| 4  | `SYN-RECEIVED` | Server menerima SYN dan membalas dengan SYN-ACK, menunggu ACK                   |
| 5  | `ESTABLISHED`  | Koneksi aktif, data bisa dikirim dan diterima                                   |
| 6  | `FIN-WAIT-1`   | Endpoint mengirim FIN, menunggu ACK dan/atau FIN lawan                          |
| 7  | `FIN-WAIT-2`   | FIN dikonfirmasi, menunggu FIN dari lawan                                       |
| 8  | `CLOSE-WAIT`   | Endpoint menerima FIN dari lawan, menunggu proses penutupan lokal               |
| 9  | `CLOSING`      | Kedua pihak mengirim FIN hampir bersamaan                                       |
| 10 | `LAST-ACK`     | Menunggu ACK terakhir setelah mengirim FIN                                      |
| 11 | `TIME-WAIT`    | Menunggu waktu tertentu untuk memastikan semua segmen lama habis sebelum CLOSED |

---

## 🔍 Penjelasan Detail Tiap State

### 1. `CLOSED`

* **Awal dan akhir dari koneksi TCP.**
* Tidak ada komunikasi berlangsung.
* Terjadi setelah koneksi ditutup sepenuhnya atau belum pernah dibuka.

---

### 2. `LISTEN`

* Digunakan oleh server.
* Menunggu koneksi dari client (passive open).
* Saat menerima SYN dari client, pindah ke `SYN-RECEIVED`.

---

### 3. `SYN-SENT`

* Digunakan oleh client.
* Setelah mengirim SYN (active open), menunggu SYN-ACK dari server.
* Jika diterima, lanjut ke `ESTABLISHED`.

---

### 4. `SYN-RECEIVED`

* Setelah menerima SYN, server mengirim SYN-ACK.
* Menunggu ACK dari client untuk menyelesaikan handshake.
* Jika ACK diterima, pindah ke `ESTABLISHED`.

---

### 5. `ESTABLISHED`

* Koneksi dua arah aktif.
* Data bisa ditransfer secara penuh (duplex).
* Ini adalah **state utama** untuk komunikasi data TCP.

---

### 6. `FIN-WAIT-1`

* Aplikasi lokal memulai penutupan koneksi (mengirim FIN).
* Menunggu ACK dari lawan atau FIN dari lawan.
* Jika menerima ACK → pindah ke `FIN-WAIT-2`.

---

### 7. `FIN-WAIT-2`

* ACK diterima untuk FIN kita.
* Menunggu FIN dari lawan untuk menutup koneksi sepenuhnya.

---

### 8. `CLOSE-WAIT`

* Menerima FIN dari lawan.
* Menunggu aplikasi lokal menutup koneksi (mengirim FIN).
* Biasanya terjadi saat aplikasi belum memanggil `close()`.

---

### 9. `CLOSING`

* Kedua sisi mengirim FIN hampir bersamaan.
* Menunggu ACK dari lawan untuk FIN kita.
* Setelah ACK diterima, masuk ke `TIME-WAIT`.

---

### 10. `LAST-ACK`

* Setelah mengirim FIN (dari `CLOSE-WAIT`), menunggu ACK dari lawan.
* Saat ACK diterima, koneksi ditutup → `CLOSED`.

---

### 11. `TIME-WAIT`

* Setelah koneksi ditutup, koneksi tetap ada selama **2 x MSL** (Maximum Segment Lifetime).
* Mencegah duplikasi segmen lama masuk ke koneksi baru.
* Setelah waktu habis, pindah ke `CLOSED`.

---

## 🔁 Diagram Transisi Sederhana

```
                          Passive Open
          +---------+      -> LISTEN
          | CLOSED  |<--------------------------+
          +---------+                           |
              | Active Open                     |
              v                                 |
         +------------+                         |
         | SYN-SENT   |<--+                     |
         +------------+   |                    |
              |           |                    |
              v           |                    |
     +----------------+   |    Receive SYN     |
     | SYN-RECEIVED   |<--+--------------------+
     +----------------+                        |
              |                                |
              v                                |
        +-------------+                        |
        | ESTABLISHED |------------------------+
        +-------------+
```

Untuk teardown:

```
ESTABLISHED
   |       \
FIN        FIN (dari lawan)
   |         \
FIN-WAIT-1   CLOSE-WAIT
   |            |
ACK         (kirim FIN)
   v            v
FIN-WAIT-2    LAST-ACK
   |            |
FIN         (terima ACK)
   v            v
TIME-WAIT   CLOSED
   |
   v
CLOSED
```

---

## 📌 Tips Penting

* State `TIME-WAIT` bisa menyebabkan port "tertahan" jika banyak koneksi pendek → bisa di-tuning.
* `CLOSE-WAIT` sering muncul jika aplikasi tidak menutup socket setelah FIN diterima.
* Gunakan perintah seperti `netstat -an` atau `ss -t` di Linux untuk melihat state TCP.

---

Kalau kamu mau, aku bisa bantu buatkan **visual diagram** (gambar state diagram TCP) atau **simulasi alur dengan contoh nyata**, misalnya saat kamu `curl` ke server. Mau?
