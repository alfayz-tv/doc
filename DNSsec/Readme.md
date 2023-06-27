# DNSsec
> ketika dns sudah menggunakan dnssec maka ketika kita mencari ns tidak langsung server dns memberi ip, tapi akan tanya dns atasnya dulu baru merespon alamat ip



## Dnskey RR Example
```
pandi.id.		116	IN	DNSKEY	257 3 13 6WPvRXOOTwKWeOGgz36UbLVWF1yaZypDPNTYw4UzT8vqG1NyfIiT+Ri2 krusW10MxaeVHeFpFKdHFpqWnXjmEg==

# desc
pandi.id => owner
116 => TTl
DNSKEY => Class
256 => Value 256 indicates that the Zone Key bit (bit 7) in the Flags field has value 1

```