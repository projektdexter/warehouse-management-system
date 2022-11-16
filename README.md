# Warehouse Management System
This is an end-to-end solution for operating a warehouse. This solution focusses on two aspects 
1. cost 
2. throughput 

Warehouse operations can roughly be divided into three heads 
1. receiving/storage/inbound 
2. order picking and packing 
3. dispatch/outbound

I would like to point out is that this WMS assumes that robots are used for put-away and order picking. Nonetheless if a warehouse uses human pickers, the solution will run just fine (except that drone management function which has to be removed). 

### Receiving/inbound 
During this stage a truck unloads the merchandise in the receiving docs. The physical inventory is matched with the PO. If physical inventory matches PO quantity, the system will accept the order and assign robots for put-away task. If the physical inventory does not match the PO quantity the system will store the gap in product catalog and balance the difference when the next PO is released.

### Order picking and packing
In the stage the outbound order list is consolidated for a time t and robots are assigned for the picking task. Once the order is picked, the system matches the pick quantity and order quantity. If the pick quantity matches order quantity the system accepts the picked order and prepares for dispatch. Else, the system raises an alert to user.
Inside order picking you will find three strategies:
1. **Single order picking**: An individual robot is assigned to each order. For e.g., if we have 10 orders to pick, the system will assign 10 robots for the task.
2. **Batch picking**: Multiple orders are club together and a set of robots are assigned for the full batch of orders.  
3. **Zone picking**: First the warehouse is divided into n zones. Multiple batches of outbound orders is created. Robots are assigned to each zone for picking the items falling in each zones. The order is then consolidated from all the n zones and dispatched.
4. **Wave picking**: All outbound orders can be divided into n waves. These waves are then executed one after the other or concurrently by assigning the robots for the picking task. 

For robot routing, the [vehicle routing problem](https://github.com/projektdexter/VehicleRoutingProblem) code is used.

### Outbound 
In this stage the system first calculates the number of vehicles needed, their routes and schedule. The system will then assign robots for loading the outbound vehicles. 
### Inventory management 
The system uses continuous review for inventory management. Every time an inbound or outbound takes place, the system will re-calculate the inventor level for each item. If the inventory level is below a certain threshold, this function will allow users to automate PO generation. 
### Robot management 
This function operates continuously and manages assignment and battery of robots in the warehouse. 
### Dock management
This function manages assignment for inbound & outbound docks

### Other warehouse operations
Other warehousing tasks like audit, inventory count, demand planning etc. are not covered in this solution. Nonetheless I would like to point out that these are fairly basic tasks which can be formulated with little effort.

## Example

The order arrival is considered poisson with an interarrival time of 10 mins. We simulate the warehouse operations for 12 hours.

### Input data

time_matrix: is a NxN cost matrix between the points that have to be visited by the nodes, example:

```
    0     1   2     3   4
0   0  21.0  15  21.0  10
1  21   0.0   5   0.5  11
2  15   5.0   0   5.0   5
3  21   0.5   5   0.0  10
4  10  11.0   5  10.0   0

```

On_hand_dump: A Nx1 matrix with current on hand inventory for each SKU

Number of robots: N
Robot capacity: C

