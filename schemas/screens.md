```sql
CREATE TABLE IF NOT EXISTS screens(

    id SERIAL PRIMARY KEY,
    capacity INT NOT NULL,

    theater_id INT NOT NULL,
    FOREIGN KEY (theater_id) REFERENCES theaters(id) ON DELETE CASCADE

);
```