# architect-social-media-platform
architect scalable social media platform
# Design a Highly Scalable Image Sharing Social Media Platform

## Gathering Functional Requirements

- What kind of information do we need to store about each registered user?
- What type of media can the user share?
  - Only images? Text? Video? Etc.
- What type of relationship do we have between users?
- What kind of operations can a user perform on the platform?

### In Scope

- When a user registers, they provide:
  - Mandatory: first name, last name, email address, password, profile image
  - Optional: age, location, interests, etc.
- Users can share only images.
- Every user can follow any user they want.

### Functional User Actions

#### In Scope

- A user can:
  - Post a new image
  - Follow or unfollow a user
  - Search for other users
  - View other users' public info and shared images
  - See a timeline feed posted by the people they follow

#### Out of Scope

- Reactions
- Comments
- Sharing
- Etc.

## Gather Non-Functional Requirements

### Scalability

- Billions of active users, like Facebook
- Most likely hundreds of millions of visits per day
- Each user uploads 1 image per day (2 MB)
- Data processing volume: 1 PB/day

### Availability

- 99.99% uptime

### Performance

- Since end users interact with the system directly, we need to reduce response time and page load time for every interaction on the platform.
- Target around 500 ms end-to-end latency at the 99th percentile.

## Sequence Flow

sequenceDiagram
    autonumber

    actor UserA
    actor UserB

    participant API
    participant AuthService
    participant UserService
    participant PostService

    %% Registration
    UserA->>API: Register
    API->>UserService: Create User
    UserService-->>API: User Created
    API->>AuthService: Generate Token
    AuthService-->>API: JWT Token
    API-->>UserA: Success

    %% Create Post
    UserA->>API: Upload Image Post
    API->>PostService: Create Post
    PostService-->>API: Post Created
    API-->>UserA: Success

    %% Search User
    UserB->>API: Search User
    API->>UserService: Search Users
    UserService-->>API: User Profile
    API-->>UserB: Results

    %% Follow User
    UserB->>API: Follow UserA
    API->>UserService: Follow User
    UserService-->>API: Follow Created
    API-->>UserB: Success

    %% View Profile
    UserB->>API: View UserA Profile
    API->>UserService: Get Profile
    UserService-->>API: Profile Details

    API->>PostService: Get User Posts
    PostService-->>API: Posts

    API-->>UserB: Profile + Posts
