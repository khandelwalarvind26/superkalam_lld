## Multiple users booking same seats
- Use locks on the sql db
- While the booking associated with one user is changing seat in the db, other user can neither read nor change
- Cannot proceed with booking unless all required seats are locked
- This way whichever user manages to acquire the locks will proceed with the booking
- Or use atomicity with isolation levels
- while one transaction is going on, others will keep waiting, until that transaction commits

---

## High load on DB due to single show
- Distribute database tables based on show_id (sharding)
- this way if one show (eg. coldplay concert) is taking too much bandwidth, other shows can continue booking
- Also introduce multiple backend services and distribute using load balancer

---

## High load on DB trying to fetch movie data
- It is possible that a lot of users are trying to fetch the same show data, which can lead to increased waiting times
- Static data like thumbnails, title, etc. can be easily cached using an in memory db like redis
- All movie related data which doesn't need to be in absolutely in sync like reviews, ratings, etc. can also be cached
- Reduces load on DB and much faster response times
- LRU cache makes most sense

---

## Single user blocking multiple seats
- A single user can go to the payment stage and block seats by doing bookings and not making the payment
- Introduce rate limiting from individual ip addresses
- If certain threshold number of seats already in pending status from current ip, don't allow any more bookings to go ahead

--- 

## Large number of users trying to book the same show, eg. Coldplay concert 1M+ people
- Using a similar queuing service that bookmyshow used
- Add all incoming users to a queue, assign a queue_id to each user
- Take a batch of users from the beginning of the queue and allow them to perform booking only for a time duration
