# warehouse_picking
A set of functions that can be used for picking items in a warehouse.

### Single Order Picking

One order is assigned to one picker robot

We instanciate the problem with a warehouse with 100 SKUs. The system monitors the inventory level of the SKUs and assigns a picker robot to the order. Once the order is successfully picked, inventory level is updated. 

For routing, the [vehicle routing problem](https://github.com/projektdexter/VehicleRoutingProblem) code is used.

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

