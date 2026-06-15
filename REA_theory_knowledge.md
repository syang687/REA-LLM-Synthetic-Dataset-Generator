# REA Revenue-Cycle Design - Knowledge File (Parts 1-3)

Read this ENTIRE file before generating anything. It defines Rule 1 (Part 1), the eight-step REA method
with the fourteen cardinality rules and the seven-table schema as Rule 2 (Part 2), and the table
relationships as Rule 3 (Part 3).

## Part 1 - Task Explanation Prompt

You are an excellent designer of relational databases using the REA (Resources, Events, Agents) model.
Given information about an accounting cycle (e.g., the revenue cycle) and its related business processes,
you break it down into a sequence of database design actions. Set this as Rule 1. Obey this rule explicitly
and follow it for all following instructions unless you are asked to ignore the rule.

Before generating any data, reason step by step through the eight REA design steps in Part 2.

## Part 2 - Action Explanation Prompt

Your task is to create a set of Tables structure based on the REA (Resources, Events, Agents) model,
specifically focusing on the revenue cycle. You are responsible for creating a relational database for a
firms' revenue cycle by identifying and modeling key entities and relationships within datasets related to
revenue transactions that will be defined below.

Resources, Events, and Agents in the REA model
1. Resources are considered equivalent to "asset" with one exception: economic resources in the schema do
   not automatically include claims including "accounts receivable". Instead, "accounts receivable" can be
   derived by analyzing the imbalance between recorded sales and corresponding cash receipts.
2. Events are defined as the class of phenomena which reflect changes in resources resulting from
   production, exchange, consumption, and distribution.
3. Agents include persons (e.g., employee and customer) who participate in the economic events of the
   enterprise or who are responsible for subordinates' participation.

To build the relational database we should strictly obey the following 8 steps. Follow the 8 steps unless
you are asked to ignore the rule.

Step 1: Identify the events about which management wants to collect information in the revenue cycle:
(i) Sales; (ii) Cash Receipts.

Step 2: Identify the resources affected by the events in the revenue cycle: (i) Inventory; (ii) Cash.

Step 3: Identify the agents who participated in the events in the revenue cycle: (i) Employees; (ii) Customers.

Step 4: Identify relationships between events, resources, and agents in the revenue cycle:
(i) Sales are related to Inventory; (ii) Cash Receipts are related to Cash; (iii) Sales are related to Cash
Receipts; (iv) Sales are related to Employees; (v) Sales are related to Customers; (vi) Cash Receipts are
related to Employees; (vii) Cash Receipts are related to Customers.

Step 5: Determine the cardinalities of each relationship.
- i. Sales to Inventory Relationship: Sales should involve at least one or many inventory items (1:M relationship).
- ii. Inventory to Sales Relationship: An inventory item can be related to multiple sales events (1:M relationship).
- iii. Cash Receipts to Sales Relationship: A cash receipt must be related to only one Sales event.
- iv. Sales to Cash Receipts Relationship: A sale can be related to zero, one, or many cash receipts. A sale may have no receipt (a credit sale, where payment is deferred), one receipt (paid in full), or several receipts (partial or installment payments made over time). This one-to-zero-or-many (1:0..M) relationship is what lets accounts receivable be derived from sales that are not yet fully settled.
- v. Cash to Cash Receipts Relationship: The Cash to Cash Receipts relationship is one-to-many because cash can exist without being linked to any specific cash receipt, and it can be associated with many cash receipts as multiple transactions increase the cash balance.
- vi. Cash Receipts to Cash Relationship: The Cash Receipts to Cash relationship is one and only one because each cash receipt increases exactly one cash resource.
- vii. Sales to Employees: Each sales event is handled by exactly one employee (one and only one).
- viii. Sales to Customer: Each sales event is associated with exactly one customer (one and only one).
- ix. Customers to Sales: A customer can be linked to multiple sales events (one to many relationship).
- x. Employees to Sales: An employee can be responsible for multiple sales events (one to many relationship).
- xi. Cash Receipts to Customers: Each cash receipt is linked to exactly one customer who made the payment.
- xii. Cash Receipts to Employees: Each cash receipt is processed by exactly one employee responsible for handling the transaction.
- xiii. Customers to Cash Receipts: A customer can have multiple cash receipts based on the number of payments made.
- xiv. Employees to Cash Receipts: An employee can process multiple cash receipts depending on their role in handling transactions.

Step 6: Identify the attributes of events, resources, and agents.
The revenue cycle begins with the sales event, where goods or services are provided to a customer. This event is managed by an employee who oversees the transaction from initiation to completion. The sale directly affects the inventory resource, as the items sold are deducted from the current stock levels. The inventory table is updated accordingly to reflect these changes. Following the sale, there may be an associated Cash Receipt event (only when the customer is paying in cash). Each cash receipt is linked to a specific sales event and is processed by an employee responsible for handling financial transactions. The cash resource is affected during this process, as the receipt of payment results in an increase in firm's cash balance. Customers can make multiple payments over time, leading to several cash receipts being recorded related to the same sales event. Employees are responsible for managing sales transactions, ensuring accurate inventory updates, and processing payments efficiently. Customers are key agents as well, engaging in sales transactions and fulfilling payment obligations.

Step 7: Identify information processes.
(i) Sales Event Processing: Recording details of each Sales event, linking each sale to the relevant Inventory items, associating each sale with the responsible Employee, connecting each sale to the corresponding Customer, and establishing relationships between Sales and Cash Receipts (if applicable).
(ii) Inventory Tracking: Updating Inventory records to reflect items affected by Sales events. Managing the relationship between each inventory item and multiple sales events.
(iii) Cash Receipts Processing: Recording Cash Receipts when payments are made by Customers. Associating each Cash Receipt with a specific Sales event (if applicable). Linking Cash Receipts to the relevant Cash resource. Connecting each Cash Receipt to the responsible Employee and the paying Customer.

Step 8: Design the structure of the data repository. All generated tables must have all required attributes. Do not add or remove attributes. Use the table names and file names exactly as specified. (Step 8 is aligned to Step 5: the Order_Line line-item table realizes the 1:M Sales-Inventory relationship; Sales no longer carries Item_ID; Cash carries no foreign key.)

| # | Table / file name | Primary key | Foreign keys | Additional attributes |
|---|---|---|---|---|
| 1 | Sales.csv | Order_ID | Employee_ID -> Employee, Customer_ID -> Customer | Sales_Date, Sales_Amount, Sales_Currency |
| 2 | Order_Line.csv | Line_ID | Order_ID -> Sales, Item_ID -> Inventory | Quantity, Unit_Price, Line_Amount |
| 3 | Cash_Receipts.csv | Receipt_ID | Order_ID -> Sales, Account_ID -> Cash, Employee_ID -> Employee, Customer_ID -> Customer | Receipt_Date, Amount_Received |
| 4 | Inventory.csv | Item_ID | (none) | Item_Name, Description, Selling_Price, Quantity_On_Hand |
| 5 | Cash.csv | Account_ID | (none) | Bank_Name, Bank_Account_Number, Balance |
| 6 | Employee.csv | Employee_ID | (none) | Employee_Name, Employee_Position, Hire_Date, Department, Employee_Phone_Number, Employee_Salary |
| 7 | Customer.csv | Customer_ID | (none) | Customer_Name, Customer_Phone_Number, Customer_Email, Customer_Billing_Address, Customer_Shipping_Address, Customer_Credit_Limit |

The tables should reflect real-life data (e.g., customer address "132 Jefferson Street, Hoboken, NJ 07030"; customer name "John T. Smith" or "Aaron Fu"; Item_Name "4567 Alloy Wheels"; Order_ID "O0001"; Line_ID "L00001"; Employee_ID "E004"; Customer_ID "C009"; Account_ID "A001"; Receipt_ID "R0001"). Set the description above as Rule 2 and obey it for all following instructions unless asked to ignore it.

## Part 3 - Input Data Explanation Prompt

We are creating a sample of tables based on the relationships in Rule 2. Each table represents a distinct
entity; relationships are established through primary and foreign keys to maintain data integrity.

Primary key: a unique identifier for each record in a table (e.g., Order_ID in Sales); no duplicate records.
Foreign key: a field in one table that references the primary key of another (e.g., Employee_ID in Sales
referencing Employee), enforcing referential integrity.

Relationships between tables
1. Sales links to Employee and Customer through foreign keys (Employee_ID, Customer_ID).
2. Order_Line links each sale to the inventory items on it (Order_ID -> Sales, Item_ID -> Inventory); this realizes the many-to-many Sales-Inventory relationship as two 1:M relations.
3. Cash Receipts connects to Sales, Cash, Employee, and Customer. A sale may have zero, one, or many receipts (partial/installment payments); each receipt belongs to exactly one sale.
4. Cash is linked from Cash Receipts through Account_ID, reflecting the flow of cash.
5. Employee and Customer are referenced in both Sales and Cash Receipts.
6. Inventory is tied to Sales through Order_Line, indicating which items were sold.

Set the description above as Rule 3 and obey it for all following instructions unless asked to ignore it.
