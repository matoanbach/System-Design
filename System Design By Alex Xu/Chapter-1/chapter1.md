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

### Which one to use, non-relational or relational database?

Relational databases are also called a relational database management system (RDBMS). With this type of database, we can perform joins based on the relationship of each object. However, these are usually the reason why relational database is slow.

Non-relation databases are also call NoSQL database. Because join operations are usually not supported, these databases are usually faster.

| Non-relational  | Relational |
| :-------------: | :--------: |
|     My SQL      |  CouchDB   |
| Oracle Database |   Neo4j    |
|   PostgreSQL    | Cassandra  |
|       ...       |    ...     |

## Vertical scaling vs horizontal scaling
Vertical Scaling (scale-up) means that we can make the server (or make the computer that the server is running on bigger). Horizontal Scaling (scale-out) means that we can add more servers (or we add more computers or instances)

Vertical Scaling gives one server more power (CPU power or RAM, etc.). But nowadays, adding computer power has more limitation than add more computers with the same power. Even though vertical scaling reduce the redundancy, it is disaster if one server goes down, the whole web server goes down as well.

Because of those reasons, horizontal scaling is more desirable for modern and large scale applications. However, a load balancer needs to be there for this kind of scaling to operate smoothly.

## Load Balancer

A Load balancer distributes incoming traffic equally web servers.

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-4.png">

Users request public IP as usual. After that users request to the servers, now the load balancer comes in because it has the public IP of our server, and then it communicates with our servers with private IPs.

Using a load balancer in our system, we can solve failover or our web server shut down. For example, if one server goes down, our load balancer will direct the traffic toward other servers. If the website grows rapidly, we just need to add more servers.

When we solve a problem, usually, another will come up to us. Here, we add more servers, causing failover redundancy to our database server with read/write operations. Well, we can do data replication to solve this problem. 


## Database Replication

The methodology behind database replication is the relationship between the original (master) and the copies (slave).

All data-modifying operations like insert, update, delete will come to the master database, and only read operation will come to the slaves. 

Because applications are required to read more than write, then there are usually more masters than slaves.

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-5.png">

### Advantages of database replication:
<ul>
    <li>Better performance: all writes come to master nodes and all read comes to slaves, which can let us perform more queries in parallel</li>
    <li>Reliability: one database server goes down due to unexpected disaster, etc, then data is still preserved in other databases as well. So we do not need to worry about data loss</li>
    <li>High availability: One server goes down, users can access the other servers that are still available. </li>
</ul>

### How does the flow go when one database server goes down?
<ul>
    <li>if all slaves goes down, all reads come toward masters temporarily. If one slave goes down and there are still available slaves, then these slaves replace the failed slave.</li>
    <li>If all masters goes down, one slave will be promoted to be a master.</li>
</ul>

### How does a web server look like with a load balancer and master-slave databases?

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-6.png">

## Cache

A cache is a temporary storage that store frequently accessed data or expensive responses. This kind of data storage preventing our database servers from being fetched too repeatedly and frequently.

## Cache Tier

The cache tier is a temporary data store layer, making it much faster than a typical database. It improves system performance, reduce database workload and also scale the cache tier independently.

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-7.png">

When receiving a request, a web server checks if the cache has the response or not. If it does, resend that response to users. If it does not, then fetch it from database and then save it the cache for later use.

### Considerations when using cache

<ul>
    <li>If the same data is read frequently but modified infrequently. It should not be used as permanent database. Because when the cache server restarts, all the data will be wiped out </li>
    <li>Expiration Policy: This policy tells us when the data in the cache should be removed. Don't make the expiration data too short, because it will cause the system to reload too frequently. Also, don't make it too long, since the data will be tale</li>
    <li>Consistency: This problem happens when our database and the cache are not in sync. Because we perform writes on database, our cache might not be updated</li>
    <li>Mitigating Failures: with only cache, a single point of failure will arise (SPOF). more cache storage should be recommended</li>
    <li>Eviction Policy: This policy tells us what we should remove from our cache when it is full. Several eviction policies can be used like least-recently-used (LRU), least-frequently-used (LFU) or first-in-first-out (FIFO)</li>
</ul>

## Content delivery network (CDN)

## Stateless web tier

## Data centers

## Message queue

## Logging, metrics, automation 

## Database scaling

## Millions of users and beyond
