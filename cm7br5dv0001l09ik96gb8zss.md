---
title: "System design: design a parking garage, WIP"
datePublished: Wed Feb 19 2025 10:10:15 GMT+0000 (Coordinated Universal Time)
cuid: cm7br5dv0001l09ik96gb8zss
slug: system-design-design-a-parking-garage-wip
tags: interview-prep

---

**Design a reservation and payment system for a parking garage**

[https://www.youtube.com/watch?v=NtMvNh0WFVM&list=PLrtCHHeadkHp92TyPt1Fj452\_VGLipJnL](https://www.youtube.com/watch?v=NtMvNh0WFVM&list=PLrtCHHeadkHp92TyPt1Fj452_VGLipJnL)

I’m up to 4m01s

### Step 1: define the scope

Clarifying questions I came up with

* **&lt;what do I call this category?&gt;** Who are the users for this system? Is it web only, or mobile app?
    
    * Does the reservation system need to have the same front end as the payment system? or can they be separate, e.g. reservation is a mobile app, but payment is backend only
        
* **Scale:** How many users does this system need to handle today?
    
    * Do we anticipate any growth in the next year or two?
        
* **Security**: Are there any specific security / authentication requirements we need to take into consideration?
    
    * What user data are we collecting - I assume the bare minimum.
        
* **Integrations:** Does this system need to integrate with any other systems?
    
    * Payments in particular - will we be integrating with a 3rd vendor who will handle all the PII for us?
        

Then prompted by YouTube:

* **Product specs:**
    
    * Reservation: user needs to be able to view available spots, pick one, pay for it and get a receipt
        
        * How far in advance can we book? Do we need to support same day reservations?
            
            * If so, then how do we know which spots are available? Automatic sensors?
                
    * Types of parking spot: e.g. handicapped, size of vehicle, long stay vs short stay
        
    * Availability of spots: how does the system know? Can I assume there are sensors on the ground?
        
    * Prices for the garage: flat rate, different for each size of vehicle
        
    * Payment methods:
        
        * cash only via physical terminal? Credit card supported? Barcode scanning from shopping centre?
            
        * Can I pay on exit, or is it upfront payment only?
            
    * Login required?
        
    * Do we need to support rescheduling after payment?
        
        * e.g. want to change the date, or change the vehicle type
            
        * Do we need to support refunds? e.g. if they go from large to small
            
* Backend “consistency”: need to make sure two users can’t reserve the same spot
    
    * I would have called this reliability
        

### Step 2: design!

User Interface (web app / mobile app)

→ Load balancer (probably not required for a small garage

→ Web server

* Login (if needed)
    
* GetSpots
    
    * From cache
        
* SelectSpot / ReserveSpot `POST`
    
    * Mark the spot as unavailble for X minutes
        
    * Select vehicle type
        
    * select duration
        
* PaymentService
    
    * Select method
        
    * Integration with 3rd party payment provider
        
* Confirmation
    
    * Update DB and cache
        
* Clean up (if user backs out of flow)
    
    * Mark the spot as availble again
        

Public endpoints

* `GET /availableSpots?date=2025-02-22`
    
    * Assumption: simple system - a user does not get to choose the spot, just reserves one if it exists
        
        * more complex system - a user can see all the spots, available and taken, and choose the exact spot they want
            
    * Returns `200` with a body, that can be parsed by the UI into a visual schematic for the user. Most likely some kind of grid
        
* This `GetAvailableSpotsService`
    
    * Retrieve information from a cache or DB about which spots are available for that given day
        
    * Is this a cache or a DB? Depends on the speed we need it.
        
    * Probably DB is fine
        
        * Relational or non-relational?
            
        * Relational if we want a more complex system, e.g. we want to know WHO has booked which spot on a given day
            
        * Non-relational if all we need is a simple view of the available spots
            
        * So perhaps we end up with a mix of the two.
            
        * Non relational to quickly populate the `GET`
            
        * Then as part of the `PaymentService`, we will
            
            * Write to the DB
                
            * Update the cache
                
    * We can talk about the DB / table structure if we want to. For now, let’s just move on
        

# QUESTIONS / further research

* Is a cache always a non-relational / no SQL DB?
    
* Types of endpoints - learn more about RESTful vs GraphQL - are there others?