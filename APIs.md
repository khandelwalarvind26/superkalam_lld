<!-- APIs in order in which they will be called -->
## POST /users/
- **Description**: Creates a new user
- **Request**:
    - Body(JSON):
        ```json
        {
            username: string,
            location: {
                latitude: decimal,
                longitude: decimal
            }
        }
        ```
- **Response**:
    - `200 OK`
    - `409 CONFLICT`: `User already exists`

---

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

---



## GET /movies/{search_query}
- **Description**: Return a list of movies similar to search query
- **Request**:
    - Path Parameters:
        - `search_query`: Search query to which similar movies are to be retruned
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

---


## GET /movie/{movie_id}
- **Description**: Return all details about a single movie id 
- **Request**:
    - Path Parameters:
        - `movie_id`: id of the movie to be queried
- **Response**:
    - `200 success`:
        ```json
        {   
                id: number,
                name: string,
                rating: number,
                about: string,
                duration: integer,
                release_date: datetime,
                cast: [
                    {
                        name: string,
                        picture: string, //url
                        role: string,
                    }
                ],
                reviews: [
                    {
                        username: string,
                        content: string,
                        rating: integer
                    }
                ]
        }
        ```
    - `404 Error not found`: `Unable to find movie requested`

---

## GET /buytickets/{movie_id}
- **Description**: Fetch nearest showing shows of a particular movie
- **Request**:
    - Path Parameters:
        - `movie_id`: id of the movie whose shoes we want
    - Body(JSON):
        ```json
        {
            username: string, // Used to get the location of the user and hence, find nearby shows   
            language: string,
            date: datetime
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

---


## GET /buytickets/show/{show_id}
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
                    type: string,
                    price: number
                }
            ]
        }
        ```
    - `404 error not found`: `show with required show_id not found`

---

## POST /buytickets/
- **Description**: 
    - Mark seats as booked (Basically lock the seats)
        - If cannot acquire all seats, return failure
    - Create a pending booking

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
            booking_id: string,
            total_amount: number
        }
        ```
    - `409 conflict`: Seats not available

---

## POST /payment/
- **Description**: 
    - New pending transaction for the booking
    - Add the newly created transaction reference to the booking object
    - Initiate transaction 

- **Request**:
    - Body(JSON):
        ```json
        {
            booking_id: string,
            amount: number // total amount to be payed
        }
        ```

- **Response**:
    - `200 success`: `transaction intitated`
    - `500 Internal server error`: `Failed to initiate transaction`

---

## POST payments/webhook/{transaction_id}
- **Description**: 
    - To be triggered by payment gateway
    - Mark status of transaction
    - Push booking to kafka queue

- **Request**:
    - Path parameters:
        `transaction_id`: Transaction whose status is being conveyed by payment gateway
    - Body(JSON):
        ```json
        {
            status: string // success/failure
        }
        ```
- **Response**:
    - `200 success`


---

## POST /booking_service/
- **Description**: 
    - To be used by service which processes a booking from the kafka queue
    - If transaction failed -> mark booking as failed and mark seats as empty
    - If succeded -> mark booking as completed
    - Also need to implement authentication

- **Request**:
    - Body(JSON):
        ```json
        {
            booking_id: string,
            status: string // success/failure
        }
        ```

- **Response**:
    - `200 success`

---

## GET /booking/{booking_id}
- **Description**: 
    - Return all info related to a booking/ticket
    - implement authentication, only required user can access

- **Request**:
    - Path parameters:
        - `booking_id`: id of booking to be queried

- **Response**:
    - `200 success`:
        ```json
        {
            created_at: datetime,
            booking_status: string,
            amount: integer,
            theater_name: string,
            screen_number: char,
            movie_name: string,
            thumbnail: string, //url,
            start_time: datetime,
            end_time: datetime,
            seats: [
                {
                    seat_number: string,
                    seat_type: string
                }
            ]
        }
        ```

    - `404 Not found`: `Booking with booking_id does not exist`