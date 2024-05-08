# CHAPTER 1: Scale From Zero To Millions of Users

In this chapter, we learn how to build systems that support a single user and then we can scale it up to support millions.

## Single Server Setup

For this design, we put everything in one server like Web App, Database, Cache, etc.<br/>

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-1.png">

### How does the flow goes here?

1. A user asks a DNS (a third party keeping domain names corresponding to IP address) what is our IP address.
2. IP (Internet Protocol) is returned back to the user.
3. An HTTP request is made to our web server based on the IP address.
4. Our web server responses back a JSON (maybe an HTML, meta data, etc.)

### Example of a Payload

```
# GET /users/12 - Retrieve user object with an id of 12
{
    "id":12,
    "firstName":"John",
    "lastName":"Smith",
    "address":{
        "streetAddress":"21 2nd Street",
        "city":"New York",
        "state":"NY",
        "postalCode":10021
    },
    "phoneNumbers":[
        "212 555-1234",
        "646 555-4567"
  ]
}
```
## Database
With an increase of users, we need multiple servers, each deal with each task. For example, one server is for web server and one more server is for database. This allows each server to grow independently.
<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-3.png"> 