```sql
CREATE TYPE txn_status AS ENUM ('pending', 'completed');

CREATE TABLE IF NOT EXISTS transactions(


    id VARCHAR(64) PRIMARY KEY, /* 64-bit hashed string as primary key */
    amount INT NOT NULL,
    status txn_status NOT NULL,

    booking_id VARCHAR(64) NOT NULL,
    FOREIGN KEY (booking_id) REFERENCES bookings(id)

);
``` 