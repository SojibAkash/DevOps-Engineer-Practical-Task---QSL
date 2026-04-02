# DevOps-Engineer-Practical-Task---QSL
Some task for devops engineer interview 

1. Build a Simple API

Simple API

## Endpoints
- GET /status → returns OK
- POST /data → sends back your data

## Run
npm install express
node server.js

## How it handles ~100 req/sec

- Node.js handles many requests at the same time
- Can run multiple instances
- Use a load balancer 

So it can easily handle 100 requests per second.


2. Containerize the Application
Docker Setup

## Build image
docker build -t simple-api .

## Run container
docker run -d -p 3000:3000 -e PORT=3000 simple-api

## Environment Variable
- PORT → application port





3.Reverse Proxy Setup

Nginx Configuration:

http {
    upstream app_servers {
        server app1:3000;
        server app2:3000;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://app_servers;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }
}


4. CI/CD Pipeline

Yaml file

name: CI/CD Pipeline

on:
  push:
    branches: ["main"]

jobs:
  build-test-deploy:
    runs-on: ubuntu-latest


    - name: Checkout code
      uses: actions/checkout@v2

    - name: Setup Node
      uses: actions/setup-node@v3
      with:
        node-version: '17'

    - name: Install dependencies
      run: npm install

    - name: Run test
      run: echo "No tests yet, build OK"

   
    - name: Build Docker image
      run: docker build -t simple-api .

    - name: Deploy container
      run: |
        docker stop simple-api || true
        docker rm simple-api || true
        docker run -d -p 3000:3000 --name simple-api simple-api

5. Basic Monitoring & Logs

   import logging
from fastapi import Request
import time

logger = logging.getLogger("uvicorn.access")

@app.middleware("http")
async def log_requests(request: Request, call_next):
    start_time = time.time()
    response = await call_next(request)
    duration = time.time() - start_time
    logger.info(f"{request.method} {request.url} completed in {duration:.4f}s")
    return response

    


Bonus (Optional)


Choose AWS EC2, GCP Compute Engine.
Install Docker and Docker Compose.
Pull images and run with docker-compose.
deploy on Kubernetes for scaling.


