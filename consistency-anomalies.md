# Consistency Anomalies

* Dirty reads
* Non-repeatable read
* Phantom reads
* Lost Updates
* Read Skew
* &#x20;Write Skew



### **Dirty Reads**

* **Definition**: Dirty reads occur when one transaction reads data that has been modified by another transaction but not yet committed. This means the data being read might be incomplete or invalid since the modifying transaction might rollback, leaving the data in an inconsistent state.
* **Prevention**:
  * **Read Committed Isolation Level**: In this isolation level, a transaction only reads data that has been committed. It prevents dirty reads by ensuring that a transaction can only access committed data. If a transaction modifies data but hasn't committed yet, other transactions won't be able to read that data until it's committed.
  * **Repeatable Read and Serializable Isolation Levels**: Both of these isolation levels also prevent dirty reads by ensuring that a transaction reads only committed data. They provide higher levels of isolation compared to Read Committed.

### **Non-repeatable Read**

* **Definition**: Non-repeatable reads occur when a transaction reads the same data multiple times within a transaction, but the data changes between reads due to other transactions committing changes. This can lead to inconsistent results within a single transaction.
* **Prevention**:
  * **Repeatable Read Isolation Level**: In this isolation level, a transaction ensures that once it reads a piece of data, it will remain unchanged throughout the transaction. It prevents other transactions from modifying or deleting data that has been read by a transaction until the reading transaction completes.
  * **Serializable Isolation Level**: This level offers even stronger guarantees than Repeatable Read. It prevents non-repeatable reads by effectively serializing transactions, ensuring that each transaction sees a consistent snapshot of the database regardless of concurrent changes.

### **Phantom Reads**

* **Definition**: Phantom reads occur when a transaction reads a set of rows that satisfy a certain condition, but another transaction inserts or deletes rows that also satisfy that condition before the first transaction completes. As a result, the first transaction may see additional rows (phantoms) or miss rows that satisfy its condition, leading to inconsistent results.
* **Prevention**:
  * **Serializable Isolation Level**: This is the only isolation level that prevents phantom reads effectively. Serializable isolation level ensures that a transaction sees a consistent snapshot of the database and locks entire ranges of rows that satisfy a condition until the transaction completes. This prevents other transactions from inserting or deleting rows that would affect the result of the first transaction's query.



### Reference

\[1] **Vlad Mihalcea**. _JPA Entity Version Property and Hibernate_. **Disponível em:** [https://vladmihalcea.com/jpa-entity-version-property-hibernate/](https://vladmihalcea.com/jpa-entity-version-property-hibernate/). Acesso em: 11 maio 2024.

\[2] **Vlad Mihalcea**. _A beginner’s guide to database locking and the lost update phenomena_. **Disponível em:** [https://vladmihalcea.com/a-beginners-guide-to-database-locking-and-the-lost-update-phenomena/](https://vladmihalcea.com/a-beginners-guide-to-database-locking-and-the-lost-update-phenomena/). Acesso em: 11 maio 2024.

\[3] **Vlad Mihalcea**. _A beginner’s guide to ACID and database transactions._ **Disponível em:** [https://vladmihalcea.com/a-beginners-guide-to-acid-and-database-transactions/](https://vladmihalcea.com/a-beginners-guide-to-acid-and-database-transactions/). Acesso em: 11 maio 2024.
