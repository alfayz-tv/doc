# Apa ISO 5055

> ISO 5055 adalah standar kualitas perangkat lunak yang menghitung ukuran kualitas berdasarkan jumlah kelemahan kritis dalam perangkat lunak. Secara khusus, ini melihat empat karakteristik kualitas perangkat lunak: keamanan, keandalan, efisiensi kinerja, dan pemeliharaan.

# Clause of iso 5055:
1. Scope
2. Conformance
3. Normative References
4. Terms and Definitions
5. Symbols (And abbreviated terms)
6. Weaknes included in Quality Measures and Representation Metamodels
7. List Of ASCQM Weakness
8. ASCQM Wekaness Detection Patterns
9. Calculation Of the Quality Measures

# 1. Scope
```
calculated from detecting and counting violations of good
architectural and coding practices in the source code that could result in unacceptable operational risks
or excessive costs

```

# 2. Conformance / Kesesuaian
- Autometed / Otomatis
> The analysis of the source code and counting of weaknesses shall be fully automated.
- Objective
> tidak perlu interfensi manusia, serta hasil akan selalu sama walau proses di ulangi
- Transparent
> semua sumber dengan jelas kode (termasuk versi)
- Verifiable
> Kesesuaian dengan spesifikasi ini mensyaratkan bahwa implementasi harus menyatakan
asumsi / heuristik yang digunakannya dengan detail yang cukup sehingga perhitungannya mungkin
diverifikasi secara independen oleh pihak ketiga. Selain itu, semua input yang digunakan harus dijelaskan dengan jelas
dan diperinci sehingga dapat diaudit oleh pihak ketiga.


# 3. Normative References
```

https://www.omg.org/spec/SPMS/1.2/
https://www.omg.org/spec/KDM/1.4
https://www.omg.org/spec/XMI/2.5.1
https://www.omg.org/spec/AFP/
```

# 4. Terms And Definitions
## 4.1. Automated Function Points
```

```
# 5. Symbols (and Abbreviated Terms)
```
AFP — Automated Function Points
ASCMM — Automated Source Code Maintainability Measure
ASCPEM — Automated Source Code Performance Efficiency Measure
ASCQM — Automated Source Code Quality Measure
ASCRM — Automated Source Code Reliability Measure
ASCSM — Automated Source Code Security Measure
CWE — Common Weakness Enumeration
CISQ — Consortium for IT Software Quality
KDM — Knowledge Discovery Metamodel
SMM — Structured Metrics Metamodel
SPMS — Structured Pattern Metamodel Standard

```

# 6. Weaknesses Included in Quality Measures and Representation Metamodels
## 6.1 Purpose
```
Tujuan dari klausul ini adalah untuk menyebutkan satu per satu kelemahan yang terdapat pada setiap tindakan dan untuk memberikan sebuah
deskripsi sederhana dari setiap ukuran.
```

## 6.2  Software Product Inputs
- The entire source code for the application being analyzed.
- All materials and information required to prepare the application for production.
- A list of vetted libraries that are being used to sanitize data against potential attacks.
- What routines/API calls are being used for remote authentication, to any custom initialization
and clean up routines, to synchronize resources, or to neutralize accepted file types or the names
of resources.

## 6.3 Automated Source Code Quality Measure Elements
```
look Annex C.
```

## 6.4 Automated Source Code Maintainability Measure Element Descriptions

## 6.5 Automated Source Code Performance Efficiency Measure Element Descriptions

## 6.6 Automated Source Code Reliability Measure Element Descriptions

## 6.7 Automated Source Code Security Measure Element Descriptions


## 6.8 Introduction to the Specification of Quality Measure Elements
```
Ukur Elemen dengan konvensi berikut:
• Elemen struktural yang termasuk dalam pola kelemahan direpresentasikan dalam Knowledge Discovery
Metamodel (KDM).
• Hubungan antar elemen struktur yang merupakan pola kelemahan direpresentasikan dalam
Software Patterns Metamodel Standard (SPMS) untuk menghitung ukuran pada tingkat kelemahan.
```

## 6.9 Knowledge Discovery Metamodel (KDM)
```

```
## 6.10 Software Patterns Metamodel Standard (SPMS)
```


```
## 6.11 Reading guide
```
- Sub-clause 7.x represents the Software Quality characteristic addressed by the associated
weakness patterns.
- Sub-clause 7.x.y represents the SPMS modeling associated with a weakness pattern for a specific
weakness associated with the Software Quality characteristic

```
# 7. List of ASCQM Weaknesses

# 8. ASCQM Weakness Detection Patterns
## 8.1 Specification of Detection Patterns
> Pola deteksi memberikan panduan untuk deteksi otomatis kelemahan yang disebutkan dalam Klausul 7.
Setiap kelemahan mungkin memiliki beberapa contoh berbeda dalam kode sumber. Dengan demikian, kelemahan mungkin
terkait dengan beberapa pola deteksi yang berbeda. Setiap pola deteksi dapat dikaitkan dengan
kelemahan dalam beberapa ukuran kualitas yang berbeda. Ada 135 pola deteksi yang terkait dengan
kelemahan dalam Pengukuran Kualitas Kode Sumber Otomatis. Jumlah ini akan bertambah karena lebih banyak deteksi
pola ditemukan dan ditentukan.
Pola Deteksi menggunakan mikro-KDM untuk memberikan perincian yang lebih besar pada spesifikasi kelemahannya
pola. Kendala semantik tambahan diperlukan untuk mengoordinasikan produsen dan konsumen KDM
model untuk menggunakan lapisan Elemen Program KDM untuk aplikasi analisis kontrol dan aliran data, juga
seperti untuk memberikan lebih banyak presisi untuk Resource Layer dan Abstraction Layer. Mikro-KDM tercapai
ini dengan membatasi perincian elemen aksi daun dan maknanya dengan menyediakan himpunan
tindakan mikro dengan semantik yang telah ditentukan. Micro-KDM memperlakukan aksi makro asli sebagai wadah yang
memiliki tindakan mikro tertentu dengan semantik yang telah ditentukan. Jadi, semantik yang tepat dari tindakan makro adalah
didefinisikan. Micro-KDM membatasi pola bagaimana memetakan pernyataan sistem yang ada sebagai
ditentukan oleh bahasa pemrograman menjadi KDM.


# 9. Calculation of the Quality Measures
> Setelah meninjau beberapa alternatif, hitungan total pelanggaran aturan kualitas dipilih sebagai yang terbaik
opsi untuk ukuran dasar untuk masing-masing dari empat karakteristik kualitas perangkat lunak yang tercakup dalam hal ini
spesifikasi. Ukuran karakteristik kualitas perangkat lunak sering dinilai pada komponen
tingkat dan kemudian dikumpulkan untuk mengembangkan skor keseluruhan untuk aplikasi. Namun, mencetak gol di
tingkat komponen ditolak karena banyak pelanggaran aturan kualitas tidak dapat diisolasi menjadi satu
komponen, melainkan melibatkan interaksi di antara beberapa komponen. Oleh karena itu, masing-masing Otomatis
Skor Ukuran Kualitas Kode Sumber dihitung sebagai jumlah dari elemen ukuran kualitasnya yang dihitung
di seluruh aplikasi.

```
Occurrence Count of Weakness x = Σ (Occurrences of ASCQM-y)
Where x = a CWE weakness (CWE-119, CWE-120, etc.)
y = a detection pattern for weakness x
and
Occurrence Count of Weakness Category x = Σ (Occurrence Count of ASCQM-y)
Where x = a software quality characteristic (Reliability, Security, Performance Efficiency,
Maintainability)
y = a detection pattern for quality characteristic x

```
![image iso](https://github.com/alfayz-tv/doc/blob/master/images/iso.png)

## pada dasarnya pengukuran di lakukan pada level source code, meliputi:


|  Software Quality Characteristic | Unit Level  |  Technology Level |  System Level |
|---|---|---|---|
| Security  |   Missing initialization  | Use of hard-coded credentials | Buffer Overflow |
|           |   Improper validation of array index  | References to release resources | Input validation |
|           |   Improper locking    |   | SQL injection |
|           |   Failure to use vetted libraries or frameworks |     | Cross-site scripting|
|           |   Uncontrolled format string |        | Secure architecture design compliance |
|Reliability| Error and Exception handling | Multi-layer design compliance | Exception handling through transactions |
|           | Complexity of algorithms | Software manages data integrity and consistency | Protecting state in multi-threaded environments |
|           | Error prone programming |             | Resource bounds management |
|           | Safe use of inheritance and polymorphism |        | Null pointers dereference detection |
|           | Managing allocated resources, timeouts |      |   |
|Efficiency | Expensive computations in loops | Compliance with object-oriented best practices | Appropriate interactions with expensive or remote resources |
|           | Compliance with garbage collection best practices | Compliance with SQL best practices | Data access performance and data management |
|           |       | Memory, network, and disk space management | Centralized handling of client requests |
|           |       |       | Use of middle tier components versus procedures/DB functions |
|           |       |       | Algorithm complexity |
|Maintainability | High cyclomatic complexity | Unstructured and duplicated code | Tightly coupled modules |
|           | Over-parameterization of methods | Controlled level of dynamic coding | Strict hierarchy of calling between architectural layers |
|           | Hard coding of literals | Compliance with OO best practices | Excessive horizontal layers |
|           | Excessive component size |        | Encapsulated data access |