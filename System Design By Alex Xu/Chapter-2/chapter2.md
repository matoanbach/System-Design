# CHAPTER 2: BACK-OF-THE-ENVELOPE ESTIMATION

You might be asked to estimate your system capacity or performance requirements.

To have a good sense of scalability, you should well understand: power of two, latency numbers and availability numbers.

## Power of two

| Power | Approximate value | Full Name  | Short Name |
| :---: | :---------------: | :--------: | :--------: |
|  10   |    1 Thousand     | 1 Kilobyte |    1 KB    |
|  20   |     1 Million     | 1 Megabyte |    1 MB    |
|  30   |     1 Billion     | 1 Gigabyte |    1 GB    |
|  40   |    1 Trillion     | 1 Terabyte |    1 TB    |
|  50   |   1 Quadrillion   | 1 Petabyte |    1 PB    |

## Latency numbers every programmer should know

|                  Operation name                  |          Time           |
| :----------------------------------------------: | :---------------------: |
|                L1 cache reference                |         0.5 ns          |
|                Branch mispredict                 |          5 ns           |
|                L2 cache reference                |          7 ns           |
|                Mutex lock/unlock                 |         100 ns          |
|              Main memory reference               |         100 ns          |
|           Compress 1K bytes with Zippy           |    10,000 ns = 10 us    |
|        Send 2K bytes over 1 Gbps network         |    20,000 ns = 20 us    |
|        Read 1MB sequentially from memory         |   250,000 ns = 250 us   |
|      Round trip within the same datacenter       |   500,000 ns = 500 us   |
|                    Disk seek                     |  10,000,000 ns = 10 ms  |
|     Read 1 MB sequentially from the network      |  10,000,000 ns = 10 ms  |
|         Read 1 MB sequentially from disk         |  30,000,000 ns = 30 ms  |
| Send packet CA (California) -> Netherlands -> CA | 150,000,000 ns = 150 ms |

### Below is a better visualization

<img src="https://github.com/matoanbach/System-Design-Notes/blob/main/System%20Design%20By%20Alex%20Xu/Chapter-2/pics/figure2-1.png">

### Conclusions:

<ul>
    <li>Memory is fast, disk is slow</li>
    <li>Avoid disk seeks if possible</li>
    <li>Simple compression algorithms are fast</li>
    <li>Compress data before sending if possible</li>
    <li>It takes time to send data between different regions</li>
</ul>

## Availability numbers

High availability measures the time our system is continuous operational. It is usually measured by percentage.

A service level agreement (SLA) is agreement between service providers and your customer. Uptime is traditionally measured in nines. The more nines, the better.

| Availability % |  Downtime per day   | Downtime per year |
| :------------: | :-----------------: | :---------------: |
|      99%       |    14.40 minutes    |     3.65 days     |
|     99.9%      |     1.4 minutes     |    8.77 hours     |
|     99.99%     |    8.64 minutes     |   52.60 minutes   |
|    99.999%     | 864.00 milliseconds |   5.36 minutes    |
|    99.9999%    | 86.40 milliseconds  |   31.56 minutes   |

Example: Estimate Twitter QPS and storage requirements
Assumptions:

<ul>
    <li>300 million monthly active users</li>
    <li>50% of users use Twitter daily</li>
    <li>Users posts 2 tweets per day on average</li>
    <li>10% of tweets contain media</li>
    <li>Data is stored for 5 years</li>
</ul>
Estimations:
Query per second (QPS) estimate:
<ul>
    <li>Daily active users (DAU) = 300 million * 50% = 150 million</li>
    <li>Tweets QPS = 150 million * 2 tweets / 24 hour / 3600 seconds = 3500</li>
    <li>Peek QPS = 2 * QPS = ~ 7000</li>
</ul>
We will only estimate media storage here

<ul>
    <li>Average tweet size</li>
    <ul>
        <li>tweet_id  64 bytes</li>
        <li>text      140 bytes</li>
        <li>media      1MB</li>
    </ul>
    <li>Media storage: 150 million * 2 * 10% * 1 MB = 30 TB per day</li>
    <li>5-year media storage: 30 TB * 365 * 5 = ~55 PB</li>
</ul>

Tips:
<ul>
    <li>Rounding and Approximation</li>
    <li>Write down your assumptions</li>
    <li>Label your units</li>
    <li>Commonly asked: QPS, peak QPS, storage, cache, number of servers</li>
</ul>

