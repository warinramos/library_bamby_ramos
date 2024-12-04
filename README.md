# LIBRARY API DOCUMENTATION

## Description 📚✨  
This application is a web API developed using the Slim Framework and employs JWT (JSON Web Token) for secure authentication and authorization. It enables users to efficiently manage a library database with features including:  

- **User Management**: User account registration, login, and profile management.  
- **Author Management**: Create, modify, view, and remove authors.  
- **Book Management**: Add, edit, view, and delete books.  
- **Relationships**: Manage associations between books and their respective authors.  

## Tools and Technologies 🛠️💻  
- **PHP**: A versatile server-side scripting language ideal for creating dynamic and secure APIs.  
- **Slim Framework**: A minimalistic PHP framework optimized for building RESTful web services efficiently.  
- **JWT (JSON Web Token)**: A widely-used standard for implementing secure, stateless authentication and authorization.  
- **MySQL**: A robust relational database system for storing and managing data related to users, authors, books, and their associations.  
- **JSON**: A lightweight and flexible data format for facilitating smooth communication between the client and server in API operations.  


## Features 🌟📦  

### Authentication and Security  
- Utilizes JWT for creating and verifying secure access tokens.  
- Middleware protects API endpoints and ensures token integrity by checking for reuse.  

### Core Functionalities  
- **User Management**: Supports user registration, login, account updates, and user viewing/deletion.  
- **Author Management**: Provides secure operations to manage author details.  
- **Book Management**: Facilitates adding, editing, and deleting books.  
- **Books-Authors Relationships**: Manages many-to-many associations between books and authors.  

### Token Management  
- Tokens are valid for 1 hour before expiring.  
- Previously used tokens are tracked and invalidated using `$_SESSION['used_tokens']`.  

## Endpoints 🌐🔗  

1. ### Register a User  
    - **Endpoint:** `/user/register`  
    - **Method:** `POST`  
    - **Description:**  
        This API endpoint allows new users to register by providing a username and password. The system sanitizes the input, checks if the username already exists in the database, and securely stores the user's password with hashing if the username is unique.  
    - **Sample Request (JSON):**
        ```json
        {
            "username": "example_username",
            "password": "example_password"
        }
        ```  
    - **Response:**  
        - **On Success:**
            ```json
            {
                "status": "success",
                "data": "User registered successfully."
            }
            ```  
        - **On Failure (Username already exists):**
            ```json
            {
                "status": "fail",
                "data": "Username already taken."
            }
            ```  
        - **On Failure (Internal Server Error):**
            ```json
            {
                "status": "fail",
                "data": "Error message here"
            }
            ```
            
2. ### Authenticate a User  
    - **Endpoint:** `/user/authenticate`  
    - **Method:** `POST`  
    - **Description:**  
        This API endpoint allows users to authenticate by providing a username and password. If the provided credentials match an existing user in the database, a JWT (JSON Web Token) is generated and returned for further secure interactions. The system also manages previously used tokens to ensure they are expired and not reused.  
    - **Sample Request (JSON):**
        ```json
        {
            "username": "example_username",
            "password": "example_password"
        }
        ```  
    - **Response:**  
        - **On Success:**
            ```json
            {
                "status": "success",
                "token": "your_jwt_token_here"
            }
            ```  
        - **On Failure (Invalid username or password):**
            ```json
            {
                "status": "fail",
                "data": "Invalid username or password."
            }
            ```  
        - **On Failure (Internal Server Error):**
            ```json
            {
                "status": "fail",
                "data": "Error message here"
            }
            ```  

3. ### Update a User  
    - **Endpoint:** `/user/update`  
    - **Method:** `PUT`  
    - **Description:**  
        This API endpoint allows users to update their account details, including username and password. The user needs to provide a valid token for authentication. The token is checked for reuse and marked as used after the update. The system ensures that the old username exists and validates the new username and password before making the update in the database.  
    - **Sample Request (JSON):**
        ```json
        {
            "old_username": "old_username",
            "new_username": "new_username",
            "new_password": "new_password",
            "token": "your_jwt_token_here"
        }
        ```  
    - **Response:**  
        - **On Success:**
            ```json
            {
                "status": "success",
                "data": "User updated successfully"
            }
            ```  
        - **On Failure (Token already used):**
            ```json
            {
                "status": "fail",
                "data": "Token has already been used. Please provide a new token for the next request."
            }
            ```  
        - **On Failure (Invalid token):**
            ```json
            {
                "status": "fail",
                "data": "Invalid token."
            }
            ```  
        - **On Failure (User not found):**
            ```json
            {
                "status": "fail",
                "data": "User not found"
            }
            ```  
        - **On Failure (Internal Server Error):**
            ```json
            {
                "status": "fail",
                "data": "Error message here"
            }
            ```  

4. ### Delete a User  
    - **Endpoint:** `/user/delete`  
    - **Method:** `DELETE`  
    - **Description:**  
        This API endpoint allows an authenticated user to delete their account from the system. The user must provide a valid token for authentication. The token is checked to ensure it has not been used before and is valid. If the user exists, their account and associated tokens are removed from the database.  
    - **Sample Request (JSON):**
        ```json
        {
            "username": "username_to_delete",
            "token": "your_jwt_token_here"
        }
        ```  
    - **Response:**  
        - **On Success:**
            ```json
            {
                "status": "success",
                "data": "User deleted successfully"
            }
            ```  
        - **On Failure (Token already used):**
            ```json
            {
                "status": "fail",
                "data": "Token has already been used. Please provide a new token for the next request."
            }
            ```  
        - **On Failure (Invalid token):**
            ```json
            {
                "status": "fail",
                "data": "Invalid token."
            }
            ```  
        - **On Failure (User not found):**
            ```json
            {
                "status": "fail",
                "data": "User not found"
            }
            ```  
        - **On Failure (Internal Server Error):**
            ```json
            {
                "status": "fail",
                "data": "Error message here"
            }
            ```  

5. ### Display a User  
    - **Endpoint:** `/user/display`  
    - **Method:** `POST`  
    - **Description:**  
        This API endpoint allows a user to display their details by providing a valid token. The token is checked to ensure it has not been used before. If valid, the user information is fetched from the database and returned.  
    - **Sample Request (JSON):**
        ```json
        {
            "username": "username_to_display",
            "token": "your_jwt_token_here"
        }
        ```  
    - **Response:**  
        - **On Success:**
            ```json
            {
                "status": "success",
                "data": {
                    "userid": 1,
                    "username": "username_to_display",
                    "password": "hashed_password"
                }
            }
            ```  
        - **On Failure (Token already used):**
            ```json
            {
                "status": "fail",
                "data": "Token has already been used. Please provide a new token for the next request."
            }
            ```  
        - **On Failure (Invalid token):**
            ```json
            {
                "status": "fail",
                "data": "Invalid token."
            }
            ```  
        - **On Failure (User not found):**
            ```json
            {
                "status": "fail",
                "data": "User not found"
            }
            ```  
        - **On Failure (Internal Server Error):**
            ```json
            {
                "status": "fail",
                "data": "Error message here"
            }
            ```  

6. ### Add a Book  
    - **Endpoint:** `/book/add`  
    - **Method:** `POST`  
    - **Description:**  
        This API endpoint allows for the addition of a book into the database. It checks if the token is valid and ensures the token has not been used before. If the book already exists, it will not be added again.  
    - **Sample Request (JSON):**
        ```json
        {
            "title": "New Book Title",
            "token": "your_jwt_token_here"
        }
        ```  
    - **Response:**  
        - **On Success:**
            ```json
            {
                "status": "success",
                "data": "Book added successfully",
                "bookid": 1
            }
            ```  
        - **On Failure (Token already used):**
            ```json
            {
                "status": "fail",
                "data": "Token has already been used. Please provide a new token."
            }
            ```  
        - **On Failure (Invalid token):**
            ```json
            {
                "status": "fail",
                "data": "Invalid token."
            }
            ```  
        - **On Failure (Internal Server Error):**
            ```json
            {
                "status": "fail",
                "data": "Error message here"
            }
            ```  

7. ### Update a Book  
    - **Endpoint:** `/book/update`  
    - **Method:** `POST`  
    - **Description:**  
        This API endpoint allows for updating the title of an existing book. It first validates the token and ensures it hasn't been used before updating the book information.  
    - **Sample Request (JSON):**
        ```json
        {
            "token": "your_jwt_token_here",
            "bookid": 1,
            "title": "Updated Book Title"
        }
        ```  
    - **Response:**  
        - **On Success:**
            ```json
            {
                "status": "success",
                "data": "Book updated successfully"
            }
            ```  
        - **On Failure (Token already used):**
            ```json
            {
                "status": "fail",
                "data": "Token has already been used. Please provide a new token."
            }
            ```  
        - **On Failure (Invalid token):**
            ```json
            {
                "status": "fail",
                "data": "Invalid token."
            }
            ```  
        - **On Failure (Internal Server Error):**
            ```json
            {
                "status": "fail",
                "data": "Error message here"
            }
            ```  

8. ### Delete a Book  
    - **Endpoint:** `/book/delete`  
    - **Method:** `POST`  
    - **Description:**  
        This API endpoint allows for deleting a book by its `bookid`. It first validates the token and ensures it hasn't been used before deleting the book.  
    - **Sample Request (JSON):**
        ```json
        {
            "token": "your_jwt_token_here",
            "bookid": 1
        }
        ```  
    - **Response:**  
        - **On Success:**
            ```json
            {
                "status": "success",
                "data": "Book deleted successfully"
            }
            ```  
        - **On Failure (Token already used):**
            ```json
            {
                "status": "fail",
                "data": "Token has already been used. Please provide a new token."
            }
            ```  
        - **On Failure (Invalid token):**
            ```json
            {
                "status": "fail",
                "data": "Invalid token."
            }
            ```  
        - **On Failure (Internal Server Error):**
            ```json
            {
                "status": "fail",
                "data": "Error message here"
            }
            ```  

9. ### Display All Books  
    - **Endpoint:** `/book/display`  
    - **Method:** `POST`  
    - **Description:**  
        This API endpoint allows for displaying all the books in the library by retrieving the `bookid` and `title` of each book. It first validates the token and ensures it hasn't been used before fetching and displaying the books.  
    - **Sample Request (JSON):**
        ```json
        {
            "token": "your_jwt_token_here"
        }
        ```  
    - **Response:**  
        - **On Success:**
            ```json
            {
                "status": "success",
                "data": [
                    {
                        "bookid": 1,
                        "title": "Book Title 1"
                    },
                    {
                        "bookid": 2,
                        "title": "Book Title 2"
                    },
                    ...
                ]
            }
            ```  
        - **On Failure (Token already used):**
            ```json
            {
                "status": "fail",
                "data": "Token has already been used. Please provide a new token."
            }
            ```  
        - **On Failure (Invalid token):**
            ```json
            {
                "status": "fail",
                "data": "Invalid token."
            }
            ```  
        - **On Failure (Internal Server Error):**
            ```json
            {
                "status": "fail",
                "data": "Error message here"
            }
            ```  

10. ### Add Author  
    - **Endpoint:** `/author/add`  
    - **Method:** `POST`  
    - **Description:**  
        This API endpoint allows for adding a new author to the library. It first validates the provided token, checks if the author already exists in the database, and adds the new author if not already present.  
    - **Sample Request (JSON):**
        ```json
        {
            "name": "Author Name",
            "token": "your_jwt_token_here"
        }
        ```  
    - **Response:**  
        - **On Success:**
            ```json
            {
                "status": "success",
                "data": "Author added successfully",
                "authorid": 123
            }
            ```  
        - **On Failure (Token already used):**
            ```json
            {
                "status": "fail",
                "data": "Token has already been used. Please provide a new token."
            }
            ```  
        - **On Failure (Invalid token):**
            ```json
            {
                "status": "fail",
                "data": "Invalid token."
            }
            ```  
        - **On Failure (Internal Server Error):**
            ```json
            {
                "status": "fail",
                "data": "Error message here"
            }
            ```  

11. ### Delete Author  
    - **Endpoint:** `/author/delete`  
    - **Method:** `POST`  
    - **Description:**  
        This API endpoint allows for deleting an author from the library based on the provided `authorid`. It validates the token, marks it as used, and then deletes the author.  
    - **Sample Request (JSON):**
        ```json
        {
            "authorid": 123,
            "token": "your_jwt_token_here"
        }
        ```  
    - **Response:**  
        - **On Success:**
            ```json
            {
                "status": "success",
                "data": "Author deleted successfully"
            }
            ```  
        - **On Failure (Token already used):**
            ```json
            {
                "status": "fail",
                "data": "Token has already been used. Please provide a new token."
            }
            ```  
        - **On Failure (Invalid token):**
            ```json
            {
                "status": "fail",
                "data": "Invalid token."
            }
            ```  
        - **On Failure (Internal Server Error):**
            ```json
            {
                "status": "fail",
                "data": "Error message here"
            }
            ```  

12. ### Update Author  
    - **Endpoint:** `/author/update`  
    - **Method:** `POST`  
    - **Description:**  
        This API endpoint allows for updating the name of an existing author based on the provided `authorid`. It validates the token, marks it as used, and updates the author's name in the database.  
    - **Sample Request (JSON):**
        ```json
        {
            "authorid": 123,
            "name": "New Author Name",
            "token": "your_jwt_token_here"
        }
        ```  
    - **Response:**  
        - **On Success:**
            ```json
            {
                "status": "success",
                "data": "Author updated successfully"
            }
            ```  
        - **On Failure (Token already used):**
            ```json
            {
                "status": "fail",
                "data": "Token has already been used. Please provide a new token."
            }
            ```  
        - **On Failure (Invalid token):**
            ```json
            {
                "status": "fail",
                "data": "Invalid token."
            }
            ```  
        - **On Failure (Internal Server Error):**
            ```json
            {
                "status": "fail",
                "data": "Error message here"
            }
            ```  

13. ### Display All Authors  
    - **Endpoint:** `/author/display`  
    - **Method:** `POST`  
    - **Description:**  
        This API endpoint retrieves all authors from the database, validating the token and ensuring it is not previously used. It then returns a list of authors with their `authorid` and `name`.  
    - **Sample Request (JSON):**
        ```json
        {
            "token": "your_jwt_token_here"
        }
        ```  
    - **Response:**  
        - **On Success:**
            ```json
            {
                "status": "success",
                "data": [
                    {"authorid": 1, "name": "Author One"},
                    {"authorid": 2, "name": "Author Two"},
                    // more authors
                ]
            }
            ```  
        - **On Failure (Token already used):**
            ```json
            {
                "status": "fail",
                "data": "Token has already been used. Please provide a new token."
            }
            ```  
        - **On Failure (Invalid token):**
            ```json
            {
                "status": "fail",
                "data": "Invalid token."
            }
            ```  
        - **On Failure (Internal Server Error):**
            ```json
            {
                "status": "fail",
                "data": "Error message here"
            }
            ```  

14. ### Add Book-Author Association  
    - **Endpoint:** `/books_authors/add`  
    - **Method:** `POST`  
    - **Description:**  
        This API endpoint associates a book with an author in the `books_authors` table by using their respective `bookid` and `authorid`. It validates the provided token and ensures it is not previously used.  
    - **Sample Request (JSON):**
        ```json
        {
            "bookid": 1,
            "authorid": 2,
            "token": "your_jwt_token_here"
        }
        ```  
    - **Response:**  
        - **On Success:**
            ```json
            {
                "status": "success",
                "data": "Book-Author association added successfully."
            }
            ```  
        - **On Failure (Token already used):**
            ```json
            {
                "status": "fail",
                "data": "Token has already been used. Please provide a new token."
            }
            ```  
        - **On Failure (Invalid token):**
            ```json
            {
                "status": "fail",
                "data": "Invalid token."
            }
            ```  
        - **On Failure (Internal Server Error):**
            ```json
            {
                "status": "fail",
                "data": "Error message here"
            }
            ```  

15. ### Delete Book-Author Association  
    - **Endpoint:** `/books_authors/delete`  
    - **Method:** `DELETE`  
    - **Description:**  
        This API endpoint deletes the association between a book and an author from the `books_authors` table using the `collectionid`. It verifies the provided token and ensures it is not previously used before performing the deletion.  
    - **Sample Request (JSON):**
        ```json
        {
            "collectionid": 1,
            "token": "your_jwt_token_here"
        }
        ```  
    - **Response:**  
        - **On Success:**
            ```json
            {
                "status": "success",
                "data": "Book-Author association deleted successfully."
            }
            ```  
        - **On Failure (Token already used):**
            ```json
            {
                "status": "fail",
                "data": "Token has already been used. Please provide a new token."
            }
            ```  
        - **On Failure (Invalid token):**
            ```json
            {
                "status": "fail",
                "data": "Invalid token."
            }
            ```  
        - **On Failure (Internal Server Error):**
            ```json
            {
                "status": "fail",
                "data": "Error message here"
            }
            ```  

16. ### Display All Book-Author Associations  
    - **Endpoint:** `/books_authors/display`  
    - **Method:** `GET`  
    - **Description:**  
        This API endpoint retrieves all the book-author associations from the `books_authors` table. It verifies the provided token before fetching the data and ensures that the token has not been used previously.  
    - **Sample Request (JSON):**
        ```json
        {
            "token": "your_jwt_token_here"
        }
        ```  
    - **Response:**  
        - **On Success:**
            ```json
            {
                "status": "success",
                "data": [
                    {
                        "bookid": 1,
                        "authorid": 2,
                        "collectionid": 1
                    },
                    {
                        "bookid": 3,
                        "authorid": 4,
                        "collectionid": 2
                    }
                   
                ]
            }
            ```  
        - **On Failure (Token already used):**
            ```json
            {
                "status": "fail",
                "data": "Token has already been used. Please provide a new token."
            }
            ```  
        - **On Failure (Invalid token):**
            ```json
            {
                "status": "fail",
                "data": "Invalid token."
            }
            ```  
        - **On Failure (Internal Server Error):**
            ```json
            {
                "status": "fail",
                "data": "Error message here"
            }
            ```  
