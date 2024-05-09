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

Because applications are required to read more than write, then there are usually more slaves than masters.

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

A CDN is used to deliver static content like HTML, music, images, videos, CSS, javascript, etc.

It can also deliver Dynamic content.

When a user make a request for a static content, the closest CDN will be responsible for sending that content. The further the users are, the longer it will take to deliver that content.

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-9.png">

### How the flow go for CDN?

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-10.png">

<ol>
    <li>User A request an image with an URL.</li>
    <li>If the image is not in the CDN yet, CDN will fetch that image from the original storage like Amazon S3</li>
    <li>The image will be stored in the CDN afterward with Time-to-live policy that tells us when we can remove that content from the CDN cache. </li>
    <li>The image is returned back to User A. And the image will live until the TTL (time-to-live) policy expires.</li>
    <li>User B sends a request of the same image to the CDN. If that image is still available in the CDN, return it back to User B</li>
</ol>

### Considerations of using a CDN

Cost: data coming in and out of the CDN will be chared by whoever is its host. You might want to remove data that is not frequently used from the CDN to save the costs

Cache expiry: If the cache expiry time is too short, the CDN needs to refetch data from the original database more frequently, rising the costs. If the expiry time is too long, the data could not be fresh anymore.

CDN fallback: It is recommended to know how to deal with the situation of CDN outage.

Invalidating files: Sometimes, we need to remove some content before it expires.

### The design after adding CDN and cache

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-11.png">

## Stateless web tier

Stateless method allows our applications to scale horizontally.

### Stateful architecture

Lets say we have multiple stateful servers. So when a user authenticating session is created in one server, but the session can't be used in another server. That means the user must authenticate themselves if they request another server.

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-12.png">

### Stateless architecture

A token session is created by one server can be reused by another server as long as the token is still valid. This method is referred as stateless architecture where users do not need to create a new session repeatedly for each server.

State data is stored in a shared data store and kept of web servers. It is simpler, more robust, and scalable.

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-13.png">

### The design after adding a stateless web tier

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-14.png">

## Data centers

The figure below is an example of two data centers, preventing unexpected issues. In most cases, user requests are routed based on their geographical location.

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-15.png">

### An example when an database center outage occurs

The given example below show what happens when there is one server is failed.
<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-16.png">

However, it goes along with several technical challenges.

<ul>
    <li>Traffic redirection: traffic has to be redirected to the closest data center.</li>
    <li>Data synchronization: a healthy server needs to be in sync with the server that is failed</li>
    <li>Test and deployment: testing and deploying multi-data center setup is not easy.</li>
</ul>

## Message queue
A message queue is durable, enabling asynchronous communication.

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-17.png">

Basic architecture:
<ul>
    <li>A producer creates a message and push it into the queue</li>
    <li>The consumer subscribe and then process it</li>
</ul>

Message queue allows decoupling between producer and consumer. This means that when consumer is failed, producer can still push messages into the queue and then the consumer can process it later.

An example of message queue - photo processing:

- Web servers publish photo processing jobs to the message queue.
- Photo processing workers pick up the jobs the message queue and process it 
- Number of workers can be scaled up and down based on how busy the message queue is.

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-18.png">

## Logging, metrics, automation
These are good practices to maintain and debug if we have a large-scale system.

- Logging: errors can be stored and then later searched and viewed by our service operators
- Metrics: collecting different types of metrics help us to gain business insights and understand the health of our system better.
- Automation: development productivity can be improved if we invest in continuous integration automation tools such as automated build, deploy, test, etc. 

### Our updated system design

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-19.png">

## Database scaling
### Vertical scaling
Vertical scaling is when when add more power (CPU, RAM, DISK, etc.) to an existing machine.
There are drawbacks:
- We can add more CPU, RAM, etc. But there is always a limit for hardware
- Single point of failures
- The overall cost is high


### Horizontal scaling
Horizontal scaling is also known as sharding technique, which is just simply to add more machines.

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-20.png">

Sharing technique divides data tables into many tables and stores them into different sharding database. Each database will have a different sharding key, which will serves different users.

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-21.png">

Below shows how the user table inside each sharded database look like.

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-22.png">

There are challenges:
- Resharding data: resharding is needed when one single shard is full. Some shards can be filled or exhausted more quickly due to uneven data distribution. This problem can be solved with consistent hashing
- Celebrity problem (aka hotspot): one shard could potentially hold more celebrities' information like Katy Pety, Justin Bieber and Lady Gaga, causing overload. This can be solved by distributing each celebrity among shards equally.
- Join and de-normalization: Once a database has been sharded, it is challenging to rejoin the tables. 

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-1/pics/figure1-23.png">

## Millions of users and beyond

Scaling is iterative not once-in-a-life-time event. 
Key points of this chapter
<ul>
    <li>Keep the web tier stateless</li>
    <li>Build redundancy at every tier</li>
    <li>Cache data as much as you can</li>
    <li>Support multiple data centers</li>
    <li>Host static assess in CDN</li>
    <li>Scale your data tier by sharding</li>
    <li>Split tiers into individual services</li>
    <li>Monitor your system and use automation tools</li>
</ul>