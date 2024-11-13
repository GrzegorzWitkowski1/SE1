# Hitchhiking System API Documentation (Revised)

## Introduction

This API allows users to manage hitchhiking trips, search for potential companions, rate hitchhiking spots, create events, and interact with emergency services. It supports user registration, profile management, trip posting, event participation, spot rating, achievement tracking, and more. The system is designed for scalability and performance, ensuring smooth interaction between users, events, and locations.

## Authentication

All API requests that require user identification must be authenticated using JWT (JSON Web Tokens). Upon successful login, a JWT token is issued that must be included in the `Authorization` header for all protected endpoints.

### Authentication Endpoints

#### Register a New User

**POST /register**

Registers a new user in the system.

- **Request Body:**
    ```json
    {
        "username": "johndoe",
        "email": "johndoe@example.com",
        "password": "password123",
        "confirm_password": "password123",
        "bio": "Adventurer looking for companions."
    }
    ```

- **Response:**
    - `201 Created`
    - ```json
      {
          "message": "Registration successful.",
          "user_id": 123
      }
      ```

#### Login

**POST /login**

Logs in a user and returns a JWT token.

- **Request Body:**
    ```json
    {
        "email": "johndoe@example.com",
        "password": "password123"
    }
    ```

- **Response:**
    - `200 OK`
    - ```json
      {
          "token": "jwt-token-here"
      }
      ```

#### Logout

**GET /logout**

Logs out the user by invalidating the current session.

- **Response:**
    - `200 OK`
    - ```json
      {
          "message": "Logout successful."
      }
      ```

## User Profile Management

### Retrieve User Profile

**GET /user/profile**

Fetches the profile details of the authenticated user.

- **Response:**
    - `200 OK`
    - ```json
      {
          "username": "johndoe",
          "email": "johndoe@example.com",
          "bio": "Adventurer looking for companions."
      }
      ```

### Update User Profile

**PUT /user/profile**

Updates the authenticated user's profile (e.g., bio, photo).

- **Request Body:**
    ```json
    {
        "bio": "New bio information."
    }
    ```

- **Response:**
    - `200 OK`
    - ```json
      {
          "message": "Profile updated successfully."
      }
      ```

## Trip Management

### Post a Trip

**POST /trip**

Allows users to post their upcoming hitchhiking trip with goals, approximate location, destination, and details.

- **Request Body:**
    ```json
    {
        "trip_name": "Hitchhike from NYC to LA",
        "start_date": "2024-12-01T00:00:00Z",
        "end_date": "2024-12-15T00:00:00Z",
        "approximate_location": "New York City, NY",
        "destination": "Los Angeles, CA",
        "goals": "Looking for a companion to join me on the road."
    }
    ```

- **Response:**
    - `201 Created`
    - ```json
      {
          "trip_id": 45,
          "message": "Trip posted successfully."
      }
      ```

### Retrieve Trips

**GET /trips**

Searches for trips based on goals, location, and other parameters.

- **Query Parameters:**
    - `goal`: Filter trips by goals.
    - `location`: Filter trips by location.
    - `start_date`: Filter trips by start date.
    - `end_date`: Filter trips by end date.

- **Response:**
    - `200 OK`
    - ```json
      [
          {
              "trip_id": 45,
              "trip_name": "Hitchhike from NYC to LA",
              "start_date": "2024-12-01T00:00:00Z",
              "end_date": "2024-12-15T00:00:00Z",
              "approximate_location": "New York City, NY",
              "destination": "Los Angeles, CA",
              "goals": "Looking for a companion."
          }
      ]
      ```

### Delete Trip

**DELETE /trip/{trip_id}**

Deletes an existing trip.

- **Response:**
    - `200 OK`
    - ```json
      {
          "message": "Trip deleted successfully."
      }
      ```

### Respond to Trip

**POST /trip/{trip_id}/respond**

A user can respond to a trip post, showing interest or asking for more details.

- **Request Body:**
    ```json
    {
        "message": "I’m interested, tell me more about your trip."
    }
    ```

- **Response:**
    - `200 OK`
    - ```json
      {
          "message": "Response sent successfully."
      }
      ```

## Spot Management

### Post a Spot

**POST /spot**

Users can add a new hitchhiking spot, including tips and an initial rating.

- **Request Body:**
    ```json
    {
        "location": "Los Angeles, CA",
        "tips": "Good spot near the freeway exit. Look for the gas station.",
        "initial_rating": 4,
        "waiting_time": "10-15 minutes"
    }
    ```

- **Response:**
    - `201 Created`
    - ```json
      {
          "spot_id": 123,
          "message": "Spot posted successfully."
      }
      ```

### Retrieve Spots

**GET /spots**

Retrieve a list of spots based on filters like location, ratings, and waiting time.

- **Query Parameters:**
    - `location`: Search spots by location.
    - `rating`: Search spots by rating.
    - `waiting_time`: Search spots by average waiting time.

- **Response:**
    - `200 OK`
    - ```json
      [
          {
              "spot_id": 123,
              "location": "Los Angeles, CA",
              "tips": "Good spot near the freeway exit. Look for the gas station.",
              "rating": 4.5,
              "waiting_time": "10-15 minutes"
          }
      ]
      ```

### Rate a Spot

**POST /spot/{spot_id}/rate**

Users can rate a hitchhiking spot based on their experience, including waiting time.

- **Request Body:**
    ```json
    {
        "rating": 5,
        "comment": "Great spot, easy to get a ride.",
        "waiting_time": "5-10 minutes"
    }
    ```

- **Response:**
    - `200 OK`
    - ```json
      {
          "message": "Spot rated successfully."
      }
      ```

### Approve a Spot (Admin)

**POST /spot/{spot_id}/approve**

Approves a spot for visibility.

- **Response:**
    - `200 OK`
    - ```json
      {
          "message": "Spot approved."
      }
      ```

### Disapprove a Spot (Admin)

**POST /spot/{spot_id}/disapprove**

Disapproves a spot for visibility.

- **Response:**
    - `200 OK`
    - ```json
      {
          "message": "Spot disapproved."
      }
      ```

## Event Management

### Create an Event

**POST /event**

Allows event organizers to create new hitchhiking-related events.

- **Request Body:**
    ```json
    {
        "event_name": "Hitchhiking Meetup in NYC",
        "location": "Central Park, NYC",
        "date": "2024-12-10T14:00:00Z",
        "details": "Join us for a hitchhiking event!"
    }
    ```

- **Response:**
    - `201 Created`
    - ```json
      {
          "event_id": 25,
          "message": "Event created successfully."
      }
      ```

### Retrieve Events

**GET /events**

List all events or filter by date, location, or type.

- **Query Parameters:**
    - `location`: Filter events by location.
    - `date`: Filter events by date.

- **Response:**
    - `200 OK`
    - ```json
      [
          {
              "event_id": 25,
              "event_name": "Hitchhiking Meetup in NYC",
              "location": "Central Park, NYC",
              "date": "2024-12-10T14:00:00Z",
              "details": "Join us for a hitchhiking event!"
          }
      ]
      ```

### Join an Event

**POST /event/{event_id}/join**

Allows a user to join an event.

- **Response:**
    - `200 OK`
    - ```json
      {
          "message": "Successfully joined the event."
      }
      ```

### Rate an Event

**POST /event/{event_id}/rate**

Users can rate an event they attended.

- **Request Body:**
    ```json
    {
        "rating": 4,
        "comment": "It was fun, but I would have liked more activities."
    }
    ```

- **Response:**
    - `200 OK`
    - ```json
      {
          "message": "Event rated successfully."
      }
      ```

## Notifications

### Get Notifications

**GET /notifications**

Fetches all notifications for the authenticated user (e.g., new trip responses, event updates).

- **Response:**
    - `200 OK`
    - ```json
      [
          {
              "notification_id": 1,
              "message": "You have a new response to your trip post.",
              "type": "response",
              "read": false
          }
      ]
      ```

### Mark Notification as Read

**POST /notifications/mark_as_read**

Marks a specific notification as read.

- **Request Body:**
    ```json
    {
        "notification_id": 1
    }
    ```

- **Response:**
    - `200 OK`
    - ```json
      {
          "message": "Notification marked as read."
      }
      ```

## Emergency Services

### SOS Button

**POST /user/{user_id}/sos**

Triggers an emergency alert, notifying contacts and nearby users.

- **Response:**
    - `200 OK`
    - ```json
      {
          "message": "Emergency alert sent successfully."
      }
      ```

### Emergency Contacts

**GET /user/{user_id}/emergency**

Retrieves emergency contact details for a user.

- **Response:**
    - `200 OK`
    - ```json
      {
          "emergency_contacts": [
              {"name": "Jane Doe", "phone": "+1234567890"}
          ]
      }
      ```

## Achievement System

### List Achievements

**GET /achievements**

Fetches a list of achievements available for users (e.g., "First Trip Posted," "Top Spot Contributor").

- **Response:**
    - `200 OK`
    - ```json
      [
          {
              "achievement_id": 1,
              "title": "First Trip Posted",
              "description": "Awarded for posting your first trip."
          }
      ]
      ```

### User Achievements

**GET /user/achievements**

Retrieves a list of achievements unlocked by the authenticated user.

- **Response:**
    - `200 OK`
    - ```json
      [
          {
              "achievement_id": 1,
              "title": "First Trip Posted",
              "description": "Awarded for posting your first trip.",
              "date_achieved": "2024-01-01T00:00:00Z"
          }
      ]
      ```

## Reporting Users

### Report a User

**POST /user/{user_id}/report**

Allows a user to report another user for inappropriate behavior or safety concerns.

- **Request Body:**
    ```json
    {
        "reason": "Inappropriate behavior during a trip."
    }
    ```

- **Response:**
    - `200 OK`
    - ```json
      {
          "message": "User reported successfully."
      }
      ```

## Admin Operations

### Verify Spot (Admin)

**POST /spot/{spot_id}/verify**

Admin verifies a spot for its validity or safety.

- **Response:**
    - `200 OK`
    - ```json
      {
          "message": "Spot verified."
      }
      ```

### Approve or Disapprove Spot (Admin)

**POST /spot/{spot_id}/approve**  
**POST /spot/{spot_id}/disapprove**

Admin approves or disapproves a spot for visibility on the platform.

- **Response:**
    - `200 OK`
    - ```json
      {
          "message": "Spot approved/disapproved."
      }
      ```
      
