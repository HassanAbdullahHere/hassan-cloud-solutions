

# APIs with Lambda + API Gateway

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-api)

**Author:** Hassan Abdullah  
**Email:** hassanabdullahhere01@gmail.com

---

![Project Banner](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-compute-api_c9d0e1f2)

---

## Overview

In this project, I build a fully serverless API by connecting an AWS Lambda function to Amazon API Gateway. The Lambda function retrieves data from a DynamoDB table, and API Gateway exposes it as a public HTTP endpoint — no servers to manage.

### Tools & Services

| Service | Purpose |
|---------|---------|
| AWS Lambda | Run backend logic without managing servers |
| Amazon API Gateway | Create, deploy, and secure the HTTP API endpoint |
| Amazon DynamoDB | Data source queried by the Lambda function |

### Key Concepts

- Serverless compute and event-driven architecture
- API resources and HTTP methods (GET, POST, etc.)
- API Gateway stages and deployment snapshots
- API documentation and versioning

### Project Reflection

**Duration:** ~60 minutes

I chose this project to understand how serverless APIs are built on AWS — specifically how Lambda and API Gateway work together to replace a traditional server-based backend.

---

## Lambda Functions

**AWS Lambda** is a serverless compute service that runs your code in response to events — no need to provision, configure, or manage servers. You pay only for the compute time consumed while your function runs.

For this project, the Lambda function:

1. Receives a trigger from API Gateway when an HTTP request arrives.
2. Connects to a DynamoDB table.
3. Retrieves and returns the requested data as a JSON response.

Lambda manages the underlying infrastructure automatically — scaling up or down based on incoming request volume.

![Lambda Function Setup](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-compute-api_a1b2c3d5)

---

## API Gateway

An **API (Application Programming Interface)** is a contract that lets different software systems communicate. One system makes a request; the API defines how that request should be structured and what response to expect.

**Amazon API Gateway** is a fully managed service for building, publishing, and securing APIs at any scale. It acts as the "front door" to backend services like Lambda:

- Receives incoming HTTP requests from clients.
- Routes them to the correct Lambda function.
- Returns the Lambda response back to the caller.
- Enforces authentication, throttling, and logging.

This separation means Lambda only ever sees clean, validated requests — API Gateway handles all the edge concerns.

![API Gateway Overview](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-compute-api_m3n4o5p6)

---

## API Resources and Methods

**API resources** are the URL paths that make up your API structure (e.g. `/items`, `/users/profile`). Each resource represents a logical unit of functionality.

**API methods** define what action can be performed on a resource. They map directly to standard HTTP verbs:

| Method | Purpose |
|--------|---------|
| GET | Read / retrieve data |
| POST | Create new data |
| PUT | Update existing data |
| DELETE | Remove data |

For this project, I created a **GET** method on my resource and connected it to the Lambda function — so any HTTP GET request to that endpoint triggers the function and returns data from DynamoDB.

![API Resource and GET Method](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-compute-api_c9d0e1f2)

---

## API Deployment

Building an API in API Gateway does not make it publicly accessible until it is **deployed** to a stage.

**Stages** are named snapshots of your API at a point in time (e.g. `dev`, `staging`, `prod`). They allow you to:

- Test changes in a lower environment before promoting to production.
- Roll back to a previous deployment if something breaks.
- Give different consumers access to different versions of the API.

After deployment, API Gateway generates an **Invoke URL** — the public endpoint clients use to call the API. I tested mine directly in a browser to confirm it returned data from DynamoDB.

![API Deployed with Invoke URL](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-compute-api_3ethryj2)

---

## EXTRA: API Documentation

Good API documentation is essential — it tells other developers exactly how to call each endpoint, what parameters are required, and what responses to expect.

Steps taken:

1. Added documentation parts to the API Gateway resource and GET method in the console.
2. Published the documentation as version `1` to the `prod` stage.
3. Downloaded the exported documentation to verify the structure.

The exported file showed all documented resources and methods, formatted for distribution to API consumers.

**Why it matters:** Without documentation, even a perfectly built API is hard to adopt. Publishing versioned docs alongside each deployment keeps consumers in sync with changes.

![Published API Documentation](http://learn.nextwork.org/loving_gold_zesty_sage/uploads/aws-compute-api_z9a0b1c2)

---

## Architecture Summary

```
Client (browser / app)
        │
        │  HTTP GET request
        ▼
  Amazon API Gateway
  (route, auth, throttle)
        │
        │  invoke
        ▼
   AWS Lambda function
  (retrieve data logic)
        │
        │  query
        ▼
  Amazon DynamoDB
  (data source)
        │
        └──► response flows back up the chain to the client
```

---

## Lessons Learned

- **Serverless ≠ no infrastructure** — it means AWS manages the infrastructure. You still design the logic and security.
- **API Gateway decouples the client from the backend.** Lambda can be swapped, updated, or versioned without the client knowing.
- **Stages enable safe deployments.** Always deploy to a non-production stage first and promote only after verification.
- **Documentation is part of the API contract.** Treat it as a deliverable, not an afterthought.
