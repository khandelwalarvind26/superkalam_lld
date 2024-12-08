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
    
- **SQL Query**:
    ```sql
    
    ```