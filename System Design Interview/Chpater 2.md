# Chapter 2 - Back of the envelope estimations

**Back of the envelope calculations**  are rough calculations that can be done quickly using combination of thought experiments and common performance numbers with whatever you have on hand. They are useful for getting a ballpark estimate of a quantity of a good feel of which design will meet your requirements, and are often used to answer order-of-magnitude questions. For example, you might use back of the envelope calculations to estimate the number of people in a crowd, the size of a market, or the growth rate of a population.

To have a good sense of scalability basics we should be aware of three numbers:

- Power of two
- Latency numbers
- Availability numbers

## Power of two

The power of two is a mathematical constant equal to 2^10 = 1024. It is frequently used in computer science. For example, 1 KiB = 1024 bytes, 1 MiB = 1024 KiB, and so on.

| Power of two | Approximate Value | Name |
|--------------|-------|------|
| 2^10 | 1 Thousand | kb |
| 2^20 | 1 Million | mb |
| 2^30 | 1 Billion | gb |
| 2^40 | 1 Trillion | tb |
| 2^50 | 1 Quadrillion | pb |

## Latency numbers

Latency is the time it takes for a request to travel from the client to the server and back. Latency is often measured in milliseconds (ms), so 1 ms = 0.001 seconds. Latency numbers are important because they help us understand how long it takes to send a request to the server and get a response back. For example, if you are building a web application, you want to make sure that your web pages load quickly.

| Operation name | Time |
|--------------|-------|
| L1 cache reference | 0.5 ns |
| Branch mispredict | 5 ns |
| L2 cache reference | 7 ns |
| Mutex lock/unlock | 100 ns |
| Main memory reference | 100 ns |
| Compress 1K bytes with Zippy | 10,000 ns |
| Send 2K bytes over 1 Gbps network | 20,000 ns |
| Read 1 MB sequentially from memory | 250,000 ns |
| Round trip within same datacenter | 500,000 ns |
| Disk seek | 10,000,000 ns |
| Read 1 MB sequentially from network | 10,000,000 ns |
| Read 1 MB sequentially from disk | 30,000,000 ns |
| Send packet CA->Netherlands->CA | 150,000,000 ns |

> The seek time is the time it takes a specific part of a hardware's mechanics to locate a particular piece of information on a storage device. This value is typically expressed in milliseconds (ms), where a smaller value indicates a faster seek time.

From this table, we get the following insights:

- Memory is faster than disk.
- Avoid disk seeks whenever possible.
- Simple compression algorithms are fast.
- Compress data before sending it over the network if possible.
- Data centers are usually in different locations. It takes time to send data between them.

## Availability numbers

Availability is the probability that a system will be up and running at a given point in time. Availability is often measured in nines. For example, 99.9% availability means that the system will be up and running 99.9% of the time. This translates to 8.77 hours of downtime per year. Availability numbers are important because they help us understand how reliable a system is. For example, if you are building a web application, you want to make sure that your web pages are available to your users.

| Availability | Downtime per day | Downtime per year |
|--------------|-------|------|
| 99% | 14m 24s | 3d 15h 40m |
| 99.9% | 1m 26s | 8h 45m |
| 99.99% | 8s | 52m 35s |
| 99.999% | 0.8s | 5m 15s |
| 99.9999% | 0.08s | 31s |

## Example : Estimate Twitter queries per seconds (QPS) and storage requirements

> Please note the following numbers are for this exercise only as they are not real numbers from Twitter.

**Assumptions:**

- 1 billion monthly active users
- 50% of users are active daily
- Users post 5 tweets per day on average
- 20% of tweets contain media (images, videos, etc.), in average 1 MB per media
- Data is stored for 5 years

**Estimations:**

Queries per second (QPS) = (1 billion * 50% * 5 tweets per day) / (24 hours * 3600 seconds) = 28935 QPS

Peak QPS = 2 * QPS = 57870 QPS

Media storage per day = (1 billion * 50% * 5 tweets per day * 20% * 1 MB) = 500 TB

5 years storage = 500 TB * 365 days * 5 years = 91.25 PB

## Tips for your next interview

- **Rounding and approximations** are your friends. what is the result of 99987 / 9.1 ? simply round it to 10000 / 10 = 1000
- **Write down your assumptions** to be referenced later.
- **Label your units**. What is "5", is it 5 kb, 5 mb, 5 gb?
- **Commonly asked back-of-the-envelope estimations**: QPS, peak QPS, storage, cache, number of servers, etc. You can practice these calculations when preparing for an interview. Practice makes perfect.
