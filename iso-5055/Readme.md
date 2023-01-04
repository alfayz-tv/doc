# Apa ISO 5055

> ISO 5055 adalah standar kualitas perangkat lunak yang menghitung ukuran kualitas berdasarkan jumlah kelemahan kritis dalam perangkat lunak. Secara khusus, ini melihat empat karakteristik kualitas perangkat lunak: keamanan, keandalan, efisiensi kinerja, dan pemeliharaan.

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