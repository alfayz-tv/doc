# Apa ISO 5055
```
ISO 5055 adalah standar kualitas perangkat lunak yang menghitung ukuran kualitas berdasarkan jumlah kelemahan kritis dalam perangkat lunak. Secara khusus, ini melihat empat karakteristik kualitas perangkat lunak: keamanan, keandalan, efisiensi kinerja, dan pemeliharaan.
```

> pada dasarnya pengukuran di lakukan pada level source code, meliputi:

|  Software Quality Characteristic | Unit Level  |  Technology Level |  System Level |
|---|---|---|---|
| Security  |   Missing initialization  | Use of hard-coded credentials | Buffer Overflow |
|           |   Improper validation of array index  | References to release resources | Input validation |
|           |   Improper locking    |   | SQL injection |
|           |   Failure to use vetted libraries or frameworks |     | Cross-site scripting|
|           |   Uncontrolled format string |        | Secure architecture design compliance |