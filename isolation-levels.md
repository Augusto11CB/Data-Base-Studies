# Isolation Levels

* Serializable.
* Repeatable reads.
* Read committed.
* Read uncommitted.



### **Read Committed Isolation Level**

* In the Read Committed isolation level, a transaction only sees data that has been committed by other transactions. This means that if another transaction is in the process of modifying data, those changes are not visible to a transaction with Read Committed isolation until they are committed.
* This isolation level prevents dirty reads, as transactions can only read committed data. However, it does not prevent non-repeatable reads or phantom reads.
* In terms of concurrency, Read Committed allows other transactions to modify data that has been read by a transaction, which can result in non-repeatable reads or phantom reads.

### **Repeatable Read Isolation Level**

* In the Repeatable Read isolation level, once a transaction reads a piece of data, it ensures that subsequent reads of that data within the same transaction will return the same result, even if other transactions modify or delete the data in the meantime.
* This isolation level prevents both dirty reads and non-repeatable reads. Dirty reads are prevented because transactions only read committed data. Non-repeatable reads are prevented because once a transaction reads data, it remains unchanged for the duration of the transaction.
* However, Repeatable Read isolation does not prevent phantom reads, as it doesn't lock entire ranges of rows that satisfy a condition.

### **Serializable Isolation Level**

* Serializable is the strongest isolation level in SQL. It provides full ACID (Atomicity, Consistency, Isolation, Durability) guarantees by effectively serializing transactions.
* In the Serializable isolation level, transactions are executed as if they are serialized, one after the other. This ensures that each transaction sees a consistent snapshot of the database, unaffected by concurrent transactions.
* Serializable isolation prevents all three types of anomalies: dirty reads, non-repeatable reads, and phantom reads. It achieves this by placing locks on the data that a transaction reads or writes, preventing other transactions from modifying that data until the transaction completes.
* While Serializable isolation ensures the highest level of consistency, it can also lead to decreased concurrency and potentially higher contention for resources, as transactions may need to wait for locks to be released.

### **Main Differences**

* **Consistency Guarantees**: Read Committed offers the lowest level of consistency guarantees, followed by Repeatable Read, and Serializable offers the highest level of consistency guarantees.
* **Concurrency vs. Consistency**: Read Committed allows for higher concurrency but sacrifices consistency to some extent. Serializable, on the other hand, sacrifices concurrency for stronger consistency guarantees.
* **Locking**: Repeatable Read and Serializable isolation levels use locking mechanisms to prevent non-repeatable reads and phantom reads, whereas Read Committed doesn't use locking to the same extent.
* **Phantom Reads**: Serializable isolation is the only level that effectively prevents phantom reads by locking entire ranges of rows that satisfy a condition until the transaction completes.



### Reference

\[1] MIHALCEA, Vlad. _JPA Entity Version Property and Hibernate_. **Disponível em:** [https://vladmihalcea.com/jpa-entity-version-property-hibernate/](https://vladmihalcea.com/jpa-entity-version-property-hibernate/). Acesso em: 11 maio 2024.

\[2] MIHALCEA, Vlad. _A beginner’s guide to database locking and the lost update phenomena_. **Disponível em:** [https://vladmihalcea.com/a-beginners-guide-to-database-locking-and-the-lost-update-phenomena/](https://vladmihalcea.com/a-beginners-guide-to-database-locking-and-the-lost-update-phenomena/). Acesso em: 11 maio 2024.

\[3] MIHALCEA, Vlad. _A beginner’s guide to ACID and database transactions._ **Disponível em:** [https://vladmihalcea.com/a-beginners-guide-to-acid-and-database-transactions/](https://vladmihalcea.com/a-beginners-guide-to-acid-and-database-transactions/). Acesso em: 11 maio 2024.
