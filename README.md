# sayantan_vt_13_07_2024
Assignment - URL Shortner

# Architecture Overview
Frontend: Interface for users to interact with the service.
Backend: Handles API requests, URL shortening, redirection, and data storage.
Database: Stores the URL mappings and associated metadata.
Caching Layer: For frequently accessed URLs to reduce database load.
Load Balancer: Ensures high availability and distributes incoming traffic.

# API Design

# 1. POST: Shorten Url

Endpoint: /api/shorten
# Request Body:
json
  {
      "long_url": "https://example.com/very/long/url"
  }
# Response:
json
  {
      "short_url": "http://localhost:8080/abc123",
      "id": 12345
  }
# Flow:
Validate the long URL.
Generate a unique short URL.
Insert the mapping into the database with an expiration date of 10 months from creation.
Return the short URL and id.
# 2. POST: Update Short URL

Endpoint: /api/update
# Request Body:
json
  {
      "short_url": "http://localhost:8080/abc123",
      "new_long_url": "https://newexample.com/new/long/url"
  }
# Response:
json
  {
      "success": true
  }  
# Flow:
Validate the short URL and new long URL.
Update the long URL in the database.
Return success status.
# 3. GET: Get Destination URL

Endpoint: /api/get
# Request Params: 
  short_url=abc123
# Response:
json
  {
      "long_url": "https://example.com/very/long/url"
  }
# Flow:
Validate the short URL.
Fetch the long URL from the database.
Return the long URL.
# 4. POST: Update Expiry

Endpoint: /api/update-expiry
# Request Body:
json
  {
      "short_url": "http://localhost:8080/abc123",
      "days_to_add": 30
  }
# Response:
json
  {
      "success": true
  }
# Flow:
Validate the short URL.
Update the expiration date in the database.
Return success status.
# 4. URL Shortening Algorithm
To ensure that shortened links are not predictable, we can use a hash function combined with a unique identifier. We can use a base62 encoding (which uses a-z, A-Z, and 0-9) to create short, unique strings.

# 5. High Availability and Caching
Database Replication: Set up PostgreSQL replication for high availability.
Load Balancer: Use a load balancer to distribute traffic across multiple instances of the service.
Caching: Use Redis or Memcached to cache frequently accessed URLs to reduce latency and database load.
