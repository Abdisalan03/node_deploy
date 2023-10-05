# Documentation for Bookstore API

## Introduction
Welcome to the API documentation for the Bookstore API. This API provides endpoints for managing bookstores, authors, and books. The API is designed to be simple and easy to use, with clear and concise documentation. The API is hosted at 

### Authentication
Most endpoints in the API require authentication using ``JSON`` Web Tokens (JWT). To authenticate, include the Authorization header in the request with the JWT. The header should be in the format: Bearer <token>, where <token> is the JWT obtained during the login process.

#### **Endpoints**
The Bookstore API provides the following endpoints:

## Owner Signup
**Endpoint**: 
   - `POST localhost:4000/api/owners/signup`

This endpoint is used to create a new owner account. The request should be a POST request with the following parameters:

##### Request body :
| parameter | Type   | Required    | Description   
|---------------|-----|------------|-------------------|
| name   	| String  | Yes    | The name of the owner.
| email      | String     | Yes	    | The email of the owner.
| Password  	| String  	| Yes | The password of the owner.


##### Response :
```````js
 {
  "status": 201,
  "message": "Owner created successFully",
  "newOwner": {
    "id": 2,
    "name": "",
    "email": "yaskaassoy@gmail.com",
    "password": "$2b$10$XrCBl8L/VETAz1ZoNfqMxuBd3alzGRzoBOy9yx5Kgeved83NYYc1W",
    "created": "2023-10-02T03:47:42.765Z",
    "updated": "2023-10-02T03:47:42.765Z" 
  } 
} 
```````



### Owner Login
**Endpoint**: 
-  ```POST localhost:4000/api/owners/login```

This endpoint is used to log in an existing owner. The request should be a POST request with the following parameters:

##### Request body :
| parameter | Type   | Required    | Description   |
|---------------|-----|--------------|-------------------|
| email   	| String  | Yes    | The email of the owner.
| password     | String     | Yes	    | The passowrd of the owner.

##### Response :
```js
{
  "status": 200,
  "message": "Owner logged in successfully",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6MywiZW1haWwiOiJ5YXNrYWFzc28zeUBnbWFpbC5jb20iLCJpYXQiOjE2OTYyMjE4MDgsImV4cCI6MTY5NjIyNTQwOH0.13JEak-uRxX-9x2Wf15Otwee4NJvBwaxXZWsUwhv0sg"
}
```
If the owner does not exist, a ```404 Not Found```response will be returned. If the password is incorrect, a ```401 Unauthorized``` response will be returned. If the request is successful, a ```200 OK``` response will be returned with a JSON Web Token (JWT) that can be used to authenticate future requests.

### Authentication Middleware
This middleware function is used to authenticate requests by verifying the JSON Web Token (JWT) included in the Authorization header.

###### Middleware Function:


```js 
const authenticate = (req, res, next) => {
  const token = req.headers.authorization;

  if (!token) {
    return res
      .status(401)
      .json({ status: 401, message: "Authentication failed - missing token" });
  }
  

   const tokenWithoutBearer = token.split(" ")[1];

  jwt.verify(tokenWithoutBearer, SECRET_KEY, (error, decoded) => {
    if (error) {
      return res
        .status(401)
        .json({ status: 401, message: "Authentication failed - invalid token" });
    }

    req.decoded = decoded;

    next();
  });
};
 ```

#### Usage
To use this middleware, include it in the route handler function before the desired functionality.


```js 
 router.get("/protected", authenticate, (req, res) => {
  // Accessible only if authenticated
  // Your logic here
});
Error Responses
Status Codes:
401 - Unauthorized
401 - Unauthorized
Missing Token:

{
  "status": 401,
  "message": "Authentication failed - missing token"
}
```

###### Invalid Token:
```js 
{
  "status": 401,
  "message": "Authentication failed - invalid token"
}
```

##### Bookstore Endpoints:
Create BookStore
EndPoint: POST /api/bookstore

This endpoint is used to create a new bookstore. The request should be a POST request with the following parameters:

##### Request body :
| parameter | Type   | Required    | Description   |
|---------------|-----|--------------|-------------------|
| ownerId	 | Number   |	Yes	| The OwnerId of the bookstore.      |      
| name	  | String  | Yes	|The name of the bookstore.
| location	| String	| Yes	| The location of the bookstore.
##### Response
```js
{
  "status": 200,
  "message": "BookStore successFully created!"
}
```
If the bookstore was not created, a ```400``` Bad Request response will be returned. If the request is successful, a ```200 OK  ``` response will be returned with a message indicating that the bookstore was created successfully.

### Bookstore Update
**Endpoint**:
- ```PUT localhost:4000/bookstore/:id```

This ``endpoint`` is used to update an existing bookstore. The request should be a PUT request with the following parameters:

#### Request body :
| parameter | Type   | Required    | Description   |
|---------------|-----|--------------|-------------------|
| ownerId	 | Number  	|Yes	   | The OwnerId of the bookstore.
| name  	| String	 |Yes	  | The name of the bookstore.
| location	| String	| Yes	 | The location of the bookstore.  |

If the bookstore does not exist, a 404 Not Found response will be returned. If the update was not successful, a 500 Internal Server Error response will be returned. If the request is successful, a 200 OK response will be returned with a message indicating that the bookstore was updated successfully.

### Response :

```js
 {
  "status": 200,
  "message": "BookStore successFully updated!"
} 
``````


#### Create New  Author
**EndPoint**:
-  ```POST localhost:4000/api/authors/```

This endpoint is used to create a new author. The request should be a POST request with the following parameters:

##### Request body :
| parameter | Type   | Required    | Description   |
|---------------|-----|--------------|-------------------|
| name	 | String	 | Yes	 |The name of the author. |

##### Response
```js
{
  "status": 200,
  "message": "Author successFully created!"
}
```
If the author was not created, a ```400``` Bad Request response will be returned. If the request is successful, a ```200 OK ``` response will be returned with a message indicating that the author was created successfully.



#### Create book
**EndPoint**: 
-  ```POSTPOST localhost:4000 /api/books```

This endpoint is used to create a new book. The request should be a POST request with the following parameters:

##### Request body :
| parameter | Type   | Required    | Description   |
|---------------|-----|--------------|-------------------|
| authorId	  | String	  |Yes	 | The authorId of the book. 
| bookstoreId	 | String	|Yes	 | The bookstoreId of the book.
| title	  | String	 | Yes	 | The title of the book.
| price	 | Number	 |  Yes	    | The price of the book.
| image	 | String	 |  Yes	  | The image of the book. |

##### Response
 ```js
{
  "status": 200,
  "message": "Book successFully created!"
}
 ``` 
If the book was not created, a ```400 ``` Bad Request response will be returned. If the request is successful, a  ```200 OK ``` response will be returned with a message indicating that the book was created successfully.

##### Use Supabase database
Instead of using SQLite database, change to Supabase by following this instruction:

1. Sign up at supabase.com
2. Create new project
3. Inside the project you created, go to ```Settings``` and then click ```Databases```
4. Under ```Connection string```, switch to```url``` and copy the link.
5. Create ````.env```` file in your project's root directory if you already don't have it.
6. Add ```DATABASE_URL='the url you copied'``` in the .env file.
7. Inside ```prisma``` folder, you will have ```prisma.schema``` file, change the ```datasource``` to
```
datasource db {
 provider          = "postgresql"
 url               = env("DATABASE_URL")
}
````