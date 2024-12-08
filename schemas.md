```sql
-- Simple users table for a user which comes for booking a show to the platform
CREATE TABLE IF NOT EXISTS users (

    username VARCHAR(100) PRIMARY KEY,
    city VARCHAR(100) NOT NULL
    
);
```

```sql
/*All movie details*/
CREATE TABLE IF NOT EXISTS movies (

    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    rating INT,
    thumbnail VARCHAR(100),
    duration INT NOT NULL,
    release_date DATE NOT NULL,
    about VARCHAR(500)

);
```

```sql
-- All actors info stored in this table
CREATE TABLE IF NOT EXISTS actors (

    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL
    picture VARCHAR(100), -- Url to the picture
    age INT

);
```

```sql
/* Many to many relation between movies and actors */
CREATE TABLE IF NOT EXISTS cast (

    actor_id INT NOT NULL,
    movie_id INT NOT NULL,
    PRIMARY KEY (actor_id, movie_id),
    role VARCHAR(100) NOT NULL, -- eg. Amir khan plays rancho so Rancho will be the role

    FOREIGN KEY (actor_id) REFERENCES actors(id),
    FOREIGN KEY (movie_id) REFERENCES movies(id) ON DELETE CASCADE

);
```


```sql
/*Used to fetch all reviews related to a movie to display when the movie is loaded*/
CREATE TABLE IF NOT EXISTS reviews(
    
    id SERIAL PRIMARY KEY,
    rating INT NOT NULL,
    content TEXT,

    username VARCHAR(100),
    FOREIGN KEY (username) REFERENCES users(username),
    movie_id INT NOT NULL,
    FOREIGN KEY (movie_id) REFERENCES movies(id) ON DELETE CASCADE

)
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
    screen_number CHAR NOT NULL, -- A, B, Z, etc.

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
CREATE TYPE seat_types AS ENUM ('platinum','gold','silver');

CREATE TABLE IF NOT EXISTS seats(

    id SERIAL PRIMARY KEY,
    status seat_status NOT NULL,
    seat_number VARCHAR(3) NOT NULL, /* A3, C16, etc.*/
    seat_type seat_types NOT NULL,
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
/*Booking here basically refers to a ticket*/
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