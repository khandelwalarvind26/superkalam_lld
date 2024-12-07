## Multiple users booking same seats
- Use locks on the sql db
- While the booking associated with one user is changing seat in the db, other user can neither read nor change
- Cannot proceed with booking unless all required seats are locked
- This way whichever user manages to acquire the locks will proceed with the booking
- Or use atomicity with isolation levels
- while one transaction is going on, others will keep waiting, until that transaction commits

---

## Too much load on DB due to single show
- Distribute database tables based on show_id (sharding)
- this way if one show (eg. coldplay concert) is taking too much bandwidth, other shows can continue booking
- Also introduce multiple backend services and distribute using load balancer

---

## 