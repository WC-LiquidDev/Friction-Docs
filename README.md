# API Documentation for `f9826` TitleID

This document provides an overview of the endpoints in the `f9826` TitleID for managing user authentication, user data, inventory, and in-app purchases. The backend uses a JSON file (`users.json`) to store user data and has endpoints for login, data retrieval, user data updates, and item purchases. Below is a description of each endpoint and how to use it.

---

## Base URL

All endpoints are prefixed with `/f9826`.

---

## Common Headers

### `T-SecretKey`
- **Required**: Only for `/view`.
- **Purpose**: Grants authorized access to view user data.
- **Example**: `"T-SecretKey": "KeyMaybe"`

### `T-Authorization`
- **Required**: For all user-related endpoints.
- **Purpose**: Acts as a session ticket to authorize and authenticate user access.
- **Example**: `"T-Authorization": "<UserSessionTicket>"`

---

## Endpoints

### 1. `GET /` - Access Page
- **Description**: Serves the `access.html` template for front-end access.
- **Methods**: `GET`, `POST`
- **Response**: HTML page

---

### 2. `POST /login` - User Login
- **Description**: Authenticates a user based on their `AuthenticationId` and optional `Tags`.
- **Request Body**:
    - `AuthenticationId` (string) - **Required**
    - `Tags` (array) - Optional
- **Response**:
    - `200 OK`: JSON containing user login information.
    - `400 Bad Request`: If `AuthenticationId` is missing or invalid.

---

### 3. `GET /view` - View All User Data
- **Description**: Returns all user data in `users.json`.
- **Headers**:
    - `T-SecretKey` - **Required**
- **Response**:
    - `200 OK`: JSON containing all user data.
    - `403 Forbidden`: If `T-SecretKey` does not match.
    - `404 Not Found`: If `users.json` file is missing.
    - `500 Internal Server Error`: If JSON decoding fails.

---

### 4. `POST /User/GetAccountInformation` - Get User Account Information
- **Description**: Retrieves account information for a user based on `T-Authorization`.
- **Headers**:
    - `T-Authorization` - **Required**
- **Response**:
    - `200 OK`: JSON containing user account information.
    - `401 Unauthorized`: If `T-Authorization` is missing or invalid.

---

### 5. `POST /Currency/PurchaseItem` - Purchase an Item
- **Description**: Processes a purchase request for an in-game item.
- **Headers**:
    - `T-Authorization` - **Required**
- **Request Body**: JSON containing purchase data (details specific to the purchase should be added here).
- **Response**:
    - `200 OK`: JSON containing updated user data with purchase confirmation.
    - `401 Unauthorized`: If `T-Authorization` is missing or invalid.

---

### 6. `POST /User/UpdateUserData` - Update User Data
- **Description**: Allows updates to user-specific data, such as adding extra tags or other user-specific settings.
- **Headers**:
    - `T-Authorization` - **Required**
- **Request Body**: JSON containing user data updates.
- **Response**:
    - `200 OK`: JSON containing updated user data.
    - `401 Unauthorized`: If `T-Authorization` is missing or invalid.

---

### 7. `POST /User/Inventory` - Get User Inventory
- **Description**: Retrieves the user's inventory.
- **Headers**:
    - `T-Authorization` - **Required**
- **Response**:
    - `200 OK`: JSON containing the user’s inventory.
    - `401 Unauthorized`: If `T-Authorization` is missing or invalid.

---

## Helper Functions

- **`extractUser(data_file, target_session_ticket)`**: Fetches user info based on `SessionTicket`.
- **`editUser(data_file, target_session_ticket, updates)`**: Edits and updates user data in `users.json`.
