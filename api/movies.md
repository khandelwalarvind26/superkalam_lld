## GET /movies/
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



## GET /movies/{query}
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

