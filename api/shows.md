## GET /movies/{movie_id}/shows/{date}
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

---


## GET /shows/{show_id}
- **Description**: Fetch details of any particular show
- **Request**:
    - Path Parameters:
        - `show_id`: id of the show whose details we want

- **Response**:
    - `200 success`:
        ```json
        {
            id: number,
            start_time: timestamp,
            end_time: timestamp,
            theater_name: string,
            seats: [
                {
                    id: number,
                    seat_number: string,
                    status: string,
                    price: number
                }
            ]
        }
        ```
- **SQL Query**:
    ```sql
    
    ```