# Ultimate-Password-Manager

> Authors : Pierre Thiébaud & Victor Giordani

## Context

This project is realized as part of the third Practical work in the DAI class. This simple project aims to re-created an overly
simplified password manager API. The core of the project is the API creation using the `Javalin` library. 

## Password Manager API

The Password Manager API allows users to manage stored passwords in a simple local vault.  
The API uses the HTTP protocol and the JSON format.

The API is based on the CRUD pattern. It has the following operations:
- Creating password entries.
- Getting all password entries for the logged‑in user.
- Getting one password entry by its ID.
- Updating a password entry.
- Deleting a password entry.
- Generating strong random passwords.
- Creating and authenticating users

Each user only has access to **their own** password entries.

---

## Create a new user
- `POST /users`

Create a new user.

### Request body
- `username` – The username of the user.
- `password` – The user’s password.

### Response
The response body contains a JSON object with the following properties:
- `id`
- `username`
- `createdAt`

### Status codes
- `201` [Created] - The user has been successfully created.
- `400` [Bad Request] - The request body is invalid.
- `409` [Conflict] - The user already exists.

---

## Update a user
- `PUT /users/{id}`

Replace the entire user.  
Users can only update **their own** account.

### Request body
- `username`
- `password`

### Response
Full updated user object.

### Status codes
- `200` [OK] - User information have been successfully changed
- `400` [Bad Request] - Request body is invalid
- `404` [Not Found] - User does not exist
- `403` [Forbidden] - User can't modify someone else's profile

---

## Partially update a user
- `PATCH /users/{id}`

Update only specific fields.

### Example request body
```json
{
  "username": "newName"
}
```

### Status codes
- `200` [OK] - Changes were made successfully
- `400` [Bad Request] - Request body is invalid
- `404` [Not Found] - User was not found
- `403` [Forbidden] - Not allowed to make changes on someone else's profile.

---

## Delete a user
- `DELETE /users/{id}`

Users can only delete **their own** account.

### Status codes

- `204` [No Content] - Account was deleted successfully, however, nothing is returned  
- `404` [Not Found] - User is not found
- `403` [Forbidden] - Can't delete someone else's profile


## Authentication

## Login
- `POST /login`

### Request body
- `username`
- `password`

### Response
Sets a cookie user with the user ID.

### Status codes
- `204` [No Content] - Login succeeded, however, nothing is returned
- `400` [Bad Request] - Request body is invalid
- `401` [Unauthorized] - The user does not exist or the password is incorrect

## Logout
- `POST /logout`

Clears the user cookie.

### Status codes
- `204` [No Content] - Logout successful

## Passwords (Vault)

## Create a new password entry
- `POST /passwords`

### Request body

- `service` – ex: github, discord  
- `login` – The login/username for the service  
- `password` – Optional if generate is set to true
- `generate=true` – Query to auto‑generate a strong password

### Response
- `id`
- `ownerId`
- `service`
- `login`
- `password`
- `createdAt`

### Status codes
- `201` [Created] - Password entry created successfully
- `400` [Bad Request] - Request body is invalid


## Get all password entries
- `GET /passwords`

Returns only the entries belonging to the logged‑in user.

### Response
Array of:
- `id`
- `service`
- `login`
- `password`
- `createdAt`

### Status codes
- `200` [OK] - Request succeeded

## Get one password entry
- `GET /passwords/{id}`

### Status codes
- `200` [OK] - Request succeeded
- `404` [Not Found] - Password entry not found
- `403` [Forbidden] - trying to access another user's entry

## Update a password entry
- `PUT /passwords/{id}`

Users can update only **their own** entries.

### Request body
- `service`
- `login`
- `password`

### Status codes
- `200` [OK] - Entry successfully updated  
- `400` [Bad Request] - Request body is invalid
- `404` [Not Found] - Entry does not exist
- `403` [Forbidden] - trying to update another user's entry


## Delete a password entry
- `DELETE /passwords/{id}`

### Status codes
- `204` [No Content]  
- `404` [Not Found]  
- `403` [Forbidden]