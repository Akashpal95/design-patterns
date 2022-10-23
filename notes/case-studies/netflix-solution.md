## Exercise

Design the class Diagram and database Schema for a system like Netflix with following Use Cases.

* Netflix has users.
* Every user has an email and a password.
* Users can create profiles to have separate independent environments.
* Each profile has a name and a type. Type can be KID or ADULT.
* There are multiple videos on netflix.
* For each video, there will be a title, description and a cast.
* A cast is a list of actors who were a part of the video. For each actor we need to know their name and list of videos they were a part of.
* For every video, for any profile who watched that video, we need to know the status (COMPLETED/ IN PROGRESS).
* For every profile for whom a video is in progress, we want to know their last watch timestamp.

## Solution

#### Class Diagram
```mermaid
classDiagram
    direction LR
    class User {
        +String name
        +String email
        -String password
        -String phone
        +getName() String
        +getEmail() String
        +getPhone() String
    }
    
    class Profile {
        +User user
        +ProfileType type
        +String name
        +Video[] videos
        +getVideoStatus(Video) : String
        +getVideoProgress(Video) : Datetime
    }

    class ProfileType {
        <<enumeration>>
        KID
        ADULT
    }

    Profile "*"-->"1" ProfileType
    User "1"*--"*" Profile : has

    class VideoStatus{
        <<enumeration>>
        COMPLETED
        IN_PROGRESS
        RECOMMENDED
    }

    class Video {
        +String title
        +String description
        +Actor[] cast
    }
    
    class Actor {
        +String name
    }

    Profile "1" --o "*" Video
    Video "*"--o"*" Actor


    
```

#### ER Diagram
```mermaid
erDiagram
    USER {
        int id PK
        varchar name
        varchar email
        varchar password
        varchar phone
    }

    PROFILE {
        int id PK
        varchar name
        enum type
        int user_id FK
    }

    USER ||--|{ PROFILE : creates
    
    VIDEO {
        int id PK
        varchar title
        varchar description
    }
    ACTOR {
        int id PK
        varchar name
    }
    CAST {
        int id PK
        int video_id FK
        int actor_id FK
    }

    PROFILE_VIDEO {
        int id PK
        int profile_id FK
        int video_id FK
    }

    PROFILE ||--o{ PROFILE_VIDEO : has
    VIDEO ||--o{ PROFILE_VIDEO : has
    VIDEO ||--|{ CAST : has
    ACTOR ||--|{ CAST : part_of


```
