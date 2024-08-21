Untuk menghapus semua images dan containers di Docker, Anda bisa menggunakan perintah berikut di terminal:

### 1. Hapus Semua Containers
Pertama, hapus semua containers, baik yang sedang berjalan maupun yang berhenti:

```bash
docker rm -f $(docker ps -a -q)
```

- `docker ps -a -q` mengembalikan daftar ID semua containers.
- `docker rm -f` memaksa penghapusan semua containers tersebut.

### 2. Hapus Semua Images
Setelah semua containers dihapus, Anda bisa menghapus semua images:

```bash
docker rmi -f $(docker images -a -q)
```

- `docker images -a -q` mengembalikan daftar ID semua images.
- `docker rmi -f` memaksa penghapusan semua images tersebut.

### 3. Bersihkan Data Docker (Opsional)
Jika Anda ingin menghapus semua data lain yang tersimpan oleh Docker seperti networks dan volumes, Anda bisa menggunakan:

```bash
docker system prune -a --volumes
```

- `-a` menghapus semua images yang tidak terkait dengan containers apa pun.
- `--volumes` juga menghapus semua volumes yang tidak digunakan.

**Catatan**: Pastikan Anda tidak memerlukan data apa pun sebelum menjalankan perintah ini, karena data yang dihapus tidak dapat dipulihkan.