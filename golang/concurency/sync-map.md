# sync.Map
`sync.Map` adalah tipe data yang tersedia di bahasa pemrograman Go (Golang) yang digunakan untuk mengimplementasikan peta (map) yang dapat digunakan secara aman oleh beberapa goroutine (thread) secara konkuren. `sync.Map` dirancang untuk mengatasi masalah konkurensi saat mengakses dan memodifikasi data dalam peta (map) tanpa perlu mengunci seluruh peta.

Berikut adalah beberapa karakteristik `sync.Map`:

1. Aman untuk konkurensi: `sync.Map` dirancang untuk digunakan secara aman oleh beberapa goroutine tanpa perlu mengunci peta secara keseluruhan. Ini berarti bahwa beberapa goroutine dapat membaca dan menulis ke `sync.Map` secara bersamaan tanpa khawatir tentang deadlock atau race condition.

2. Dynamic Key-Value Pairs: `sync.Map` dapat menampung pasangan kunci-nilai (key-value pairs) dengan tipe data yang berbeda-beda untuk setiap elemen dalam peta. Ini berarti Anda dapat menyimpan data dengan berbagai jenis kunci dan nilai dalam peta yang sama.

3. Opsi Load dan Store Atomik: Anda dapat menggunakan metode `Load` untuk membaca nilai dari peta dan `Store` untuk menambah atau mengganti nilai dalam peta. Kedua operasi ini dilakukan secara atomik, sehingga tidak memerlukan penguncian eksternal.

Berikut contoh penggunaan `sync.Map` dalam Go:

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var m sync.Map

    // Menambahkan data ke sync.Map
    m.Store("key1", "value1")
    m.Store("key2", "value2")

    // Membaca data dari sync.Map
    if val, ok := m.Load("key1"); ok {
        fmt.Println("Value of key1:", val)
    }

    // Menghapus data dari sync.Map
    m.Delete("key2")

    // Iterasi melalui semua elemen dalam sync.Map
    m.Range(func(key, value interface{}) bool {
        fmt.Printf("Key: %v, Value: %v\n", key, value)
        return true // Kembalikan true untuk melanjutkan iterasi
    })
}
```

Penting untuk diingat bahwa `sync.Map` memiliki kinerja yang lebih rendah daripada peta (map) biasa jika digunakan secara intensif oleh banyak goroutine, jadi hanya digunakan ketika konkurensi sangat diperlukan. Jika Anda tidak perlu konkurensi, lebih baik menggunakan peta biasa (map) dengan penguncian manual untuk menghindari overhead yang ditimbulkan oleh `sync.Map`.