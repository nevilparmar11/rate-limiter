# Rate limiter strategies 🔥

## How to try it

Clone it.

Run `npm install`.

To start different servers:

1. Fixed Window Counter -> `npm run start-fixed-window-counter`
2. Token Bucket -> `npm run start-token-bucket`
3. Sliding Window Log -> `npm run start-sliding-window-log`
4. Sliding Window Counter -> `npm run start-sliding-window-counter`

# What is a Rate Limiter?

A rate limiter is a tool that monitors the number of requests per unit of time that a client IP can send to an API endpoint. If the number of requests exceeds a certain threshold, the rate limiter will block the client IP from sending further requests for a certain period of time.

# Key Concepts

- **Limit:** The maximum number of requests that a client IP can send to an API endpoint per unit of time.
- **Window:** The unit of time that the rate limiter uses to track the number of requests sent by a client IP. It can be anything from seconds to days.
- **Identifier:** A unique attribute of a request that the rate limiter uses to identify the client IP.

# Designing a Rate Limiter

- Define the purpose of your rate limiter.
- Decide on the type of rate limiting e.g. fixed window, sliding window, etc.
- Type of Storage: In-memory or persistent storage.
- Request identifier: IP address, User ID, API key, etc.
- Counting and logging
- Code checks if the request is within the limit or not.
- Send a response to the client.

# Silently dropping a request

This can be better than returning 429 errors to the client. It's a trick to fool attackers into thinking that the request has been accepted even when the rate limiter has actually dropped the request.

# Rate Limiting Strategies

## Fixed Window Counter

- **Mechanism**: The time frame (window) is fixed, like an hour or a day. The counter resets at the end of each window.
- **Use Case**: Simple scenarios where average rate limiting over time is sufficient.
- **Pros**: Easy to implement and understand.
- **Cons**: Can allow bursts of traffic at the window boundaries.

## Sliding Window Log

- **Mechanism**: Records the timestamp of each request in a log. The window slides with each request, checking if the number of requests in the preceding window exceeds the limit.
- **Use Case**: When a smoother rate limit is needed without allowing bursts at fixed intervals.
- **Pros**: Fairer and more consistent than fixed windows.
- **Cons**: More memory-intensive due to the need to store individual timestamps.

## Token Bucket

- **Mechanism**: A bucket is filled with tokens at a steady rate. Each request costs a token. If the bucket is empty, the request is either rejected or queued.
- **Use Case**: Useful for APIs or services where occasional bursts are acceptable.
- **Pros**: Allows for short bursts of traffic over the limit.
- **Cons**: Slightly complex to implement. Can be unfair over short time scales.

## Leaky Bucket

- **Mechanism**: Requests are added to a queue and processed at a steady rate. If the queue is full, new requests are discarded.
- **Use Case**: Ideal for smoothing out traffic and ensuring a consistent output rate.
- **Pros**: Ensures a very even rate of requests.
- **Cons**: Does not handle bursts well. Excess requests are simply dropped.

## Sliding Window Counter

- **Mechanism**: Similar to fixed window but smoother. The window slides with each request, combining the simplicity of fixed windows with some advantages of sliding log.
- **Use Case**: When you need a balance between fairness and memory efficiency.
- **Pros**: More consistent than a fixed window, less memory usage than sliding log.
- **Cons**: Can be more complex to implement than fixed window.

## Rate Limiting with Queues

- **Mechanism**: Instead of rejecting excess requests, they are queued and processed when possible.
- **Use Case**: When losing requests is not acceptable, and delays are tolerable.
- **Pros**: No request is outright rejected.
- **Cons**: This can lead to high latency and resource consumption if the queue becomes too long.
