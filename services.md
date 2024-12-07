## Booking service
- Iterate over all entries in the bookings table peridically
- Check the created_at of the bookings
- If the booking is pending for a threshold limit
    - Release all seats in that booking -> mark as empty
    - Mark booking as failed

## Payments polling service
- Iterate over payments db at regular intervals
- If a payment is pending since a limit above threshold
    - Poll the payment gateway for payment status
    - Modify payment entry in payments db accordingly
