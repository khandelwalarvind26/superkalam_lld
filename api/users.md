## POST /users/
- **Description**: Creates a new user
- **Request**:
    - Body(JSON):
        ```json
        {
            "username": "khandelwalarvind26",
            "location": "Mumbai",
        }
        ```
- **Response**:
    - `200 OK`
    - `409 CONFLICT`:
    `User already exists`

- **SQL Query**:
    ```sql
    INSERT INTO users (username, location) VALUES(req.username, req.location);
    ```