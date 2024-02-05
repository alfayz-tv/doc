
# Epp (Extensible Provisioning Protocol)
> Extensible Provisioning Protocol (EPP) adalah protokol yang digunakan dalam industri domain name registration untuk melakukan pendaftaran, pembaruan, dan penghapusan nama domain. Protokol ini dirancang untuk memberikan antarmuka standar yang dapat diandalkan dan dapat diperluas untuk pengelolaan domain names secara efisien.

> EPP dikembangkan oleh Internet Engineering Task Force (IETF) untuk mengatasi beberapa kekurangan yang ada pada protokol sebelumnya yang digunakan dalam industri pendaftaran domain. Salah satu tujuan utama EPP adalah memberikan fleksibilitas dan perluasan agar dapat menangani kebutuhan yang berkembang dalam pengelolaan domain.

# RFC pada EPP

- RFC 5730 Extensible Provisioning Protocol (EPP)
- RFC 5731 Extensible Provisioning Protocol (EPP) Domain Name Mapping
- RFC 5732 Extensible Provisioning Protocol (EPP) Host Mapping
- RFC 5733 Extensible Provisioning Protocol (EPP) Contact Mapping
- RFC 5734 Extensible Provisioning Protocol (EPP) Transport over TCP
- RFC 5910 Domain Name System (DNS) Security Extensions Mapping for the Extensible   Provisioning Protocol (EPP)

# Contoh Module Yang sering di gunakan untuk koneksi Epp:
1. module untuk php
    - https://github.com/AfriCC/php-epp2
    - https://github.com/pandi-id/php-epp-id

2. Module untuk golang
    - https://github.com/al0fayz/epp-pandi-client


# Contoh Code menggunakan Php koneksi epp
```php
//example koneksi
<?php
namespace App\Epp;
use AfriCC\EPP\Client as EPPClient;

class Koneksi{
    private $server;
    private $port = 700;
    private $username;
    private $password;
    private $certPath;

    public function __construct(){
        $this->server   = env("EPPSERVERHOST");
        $this->port     = env("EPPSERVERPORT");
        $this->username = env("EPPUSERNAME");
        $this->password = env("EPPPASSWORD");
        $this->certPath = env("EPPCERT");
    }

    public function login(){
        $epp_client = new EPPClient([
            'host' => $this->server,
            'port' => $this->port,
            'username' => $this->username,
            'password' => $this->password,
            'services' => [
                'urn:ietf:params:xml:ns:domain-1.0',
                'urn:ietf:params:xml:ns:contact-1.0',
                'urn:ietf:params:xml:ns:host-1.0',
                'urn:ietf:params:xml:ns:epp-1.0'
            ],
            'local_cert' => app()->basePath('public'.$this->certPath),
            'pass_phrase' => '',
            'ssl' => true,
            'debug' => false,
        ]);
        try {
            $epp_client->connect();
            //dd($epp_client); //debug
            return $epp_client;
        } catch(\Exception $e) {
            //dd($e); //debug
            return $epp_client;
        }
    }
}

```

```php
//example check domain
<?php
use App\Epp\Koneksi;
use AfriCC\EPP\Frame\Command\Check\Domain as CheckDomain;

$epp = new koneksi;
$epp_client = $epp->login();
$frame = new CheckDomain();
$frame->addDomain('pandi.id');
$response = $epp_client->request($frame);
$r = new Response();
$return = array();
$code = NULL;

if (!($response instanceof  $r)) {
    var_dump($e);die;
}
$response_data = $response->data();
$result = $response->results()[0];
$code = $result->code();
$epp_client->close(); //close
```

> notes: setiap koneksi ke epp sebaiknya login baru mengirim perintah (command) dan setiap selesai untuk menutup koneksi agar lebih efisien

# Persiapan Koneksi ke Epp registry pandi
## 1. check dengan telnet 
```sh
# request example on terminal
telnet epp-prod.pandi.id 700
```
```sh
# example response
Trying 45.126.58.116...
Connected to epp-prod.pandi.id.
Escape character is '^]'.

```
> jika response belum connected maka hubungi ke pandi untuk whitelist IP , dan pastikan IP anda adalah indonesia region

## 2. check dengan openssl
```sh
# example request on terminal
openssl s_client -connect epp-prod.pandi.id:700

```
> response harus berisi informasi ssl dari server dan greeting

## 3. Upload SSL yang masih berlaku ke registrar-login.pandi.id
## 4. Masukan Ip anda ke Whitelist di panel registrar-login.pandi.id
## 5. konek ke epp dengan contoh code di atas
> yang perlu anda siapakan adalah registrarId, epp password, dan SSL yang sudah di upload

## 6. Jika Error
- jika error unauthorized maka pastikan kembali IP, registrarId, epp password anda benar
- jika error hanya null / "" / kosong pastikan ssl anda benar untuk susunan, atau belum expired

# Status Domain Pada Registry
## Client Assigned
1. clientHold
    
    Status ini mengizinkan registrar untuk menghapus nama domain dari DNS misal, pembayaran belum diterima dari registrant (user). Status ini tidak mempengaruhi kewenangan pihak registrar untuk melakukan update, transfer, hapus atau renew nama domain. Status ini hanya berpengaruh terhadap nama domain di eksistensi DNS, umumnya domain tidak akan dapat diakses.

2. clientTransferProhibited

    Status ini menginstruksikan Registry untuk menolak semua permintaan dari registrar lain dalam hal mentransfer nama domain ke sponsorship mereka.

3. clientUpdateProhibited

    Status ini menginstruksikan Registry untuk menolak semua permintaan update detil terhadap nama domain oleh Registrar. Tujuan dari ini adalah untuk melakukan “safety lock” terhadap informasi data nama domain sebelum itu dapat diupdate oleh registrar.

4. clientDeleteProhibited

    Status ini sama dengan clientUpdateProhibited, menginstruksikan Registry untuk menolak semua permintaan untuk penghapusan nama domain. Nama domain tetap dapat dihapus oleh registry jika sudah expire.

5. clientRenewProhibited

    Status ini sama dengan status clientUpdateProhibited dan clientDeleteProhibited, menginstruksikan Registry untuk menolak semua permintaan untuk merenew sebuah domain. Nama domain tetap dapat di auto-renew oleh Registry jika domain sudah memasuki waktu expire.


## Server Assigned
1. serverHold

    Status ini mengindikasikan bahwa Registry telah memilih untuk menghapus nama domain dari DNS untuk alasan finansial, legal atau operasional

2. serverTransferProhibited

    Status ini mengindikasikan bahwa Registry akan menolak semua permintaan untuk transfer atau melepas nama domain ke registrar lain.

3. serverUpdateProhibited

    Status ini mengindikasikan bahwa Registry akan menolak semua permintaan untuk melakukan update terhadap nama domain.

4. serverDeleteProhibited

    Status ini mengindikasikan bahwa Registry akan menolak semua permintaan untuk penghapusan nama domain. Status ini terkadang diset oleh Registry untuk pencegahan domain terhapus otomatis.

5. serverRenewProhibited

    Status ini mengindikasikan bahwa Registry akan menolak semua permintaan untuk renew nama domain. Registry terkadang menset status ini untuk alasan teknis dan kebijakan. Nama domain yang mendapatkan status ini akan berubah status menjadi pendingDelete 5 hari setelah waktu expire.

6. pendingDelete/RedemptionPeriod

    Status ini mengindikasikan bahwa Registry telah menerima permintaan untuk penghapusan nama domain, dan domain ini sudah masuk ke jadwal penghapusan dalam siklus domain. Di case tertentu Registrar masih bisa merestore nama domain menggunakan sistem Redemption Grace Period dari Registry.

7. pendingTransfer

    Status ini mengindikasikan bahwa Registry telah menerima permintaan untuk transfer nama domain ke registrar lain, dan permintaan masih dalam proses pending oleh losing registrar.

8. inactive

    Status ini dapat mengandung arti:

        a. nama domain tidak memiliki nameserver

        b. host atau informasi kontak domain tidak terkait ke nama domain (tidak resolve ke domain tertentu)


# Suspend Domain
- Suspend domain bisa di lakukan dengan cara update status domain menjadi clientHold agar domain tidak bisa di akses karena sudah terhapus dari DNS
- Suspend domain juga bisa di lakukan dengan cara update nameserver dengan nameserver yang lain agar domain mengarah ke alamat IP tertentu .