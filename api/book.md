# POST /book/
- **Description**: 
    - Create a pending booking
    - Mark seats as booked
    - New pending transaction
- **Request**:
    - Body(JSON):
        ```json
        {
            username: string, // username which makes the booking
            seat_ids: [number] //ids of seats to be booked
        }
        ```

- **Response**:
    - `200 success`:
        ```json
        {
            booking_id: string
        }
        ```
    - `409 conflict`: Seat already booked
    
- **SQL Query**:
    ```sql
    
    ```