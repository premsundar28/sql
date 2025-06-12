Sql triggers:  SQL Triggers are special types of stored procedures that automatically execute or "trigger" in response to certain events on a particular table or view in a database.

Key Points:
Event-driven: They run automatically when an INSERT, UPDATE, or DELETE operation occurs on the table.

Types of Triggers:

BEFORE Trigger: Executes before the event (INSERT/UPDATE/DELETE).

AFTER Trigger: Executes after the event.

INSTEAD OF Trigger: Executes in place of the event, often used in views.

Common Use Cases:
Data validation: Ensuring data integrity before insertion or updating.

Audit logging: Recording changes made to the data (e.g., who changed what and when).

Enforcing business rules: Automatically modifying data or taking actions based on specific conditions.

Syntax

create trigger [trigger_name] 
[before | after]  
{insert | update | delete}  
on [table_name]  
FOR EACH ROW
BEGIN
END;

Example:

CREATE TRIGGER update_timestamp
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
   UPDATE employees SET last_modified = NOW() WHERE employee_id = OLD.employee_id;
END;
