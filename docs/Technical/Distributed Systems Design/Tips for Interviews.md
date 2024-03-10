# Tips for Interviews

## Writing Service Interfaces

Write the outline of the service interface to easily describe the service behavior to the interviewer, by including:

1. **message data structures** - describes the request and response messages that a service uses to communicate over a netwrk
2. **method signature** - contains a methods name, return type, and parameter list

```bash
GetCourseRequest
    int64 user_id
    int64 timestamp
    string course_name

GetCourseResponse
    int64 userid
    repeated string lessons
    int64 timestamp

# CourseService has a method getCourse which accepts a request message GetCourseRequest and returns the response message GetCourseResponse

# It accepts a single argument, the request, and returns a single object, the response

CourseService
    GetCourseResponse getCourse (GetCourseRequest request)

```

==Reminder==: the **message data structure** is different from the **data model** - e.g. song_names could be stored as rows that are aggregated into a sequential datastructure like an array in the course response.

A system would have multiple services, and each service would have multiple methods. Making them descriptive makes the interview self-explanatory and self-documenting. 

## Database Model

Describes the logical structure, layout, and relationships of data that reside in a database. 

