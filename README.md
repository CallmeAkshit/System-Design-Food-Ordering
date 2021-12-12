# System-Design-Food-Ordering

Let us first see the ER Diagram for this System Design.

![DBMS ER diagram (UML notation)](https://user-images.githubusercontent.com/29516560/145729154-72d8b2e3-486b-4580-867d-85679492134f.png)

1. Customer Table - 
  This will hold all the customer personal information which would be required to identify the owner of the order
  For every row,
    There will be unique customerID (PRIMARY KEY)
    Address of the customer (Can be further linked with some weak attributes like Locality, City, State etc.)
    Contact Number of the customer (PRIMARY KEY)

2. Order Table - 
  This will hold all the future orders associated with a particular Customer (using CustomerID)
  For every row, 
    There will be a unique Order ID (PRIMARY KEY)
    A mapped customer ID, to represent the owner of the order.
    Future time of delivering the order
    Order related special instructions ( like cutlery required, or delivery instructions)
    Subscribed will tell whether the particular order is subscribed for future.'
    ReccuringFrequency will tell the frequency at which the order has to be repeated if subscribed. (In Days)
    
 3. ORDER HISTORY TABLE - 
  This will hold all the past orders made by a customer
  For every row,
    There will be a mapped Order ID from Order Table (PRIMARY KEY)
    A mapped customer ID, to represent the owner of the order.
    TimeOfDelivery will tell the date and time at which order was delivered. (PRIMARY KEY)
    
4. ITEM QTY MAPPING TABLE  -
  This will hold mapping of quantity of every item in an order with recipie instructions.
  For every row,
    There will be ItemQtyMappingID mapped with OrderID from Orders Table (PRIMARY KEY)
    Item will be Food Item under a particular order (PRIMARY KEY)
    Quantity of the item
    Recipe Instructions related to preparation of that item. 
    
5. RECCURING ORDERS TABLE
  This will store all the orders which are subscribed by different customers.
  For every row,
    There will be customer ID mapped from Customer Table
    OrderID of the order which is subscribed, mapped from Orders Table (PRIMARY KEY)
    Date of delivering the order.
    
 ## CRUD Operations
 
 1. Create New Non reccuring Order with existing customer
    
    Check if customer already exists using contact number from Customer Table
    If Order ID provided - SELECT order from ORDERS from Orders Table and UPDATE Time of Delivery
    If Order ID not provided - INSERT new order row in ORDERS Table
    Set Subscribed Status as NO
    Set Reccuring Frequency as 0
    
 3. Create a Future Subscription (Recurring Order)
    
    Check if customer already exists using contact number from Customer Table
    If Order ID provided - SELECT order from ORDERS from Orders Table and UPDATE Time of Delivery
    If Order ID not provided - INSERT new order row in ORDERS Table
    Set Subscribed Status as YES
    Set Reccuring Frequency as number of days in query
    INSERT a row in RECCURING ORDERS table with date of delivery.
    
 5. Process a Non-reccuring Order

    SELECT all orders from ORDERS table WHERE future date of delivery is current date.
    After Delivery is marked, INSERT row in ORDER HISTORY Table
    
 7. Process a reccuring  Order

    SELECT all orders from RECCURING ORDERS table WHERE future date of delivery is current date.
    For every order on current day, UPDATE future date of delivery incremented by reccuring frequency for that particular order ID from orders table.
    After Delivery is marked, INSERT row in ORDER HISTORY Table
    
 9. Cancel Order

    To cancel the order, Date of Delivery will be updated to 30-11-1989 (Or any past date) so that we still have order details in the Orders table, in case customer 
    want to order/subscribe for it later
    Store the Order details in Order History Table.
    
 11. Subscribe for a Order from Order History

      SELECT Order ID for the particular customer ID which is to be subscribed.
      UPDATE Subscribed Status to YES , Reccuring Frequency, Future Date of Delivery for the particular Order in Orders Table.
      INSERT row in Reccuring Orders Table containing above order ID and future date of delivery.
      
 13. Delete Subscription
     SELECT Order ID for the particular customer ID which is to be unsubscribed.
     UPDATE Subscribed Status to NO , Reccuring Frequency to 0, Future Date of Delivery to some past date for the particular Order in Orders Table.
     DELETE row from Reccuring Orders Table containing above order ID and future date of delivery.
     
      
 15. Update a One Time Order
      
      SELECT Order ID for the particular customer ID which is to be updated.
      UPDATE order details in Orders table using above Order ID.
      
 17. Update a reccuring Subscribed Order
      SELECT Order ID for the particular customer ID which is to be updated.
      UPDATE order details in Orders table using above Order ID.
      UPDATE order details in Reccuring Orders table using above Order ID. (Only if Date of Delivery is changed or reccuring frequency is changed)
  
  
  
    
 
  
