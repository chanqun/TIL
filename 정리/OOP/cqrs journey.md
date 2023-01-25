# CQRS Journey

The CQRS Pattern and event sourcing are not mere simplistic solutions to the problems associated with large-scale,
distributed systems.

## Journey 1 - Our Domain: Conference Management System

DDD는 써봄, CQRS는 경험해본 사람은 없음

### Overview of the system
- Manage the sale of different seat types for the conference
- Create a conference and define characteristics of that conference

### Selling seats for a conference
### Creating a conference

### NonFunctional Requirements

Scalability,
Flexibility,

## Journey 2 - Decomposing the domain

### Definition used in this chapter

- Domain
- Bounded context
- Context Map

### Bounded contexts in the conference management system
#### Order and Registration
- Order
  - reservation, payment, registration items (order 1 : N orderItems)
- Reservation
  - seat, ordering process (reservation 1 : N seat)
- If the registrant does not pay for the tickets with 15 minutes, 
the system deletes the reservation and the seats become available for other registrants to reserve 

#### Conference Management
- The name, description, and slug
- The start and end dates of the conference
- The different types and quotas of seats available at the conference

#### Payment

#### Discount Policy, Occasionally Disconnected, Submissions and schedule

![img.png](../image/contextmap.png)

### The Context map for the consoto conference management system
1. Events that report when conference have been created, updated, or published. Events that report when seat types have been created or updated
2. Events that report when orders have been created or updated. Events that report when attendees have been assigned to seats.
3. Requests for a payment be made.
4. Acknowledgement of the success or failure of the payment.



