# Introduction:

Welcome to the UgaTunes API, the driving force behind our mission to expose, promote, and celebrate Uganda through music. UgaTunes is more than just a music website; it's a unified Ugandan content distribution and publishing administration platform. Our goal is to empower content creators by providing the tools they need to increase their exposure and connect with audiences worldwide.

**Base URL:** `api.ugatunes.com`

With this API, you can access a wealth of music, artists, albums, and genres, all curated to showcase the vibrant Ugandan music scene. Whether you're an established artist or an up-and-coming talent, our platform is designed to help you reach international audiences and earn recognition for your work.

In this documentation, we'll explore the resources and endpoints that enable you to tap into the essence of Uganda's music culture.

# Authentication and Tokens:

To access the UgaTunes API, you'll need to authenticate using JSON Web Tokens (JWT). JWT is a secure and widely adopted method for authenticating users and securing API endpoints.

Here's a high-level overview of how JWT authentication works with our API:

1. **User Registration - `/user` (POST):**

    This endpoint allows users to create an account on UgaTunes. Users should provide their personal information, including first name, last name, username, phone number, email, and password.

    **Request:**

    ```http
    POST /api.ugatunes.com/user
    Content-Type: application/json

    {
        "first_name": "Your First Name",
        "last_name": "Your Last Name",
        "username": "yourusername",
        "phone": "yourphone",
        "email": "user@example.com",
        "password": "yourpassword"
    }
    ```

    **Response:**

    Upon successful registration, the API will respond with a JSON object containing the following information:

    ```json
    {
    	"username": "yourusername",
    	"phone": "yourphone",
    	"email": "user@example.com",
    	"role": "user",
    	"token": "[JWT Token]"
    }
    ```

    The response includes the user's username, phone number, email, role (with the default value of "user"), and the JWT token for authentication.

2. **Register as Artist - `/user/artist-register` (POST):**

    This endpoint allows artists to register on UgaTunes with artist-specific information. To use this endpoint, users need to be authenticated by including a valid JWT token in the Authorization header.

    **Request:**

    ```http
    POST /api.ugatunes.com/user/artist-register
    Content-Type: application/json
    Authorization: Bearer [JWT Token]

    {
        "image": "URL or Base64-encoded image data",
        "stage_name": "Artist Stage Name",
        "agree_terms": true
    }
    ```

    - `image`: URL or Base64-encoded image data representing the artist's profile picture.
    - `stage_name`: The artist's stage name or alias.
    - `agree_terms`: A boolean indicating whether the artist agrees to the terms and conditions (you can remove this field if not needed).

    **Response:**

    Upon successful registration as an artist, the API will respond with a JSON object containing the following information:

    ```json
    {
        "username": "yourusername",
        "email": "user@example.com",
        "stage_name": "Artist Stage Name",
        "image": "URL to the artist's profile picture",
        "first_name": "Your First Name",
        "last_name": "Your Last Name",
        "role": "artist",
        "phone": "yourphone"
    }
    ```

    - `username`: The username associated with the artist's account.
    - `email`: The registered email address.
    - `stage_name`: The artist's stage name.
    - `image`: URL to the artist's profile picture.
    - `first_name`: The artist's first name.
    - `last_name`: The artist's last name.
    - `role`: The user's role, which is "artist" in this case.
    - `phone`: The phone number associated with the artist's account.

3. **User Login - `/user/login` (POST):**

    This endpoint allows users to log in to their accounts. Users should provide their username and password.

    **Request:**

    ```http
    POST /api.ugatunes.com/user/login
    Content-Type: application/json

    {
        "username": "yourusername",
        "password": "yourpassword"
    }
    ```

    - `username`: The username associated with the user's account.
    - `password`: The user's password.

    **Response:**

    Upon successful login, the API will respond with a JSON object containing the following information:

    ```json
    {
        "email": "user@example.com",
        "username": "yourusername",
        "token": "[JWT Token]"
    }
    ```

    - `email`: The registered email address.
    - `username`: The username associated with the user's account.
    - `token`: A JSON Web Token (JWT) for authentication. Include this token in the `Authorization` header for future API requests.

    The API will also set a cookie named `__gx_ucd` with the JWT token, which should be included in subsequent requests for authentication.

4. **User Logout - `/user/logout` (POST):**

    This endpoint allows users to log out from their current session. It requires a valid JWT token in the Authorization header.

    **Request:**

    ```http
    POST /api.ugatunes.com/user/logout
    Authorization: Bearer [JWT Token]
    ```

    - `Authorization`: The `Bearer` token should include a valid JWT token obtained during login.

    **Response:**

    Upon successful logout, the API will respond with a confirmation message:

    ```json    
    {
        "message": "Logout successful."
    }
    ```

    Users can use this endpoint to securely end their current session and invalidate the JWT token.


# Artists Resources

1. **Fetch Profile - `/user/profile/<str:username>` (GET):**

    This endpoint allows users to fetch the profile of a specific artist using their username.

    **Request:**

    ```http
    GET /api.ugatunes.com/user/profile/<str:username>
    ```

    - Replace `<str:username>` with the actual username of the artist.

    **Response:**

    The API will respond with a JSON object containing the artist's profile information.

2. **List All Artists - `/artists/all` (GET):**

    This endpoint retrieves a list of all available artists.

    **Request:**

    ```http
    GET /api.ugatunes.com/artists/all
    ```

    - You can include query parameters like `includes=trending` to customize the results.

    **Response:**

    The API will respond with a JSON array containing information about all available artists.

# Music Resources

### Music Resources

1. **List All MP3 - `/mp3/all` (GET):**

   This endpoint retrieves a list of all available MP3 tracks.

   **Request:**

   ```http
   GET /api.ugatunes.com/mp3/all
   ```

   - You can include query parameters like `includes=approved` to filter the results.
   - To order the results for a specific user, use query parameters like `user=<str:username>&order=asc` (replace `<str:username>` with the actual username).

   **Response:**

   The API will respond with a JSON array containing information about all available MP3 tracks.

2. **Fetch a Music/Song - `/mp3/<str:slug>` (GET):**

   This endpoint allows users to fetch details of a specific MP3 track or song using its unique slug.

   **Request:**

   ```http
   GET /api.ugatunes.com/mp3/<str:slug>
   ```

   - Replace `<str:slug>` with the actual slug of the MP3 track or song.

   **Response:**

   The API will respond with a JSON object containing information about the requested MP3 track or song.

3. **List Favorite MP3 - `/mp3/favorites` (GET):**

   This endpoint retrieves a list of MP3 tracks marked as favorites by the user.

   **Request:**

   ```http
   GET /api.ugatunes.com/mp3/favorites
   ```

   - You can include query parameters or filters as needed.

   **Response:**

   The API will respond with a JSON array containing information about the user's favorite MP3 tracks.

# Albums Resources

1. **List All Albums - `/albums/all` (GET):**

   This endpoint retrieves a list of all available albums.

   **Request:**

   ```http
   GET /api.ugatunes.com/albums/all
   ```

   **Response:**

   The API will respond with a JSON array containing information about all available albums.

2. **Fetch an Album - `/albums/<str:slug>` (GET):**

   This endpoint allows users to fetch details of a specific album using its unique slug.

   **Request:**

   ```http
   GET /api.ugatunes.com/albums/<str:slug>
   ```

   - Replace `<str:slug>` with the actual slug of the album.

   **Response:**

   The API will respond with a JSON object containing information about the requested album.

