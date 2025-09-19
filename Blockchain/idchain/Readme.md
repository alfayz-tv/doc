

# Check Validator Full Sync
```sh
# check log validator jika masih ada log sedang sync maka validator tersebut belumm full sync
sudo journalctl -fu validator

```

# Menambah Validator atau Collator
- menambahkan validator baru yaitu dengan mengarahkan ke bootable validtor yang sudah ada
- tunggu sampai full sync
- kita bisa mendelete validator lama jika ada masalah
- ketika sudah jalan validator tidak peduli lagi ke bootable pas kita create karena data sudah sync
