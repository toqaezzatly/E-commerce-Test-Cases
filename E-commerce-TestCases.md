



## Test Cases

### Test Scenario ID: Order-1

- **Test Case ID**: Order-1A  
- **Test Case Description**: Testing calculation of the total order amount with valid order items.  
- **Test Priority**: High  
- **Pre-Requisite**: Valid `order` object with items added.  
- **Post-Requisite**: NA  

| S.No | Action                         | Inputs                                                                 | Expected Output | Actual Output | Test Browser | Test Result | Test Comments |
|------|--------------------------------|------------------------------------------------------------------------|-----------------|---------------|--------------|-------------|----------------|
| 1    | `order.calculateTotal()`       | `product1` with price 10 and quantity 2, `product2` with price 20 and quantity 1 | 40              |               | NA           |             |                |
| 2    | `order.calculateTotal()`       | `product1` with price 99999 and quantity 1, `product2` with price 1 and quantity 1 | 100000          |               | NA           |             | Test with large total amount. |

---

### Test Scenario ID: Order-2

#### Sub-scenario: Add Order Item - Valid Item
- **Test Case ID**: Order-2A  
- **Test Case Description**: Testing addition of a valid order item.  
- **Test Priority**: High  
- **Pre-Requisite**: Valid `order` object initialized.  
- **Post-Requisite**: NA  

| S.No | Action                         | Inputs                                                      | Expected Output                                                                                  | Actual Output | Test Browser | Test Result | Test Comments |
|------|--------------------------------|-------------------------------------------------------------|--------------------------------------------------------------------------------------------------|---------------|--------------|-------------|----------------|
| 1    | `order.addOrderItem()`         | `product3` with id 'p3', quantity 1                         | `order.items` array to contain `{productId: 'p1', quantity: 2}, {productId: 'p2', quantity: 1}, {productId: 'p3', quantity: 1}` |               | NA           |             |                |
| 2    | `order.addOrderItem()`         | `product4` with id 'p4', quantity 9999                       | `order.items` array to contain the new item and properly calculate the new total.                |               | NA           |             | Test with large quantity. |

---

#### Sub-scenario: Add Order Item - Invalid Item Quantity
- **Test Case ID**: Order-2B  
- **Test Case Description**: Testing addition of an order item with invalid quantity (0).  
- **Test Priority**: High  
- **Pre-Requisite**: Valid `order` object initialized.  
- **Post-Requisite**: NA  

| S.No | Action                         | Inputs                                                      | Expected Output                | Actual Output | Test Browser | Test Result | Test Comments |
|------|--------------------------------|-------------------------------------------------------------|--------------------------------|---------------|--------------|-------------|----------------|
| 1    | `order.addOrderItem()`         | `product3` with id 'p3', quantity 0                         | Error: Quantity must be > 0    |               | NA           |             |                |
| 2    | `order.addOrderItem()`         | `product3` with id 'p3', quantity -1                        | Error: Quantity must be > 0    |               | NA           |             | Test with negative quantity. |

---

### Test Scenario ID: Order-3

- **Test Case ID**: Order-3A  
- **Test Case Description**: Testing invalid order status transitions.  
- **Test Priority**: High  
- **Pre-Requisite**: Valid `order` object initialized.  
- **Post-Requisite**: NA  

| S.No | Action                         | Inputs          | Expected Output                           | Actual Output | Test Browser | Test Result | Test Comments |
|------|--------------------------------|-----------------|-------------------------------------------|---------------|--------------|-------------|----------------|
| 1    | `order.transitionStatus()`     | `invalidStatus` | Error: Invalid order status: invalidStatus |               | NA           |             |                |
| 2    | `order.transitionStatus()`     | `shipped` (from `processing`) | Error: Invalid transition from 'processing' to 'shipped' without payment |               | NA           |             | Test invalid status transition. |

---

### Test Scenario ID: Order-4

#### Sub-scenario: Concurrency - Simultaneous Order Modification
- **Test Case ID**: Order-4A  
- **Test Case Description**: Testing concurrent modification of the same order from different sessions.  
- **Test Priority**: High  
- **Pre-Requisite**: Two sessions active for the same `order`.  
- **Post-Requisite**: NA  

| S.No | Action                         | Inputs            | Expected Output                                  | Actual Output | Test Browser | Test Result | Test Comments |
|------|--------------------------------|-------------------|--------------------------------------------------|---------------|--------------|-------------|----------------|
| 1    | Session 1: `order.addOrderItem()` | `product5`, quantity 2  | `order.items` updated with product5              |               | NA           |             |                |
| 2    | Session 2: `order.addOrderItem()` | `product6`, quantity 3  | `order.items` updated with product6              |               | NA           |             |                |
| 3    | Session 1: `order.calculateTotal()` | NA                | Correct total with both items added              |               | NA           |             | Simulate simultaneous modifications. |

---

### Test Scenario ID: Order-5

#### Sub-scenario: Performance Testing - Large Order
- **Test Case ID**: Order-5A  
- **Test Case Description**: Testing system performance with a large number of order items.  
- **Test Priority**: Medium  
- **Pre-Requisite**: System should be able to handle large order sets.  
- **Post-Requisite**: NA  

| S.No | Action                         | Inputs           | Expected Output                           | Actual Output | Test Browser | Test Result | Test Comments |
|------|--------------------------------|------------------|-------------------------------------------|---------------|--------------|-------------|----------------|
| 1    | `order.addOrderItem()`         | 1000 items, each with quantity 1 | Successful addition of 1000 items and correct total calculation |               | NA           |             | Test for performance under load. |
