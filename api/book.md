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


# POST /book/{booking_id}
- **Description**: 
    - To be used by service which processes a booking from the kafka queue
    - If transaction failed -> delete booking and mark seats as empty
    - If succeded -> mark booking as completed

- **Request**:
    - Path params:
        - `booking_id`: id of the booking which is to be processed
    - Body(JSON):
        ```json
        {
            status: string //success/failure
        }
        ```

- **Response**:
    - `200 success`