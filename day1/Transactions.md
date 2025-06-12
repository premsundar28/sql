In SQL, transactions are a way to group multiple SQL operations into a single unit of work. A transaction ensures that either all the operations succeed or none of them are applied (i.e., the database is left in a consistent state). Transactions are crucial for maintaining data integrity, especially in multi-user environments.

A transaction in SQL typically follows the ACID properties:

Atomicity – All operations within the transaction are treated as a single unit. If one part fails, the whole transaction fails.

Consistency – A transaction must bring the database from one valid state to another, maintaining all rules (e.g., constraints).

Isolation – Operations within a transaction are isolated from other transactions until the transaction is complete.

Durability – Once a transaction is committed, it is permanently saved to the database, even in the case of system failure.

Basic SQL Transaction Commands

START TRANSACTION or BEGIN TRANSACTION: Begins a new transaction.

COMMIT: Commits the transaction, making all changes permanent.

ROLLBACK: Rolls back the transaction, undoing any changes made during the transaction.

SAVEPOINT: Creates a save point within a transaction that you can roll back to without affecting the entire transaction.

RELEASE SAVEPOINT: Releases a savepoint created earlier.

SET TRANSACTION: Configures the properties of the transaction (like isolation level).

Example of SQL Transactions

Starting a Transaction:

BEGIN TRANSACTION;  -- or START TRANSACTION

Example 1: Simple Transaction

Let's say you are transferring money between two bank accounts:

BEGIN TRANSACTION;

-- Deduct 100 from Account A
UPDATE accounts
SET balance = balance - 100
WHERE account_id = 'A123';

-- Add 100 to Account B
UPDATE accounts
SET balance = balance + 100
WHERE account_id = 'B456';

-- If no errors, commit the transaction
COMMIT;


Example 2: Rolling Back a Transaction

If something goes wrong (e.g., insufficient funds in Account A), we can roll back the changes:

BEGIN TRANSACTION;

-- Deduct 100 from Account A (for example, if balance is less than 100, we want to roll back)
UPDATE accounts
SET balance = balance - 100
WHERE account_id = 'A123';

-- Add 100 to Account B
UPDATE accounts
SET balance = balance + 100
WHERE account_id = 'B456';

-- Simulate an error: insufficient funds in Account A
IF (SELECT balance FROM accounts WHERE account_id = 'A123') < 100
BEGIN
    ROLLBACK;  -- Undo all changes if insufficient funds
    PRINT 'Insufficient funds. Transaction rolled back.';
END
ELSE
BEGIN
    COMMIT;  -- Commit changes if everything is okay
END;

Example 3: Using Savepoints
Savepoints allow you to roll back part of a transaction without affecting the entire transaction.

BEGIN TRANSACTION;

-- Deduct 100 from Account A
UPDATE accounts
SET balance = balance - 100
WHERE account_id = 'A123';

-- Set a savepoint
SAVEPOINT SavePoint1;

-- Deduct 100 from Account B (this step causes an error, let's assume the account doesn't exist)
UPDATE accounts
SET balance = balance - 100
WHERE account_id = 'B999';  -- Account B999 doesn't exist

-- Rollback to the savepoint, undoing the changes made after it
ROLLBACK TO SAVEPOINT SavePoint1;

-- Commit the rest of the transaction
COMMIT;


Example 4: Setting Transaction Isolation Levels
You can control the visibility of uncommitted data in other transactions by setting isolation levels:

-- Setting the isolation level to SERIALIZABLE
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

BEGIN TRANSACTION;

-- Your SQL operations

COMMIT;
Common Isolation Levels
READ UNCOMMITTED – Allows reading uncommitted data (dirty reads).

READ COMMITTED – Only allows reading committed data.

REPEATABLE READ – Prevents other transactions from modifying data that has already been read.

SERIALIZABLE – Ensures transactions are executed in such a way that the outcome is the same as if transactions were executed one after the other.

Error Handling
SQL transactions can also handle errors using TRY...CATCH blocks (in databases like SQL Server):

BEGIN TRANSACTION;

BEGIN TRY
    -- Some operation
    UPDATE accounts
    SET balance = balance - 100
    WHERE account_id = 'A123';
    
    -- Another operation
    UPDATE accounts
    SET balance = balance + 100
    WHERE account_id = 'B456';
    
    COMMIT;  -- If no errors, commit the transaction
END TRY
BEGIN CATCH
    ROLLBACK;  -- If error occurs, rollback the transaction
    PRINT 'An error occurred. Transaction rolled back.';
END CATCH;

Important Notes:
Transactions are often automatically handled by the database engine in certain cases (like within stored procedures or through implicit commit/rollback behavior).

The exact syntax and features for managing transactions can vary slightly across different RDBMS (MySQL, PostgreSQL, SQL Server, Oracle, etc.).
