# GET /shows/{show_id}
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