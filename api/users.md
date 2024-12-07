## POST /users/
- **Description**: Creates a new user
- **Request**:
    - Body(JSON):
        ```bash
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
    ```bash
    INSERT INTO users (username, location) VALUES(req.username, req.location);
    ```