# Context
Context adalah paket yang sangat penting dalam bahasa pemrograman Go (Golang) yang digunakan untuk mengelola nilai-nilai yang bersifat konkurensi dan menyebar antar goroutine dalam sebuah aplikasi. Context digunakan untuk mengendalikan pembatalan, timeout, dan nilai-nilai lain yang perlu berpindah antar goroutine dalam aplikasi yang berjalan konkuren.

Berikut adalah beberapa komponen dan aspek penting terkait dengan `context` di Go:

### Macam-Macam Context

1. **context.Background()**: Ini adalah konteks dasar yang sering digunakan sebagai titik awal untuk membuat konteks lain. Ini memiliki nilai-nilai kosong dan tidak memiliki pembatalan atau timeout.
```go
package main

import (
    "context"
    "fmt"
    "time"
)

func main() {
    ctx := context.Background()
    fmt.Println("Context created:", ctx)
}
```

2. **context.TODO()**: Ini adalah konteks yang serupa dengan `context.Background()`, tetapi lebih disarankan digunakan dalam kasus-kasus di mana Anda belum yakin apa yang harus digunakan sebagai konteks.

```go
package main

import (
    "context"
    "fmt"
)

func main() {
    ctx := context.TODO()
    fmt.Println("Context created:", ctx)
}

```

3. **context.WithCancel(parentContext)**: Fungsi ini membuat konteks baru yang dapat dibatalkan. Ketika fungsi `cancel()` dipanggil pada konteks ini, semua goroutine yang menggunakan konteks ini akan mendapatkan sinyal untuk berhenti.

```go
package main

import (
    "context"
    "fmt"
    "time"
)

func main() {
    parent := context.Background()
    ctx, cancel := context.WithCancel(parent)

    go func() {
        // Goroutine melakukan pekerjaan yang berpotensi lama
        time.Sleep(2 * time.Second)
        // Membatalkan konteks
        cancel()
    }()

    select {
    case <-ctx.Done():
        fmt.Println("Context canceled")
    }
}

```
4. **context.WithTimeout(parentContext, timeout)**: Fungsi ini membuat konteks baru dengan batas waktu. Ketika waktu timeout tercapai, konteks ini akan dibatalkan secara otomatis.

```go
package main

import (
    "context"
    "fmt"
    "time"
)

func main() {
    parent := context.Background()
    ctx, cancel := context.WithTimeout(parent, 3*time.Second)

    go func() {
        // Goroutine melakukan pekerjaan yang berpotensi lama
        time.Sleep(4 * time.Second)
        // Membatalkan konteks (akan terlambat)
        cancel()
    }()

    select {
    case <-ctx.Done():
        fmt.Println("Context canceled due to timeout")
    }
}

```

5. **context.WithDeadline(parentContext, deadline)**: Fungsi ini membuat konteks baru dengan batas waktu tertentu (deadline). Konteks ini akan dibatalkan ketika waktu deadline tercapai.

```go
package main

import (
    "context"
    "fmt"
    "time"
)

func main() {
    parent := context.Background()
    deadline := time.Now().Add(2 * time.Second)
    ctx, cancel := context.WithDeadline(parent, deadline)

    go func() {
        // Simulate some work that may take longer than the deadline
        time.Sleep(3 * time.Second)
        // Try to cancel the context (won't work after the deadline)
        cancel()
    }()

    select {
    case <-ctx.Done():
        fmt.Println("Context canceled due to deadline")
    }
}


```
6. **context.WithValue(parentContext, key, value)**: Fungsi ini digunakan untuk menyimpan nilai khusus dalam konteks, yang dapat diambil oleh goroutine yang membutuhkannya. Ini adalah cara untuk mengirim data antar goroutine secara konkuren.

```go
package main

import (
    "context"
    "fmt"
)

type key string

func main() {
    ctx := context.WithValue(context.Background(), key("user"), "john_doe")

    user := ctx.Value(key("user")).(string)
    fmt.Println("User:", user)
}
```
### Kelebihan Context:

1. **Kontrol Pembatalan**: Context memungkinkan Anda untuk dengan mudah membatalkan atau menghentikan goroutine yang berjalan dalam aplikasi konkuren jika diperlukan, tanpa harus menghancurkan seluruh aplikasi.

2. **Timeout dan Deadline**: Context memungkinkan Anda untuk menetapkan timeout atau batas waktu untuk operasi yang mungkin membutuhkan waktu lama. Ini membantu mencegah goroutine terjebak dalam operasi yang tidak terbatas waktu.

3. **Koordinasi Antar Goroutine**: Context dapat digunakan untuk mengirim data antar goroutine dalam cara yang aman secara konkuren menggunakan `context.WithValue()`.

4. **Hierarki Konteks**: Konteks dapat dibuat dalam hierarki, di mana konteks anak dapat dibuat dari konteks induk. Ini memungkinkan pengelolaan sumber daya konkurensi yang lebih baik.

### Kekurangan Context:

1. **Overhead**: Penggunaan konteks yang berlebihan dalam aplikasi dapat menyebabkan overhead karena setiap goroutine yang menggunakan konteks harus mengelola struktur data konteksnya sendiri.

2. **Kode yang Sulit Dibaca**: Penggunaan konteks yang berlebihan atau tidak tepat dalam kode dapat membuatnya lebih sulit dibaca dan memahami.

3. **Kesalahan Penggunaan**: Penggunaan yang tidak benar dari konteks, seperti tidak membatalkan konteks yang tidak diperlukan, dapat mengakibatkan sumber daya tidak terbebaskan dengan baik atau masalah lainnya.

Penggunaan `context` yang baik dan bijak sangat penting dalam aplikasi Go yang konkuren. Ini membantu mengendalikan goroutine, menjaga aplikasi aman dari deadlock, dan memungkinkan pengelolaan sumber daya yang lebih baik. Namun, penggunaan yang tidak tepat atau berlebihan dapat mengakibatkan kode yang kompleks dan sulit dimengerti, serta meningkatkan overhead. Oleh karena itu, penting untuk menggunakan konteks secara bijak sesuai dengan kebutuhan aplikasi Anda.