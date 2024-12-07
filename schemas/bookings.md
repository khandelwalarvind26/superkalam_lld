```sql
CREATE TYPE booking_status AS ENUM ('waiting', 'confirmed');

CREATE TABLE IF NOT EXISTS bookings(

    id SERIAL PRIMARY KEY,

    username VARCHAR(100) NOT NULL,
    FOREIGN KEY (username) REFERENCES users(username) ON DELETE CASCADE
);
```