# GET /movies/
- **Description**: Return a list of all movies in DB
- **Request**:
    - Body(JSON):
- **Response**:
    - `200 success`:
        ```json
        [
            {   
                id: number,
                name: string,
                rating: number
            }
        ]
        ```
- **SQL Query**:
    ```sql
    SELECT * FROM movies; 
    ```


---



# GET /movies/{query}
- **Description**: Return a list of movies similar to search query
- **Request**:
    - Path Parameters:
        - `query`: Search query to which similar movies are to be retruned
- **Response**:
    - `200 success`:
        ```json
        [
            {   
                id: number,
                name: string,
                rating: number
            }
        ]
        ```
- **SQL Query**:
    ```sql
    SELECT * FROM movies WHERE movies.name LIKE '%{query}%'; 
    ```

---



# GET /movies/{movie_id}/shows/{date}
- **Description**: Fetch nearest showing shows of a particular movie
- **Request**:
    - Path Parameters:
        - `movie_id`: id of the movie whose shoes we want
        - `date`: On which shows are being searched
    - Body(JSON):
        ```json
        {
            latitude: string,
            longitude: string,
            language: string
        }
        ```
- **Response**:
    - `200 success`:
        ```json
        [
            {
                id: number,
                start_time: timestamp,
                theater_name: string,
                min_price: number,
                max_price: number,
                availability: float //percentage
            }
        ] 
        ```
- **SQL Query**:
    ```sql
    
    ```
