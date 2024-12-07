```sql
CREATE TYPE seat_status AS ENUM ('empty', 'booked');

CREATE TABLE IF NOT EXISTS seats(

    id SERIAL PRIMARY KEY,
    status seat_status NOT NULL,
    seat_number VARCHAR(3) NOT NULL, /* A3, C16, etc.*/
    price INT NOT NULL,

    screen_id INT NOT NULL,
    show_id INT NOT NULL,
    booking_id INT,
    FOREIGN KEY (screen_id) REFERENCES screens(id) ON DELETE CASCADE,
    FOREIGN KEY (show_id) REFERENCES shows(id) ON DELETE CASCADE,
    FOREIGN KEY (booking_id) REFERENCES bookings(id)

);
```