Everything I have implemented is according to the VAAHAN portal of GoI. It gives us the response in JSON and from that the necessary field are taken according to our use case.

## ComCon Parking Manager: System Overview
This system handles the heavy lifting for parking at Comic-Con India. It is built to manage thousands of cars and bikes across a 5-day event, making sure VIPs, cosplayers, and exhibitors get to their spots without causing a traffic jam at the main gates.
## The Heart of the System: VAHAN API
We don't rely on manual data entry. The system is built around the VAHAN Portal API. The moment a car pulls into a lane:

* The camera scans the plate.
* The system hits the VAHAN database to see if it is an SUV, Sedan, or Hatchback.
* It checks the model and fuel type (to find EVs for charging spots).
* It checks insurance validity to manage on-site liability.

Using this live data, the usher’s app instantly tells them exactly which floor or zone to send the driver to.
## Handling the "Multi-Day" Problem
We split the logic into Tickets and Sessions to handle people coming and going:

* The Ticket: This is the guest's digital pass for the whole event. If an exhibitor is there for all 5 days, they have one Ticket.
* The Session: Every single time that car enters and leaves, it’s a new Session. This lets us track exactly how many spots are free at any given minute and helps calculate fees for general visitors.

## How We Save Time and Money

* Smart Caching: We don’t call the government API every time a car re-enters. We store the car details in a local cache (Redis) on the first day. If the car comes back on Day 2, the system recognises it in milliseconds for free.
* Zone Tracking: Instead of micro-managing every single spot for general cars, we track "Zone Capacity." This keeps the traffic moving. We only reserve specific, numbered spots for VIPs and Performers.

## Security and Payments
The system links every entry session to a payment record. Whether it is a pre-paid VIP pass or a "pay-per-hour" visitor, the exit gate won't open unless the session status is cleared. For exhibitors carrying heavy gear, the session also logs their "cargo status" to ensure they leave with the same equipment they brought in.
Should we add a troubleshooting section for what happens if a number plate is too muddy for the camera to read?

