SQL Triggers 
SQL Triggers are special types of stored procedures that automatically execute in response to specific events on a table or view. These events include INSERT, UPDATE, or DELETE operations.

Key Characteristics:
Event-Driven: Triggers automatically execute when a certain event (INSERT, UPDATE, DELETE) occurs on a table.

Timing: Triggers can be defined to execute before or after the data modification.

Types of SQL Triggers:
BEFORE Trigger:

Executes before the event occurs (e.g., before an INSERT or UPDATE).

Typically used for data validation or preventing invalid changes.

AFTER Trigger:

Executes after the event takes place.

Commonly used for logging or cascading changes.

INSTEAD OF Trigger:

Executes in place of the event.

Often used in views where direct updates are not possible.

Trigger Syntax:

CREATE TRIGGER [trigger_name] 
[BEFORE | AFTER]  
{INSERT | UPDATE | DELETE}  
ON [table_name]  
FOR EACH ROW
BEGIN
    -- Trigger body
END;

Common Use Cases for SQL Triggers:
Data Validation: Ensuring data integrity before inserting or updating records.

Audit Logging: Tracking changes, such as who made modifications and when.

Enforcing Business Rules: Automatically modifying data or enforcing specific conditions (e.g., preventing negative salaries).

Understanding Trigger Execution Details:

Trigger Timing:

BEFORE Trigger: Executes before the operation (helpful for validation).

AFTER Trigger: Executes after the operation (commonly used for logging and cascading changes).

INSTEAD OF Trigger: Replaces the operation, often used in views.

Execution Context:

FOR EACH ROW: Executes the trigger for each affected row. This allows the trigger to perform row-level operations.

FOR EACH STATEMENT: Executes the trigger once per statement, regardless of how many rows are affected.

Working with OLD and NEW Data:

OLD: Refers to the values of the row before the modification (used in UPDATE and DELETE triggers).

NEW: Refers to the new values after the modification (used in INSERT and UPDATE triggers).

These references are essential for comparing old and new values, especially in UPDATE operations.

Trigger Body:

The body of the trigger can contain multiple SQL statements (e.g., INSERT, UPDATE, DELETE) or control-flow logic (e.g., IF statements).

It can also include error handling or exception management, depending on the RDBMS.

Advanced Use Cases:
Cascading Operations: Automatically update or delete related records. This is similar to foreign key constraints with the ON DELETE CASCADE option.

Data Auditing: Automatically log changes to track who modified data and when.

Preventing Invalid Operations: Reject invalid data, such as ensuring a salary cannot be negative.

Example: Preventing Negative Salary (BEFORE Trigger)

CREATE TRIGGER prevent_negative_salary
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
   IF NEW.salary < 0 THEN
      SIGNAL SQLSTATE '45000'
      SET MESSAGE_TEXT = 'Salary cannot be negative';
   END IF;
END;

Trigger Management:
Dropping a Trigger: Triggers can be dropped when no longer needed or for modifications.

DROP TRIGGER [trigger_name];

Performance Considerations:
While triggers are powerful, they can add performance overhead, especially on frequently updated tables.

Recursive Triggers: Be cautious with recursive triggers (when one trigger causes another trigger to fire), as they can lead to infinite loops or unintended behavior.

Testing: Always test triggers in a development or staging environment before applying them in production to ensure they don’t degrade performance.

Summary of Key Points:
SQL triggers are automatic and event-driven procedures that execute in response to data modification events.

They come in BEFORE, AFTER, and INSTEAD OF types, and can be used for a variety of purposes such as validation, logging, and enforcing business rules.

Careful consideration should be given to trigger timing, performance impact, and management to ensure optimal usage.
