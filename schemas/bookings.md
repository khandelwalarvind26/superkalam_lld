```sql
CREATE TYPE booking_status AS ENUM ('waiting', 'confirmed');

CREATE TABLE IF NOT EXISTS bookings(

    id VARCHAR(64) PRIMARY KEY,

    username VARCHAR(100) NOT NULL,
    transaction_id VARCHAR(64) NOT NULL,
    FOREIGN KEY (username) REFERENCES users(username) ON DELETE CASCADE,
    FOREIGN KEY (trasaction_id) REFERENCES transactions(id)
);
```