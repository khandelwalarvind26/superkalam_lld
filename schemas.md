```sql
CREATE TABLE IF NOT EXISTS movies (

    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    rating INT

);
```

```sql
CREATE TABLE IF NOT EXISTS users (

    username VARCHAR(100) PRIMARY KEY,
    city VARCHAR(100) NOT NULL
    
);
```

```sql
CREATE TABLE IF NOT EXISTS theaters(

    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    latitude DECIMAL(9,6) NOT NULL,
    longitude DECIMAL(9,6) NOT NULL

);
```

```sql
CREATE TABLE IF NOT EXISTS screens(

    id SERIAL PRIMARY KEY,
    capacity INT NOT NULL,

    theater_id INT NOT NULL,
    FOREIGN KEY (theater_id) REFERENCES theaters(id) ON DELETE CASCADE

);
```

```sql
CREATE TABLE IF NOT EXISTS shows (

    id SERIAL PRIMARY KEY,
    start_time TIMESTAMP NOT NULL,
    end_time TIMESTAMP NOT NULL,
    language VARCHAR(100) NOT NULL,

    movie_id INT NOT NULL,
    theater_id INT NOT NULL,
    screen_id INT NOT NULL,
    FOREIGN KEY (movie_id) REFERENCES movies(id) ON DELETE CASCADE,
    FOREIGN KEY (theater_id) REFERENCES theaters(id) ON DELETE CASCADE,
    FOREIGN KEY (screen_id) REFERENCES screens(id) ON DELETE CASCADE
);
```

```sql
CREATE TYPE seat_status AS ENUM ('empty', 'booked');

CREATE TABLE IF NOT EXISTS seats(

    id SERIAL PRIMARY KEY,
    status seat_status NOT NULL,
    seat_number VARCHAR(3) NOT NULL, /* A3, C16, etc.*/
    price INT NOT NULL,

    screen_id INT NOT NULL,
    show_id INT NOT NULL,
    booking_id VARCHAR(64),
    FOREIGN KEY (screen_id) REFERENCES screens(id) ON DELETE CASCADE,
    FOREIGN KEY (show_id) REFERENCES shows(id) ON DELETE CASCADE,
    FOREIGN KEY (booking_id) REFERENCES bookings(id)

);
```

```sql
CREATE TYPE booking_status AS ENUM ('waiting', 'confirmed', 'failed');

CREATE TABLE IF NOT EXISTS bookings(

    id VARCHAR(64) PRIMARY KEY,
    created_at TIMESTAMP NOT NULL,
    status booking_status NOT NULL,

    username VARCHAR(100) NOT NULL,
    transaction_id VARCHAR(64) NOT NULL,
    FOREIGN KEY (username) REFERENCES users(username) ON DELETE CASCADE,
    FOREIGN KEY (trasaction_id) REFERENCES transactions(id)
);
```

```sql
CREATE TYPE txn_status AS ENUM ('pending', 'success', 'failed');

CREATE TABLE IF NOT EXISTS transactions(


    id VARCHAR(64) PRIMARY KEY, /* 64-bit hashed string as primary key */
    amount INT NOT NULL,
    status txn_status NOT NULL,

    booking_id VARCHAR(64) NOT NULL,
    FOREIGN KEY (booking_id) REFERENCES bookings(id)

);
``` 