# Microservices

Monolithic applications are built as a single unit, with all the components tightly integrated. This makes them difficult to scale, maintain, and update. Microservices architecture addresses these issues by breaking down complex applications into smaller, independent services. These services communicate with each other via language-agnostic APIs, ensuring they can be built using different technologies. Each service is designed around specific business capabilities and is independently deployable, typically with automated deployment pipelines.

1. **Microservices architecture** involves breaking down complex applications into smaller, independent services.
2. **Communication** between these services is done via language-agnostic APIs, ensuring they can be built using different technologies.
3. Services are designed around specific **business capabilities**.
4. They are **independently deployable**, typically with automated deployment pipelines.
5. The **benefits** of this approach include better fault isolation, easier testing, and greater flexibility.
6. The **challenges** include managing the increased complexity and requiring advanced tools for monitoring and management.


## Components of Microservices

1. API Gateway
    * Acts as a single entry point for clients to access the microservices.
    * Handles routing, authentication, load balancing, and caching.
2. Service Registry
    * Keeps track of all the services in the system.
    * Allows services to discover and communicate with each other.
    * For example Consul, Eureka, Zookeeper.
3. Service Layer
    * Contains the microservices that provide specific business capabilities.
    * Communicate with each other via APIs.
    * Often built using REST, GraphQL, or gRPC.
4. Authorization Server
    * Handles authentication and authorization for the microservices.
    * Issues access tokens and refresh tokens.
    * For example OAuth2, OpenID Connect.
5. Data Storage
    * Each microservice can have its own database.
    * Can use different types of databases (SQL, NoSQL) based on requirements.
6. Distributed Caching
    * Improves performance by storing frequently accessed data in memory.
    * Reduces the load on the database.
    * For example Redis, Memcached.
7. Async Microservices Communication
    * Allows services to communicate asynchronously.
    * Can use message brokers like Kafka, RabbitMQ, or event-driven architectures.
8. Metrics Visualization
    * Monitors the performance and health of the microservices.
    * Provides insights into resource usage, response times, and error rates.
    * For example Prometheus, Grafana.
9. Log Aggregation and Visualization
    * Collects and aggregates logs from all the microservices.
    * Helps in debugging and troubleshooting issues.
    * For example ELK stack (Elasticsearch, Logstash, Kibana).

---

### Monolithic vs Microservices

- **Advantages of Microservices:**
    - Improved **scalability**: Individual services can be scaled independently based on demand.
    - Better **fault isolation**: Failures in one service don’t necessarily impact the entire system.
    - Flexibility in using **diverse technologies**: Teams can choose the best tech stack for each service.

- **Challenges of Microservices:**
    - **Increased complexity**: Managing multiple services requires more sophisticated tooling and expertise.
    - Requires advanced **monitoring, logging, and communication tools**: Essential for tracking the health and performance of a distributed system.
    - **Inter-service communication overhead**: Latency can be introduced due to network calls between services.
    - **Data consistency challenges**: Ensuring consistency across services with their own databases can be difficult.

- **Advantages of Monolithic Architecture:**
    - **Simpler** to develop and deploy: All components are integrated into a single codebase.
    - Suitable for **smaller teams** or applications: Easier to manage with fewer dependencies and less infrastructure overhead.

- **Challenges of Monolithic Architecture:**
    - Difficult to **scale**: Scaling a monolith often requires scaling the entire application, which can be resource-intensive.
    - **Maintenance and updates** become challenging: A change in one part of the system can impact the entire application, leading to slower release cycles.
    - **Slower innovation**: Adding new features or adopting new technologies can be difficult without significant refactoring.

- **Conclusion:**
    - For **startups** or smaller projects, a monolithic architecture might be more practical due to its simplicity and lower operational overhead. As the application grows, however, the monolithic approach can lead to bottlenecks in scaling and deployment.
    - For **larger, complex applications**, microservices can offer greater agility and resilience. Companies like Netflix and Amazon have successfully adopted microservices to scale and innovate rapidly. However, the trade-off is the need for a mature DevOps* culture and the ability to handle the added complexity.

_*DevOps is a collaborative approach that combines development and operations teams to automate and streamline the software development lifecycle, enabling continuous integration, delivery, and deployment._

## API Gateway

- **API Gateway** is a server that acts as an API front-end, receiving API requests, enforcing throttling and security policies, passing requests to the back-end service, and then passing the response back to the requester.
- **Benefits** of using an API Gateway:
    - **Security**: Centralized authentication and authorization.
    - **Rate limiting**: Prevents abuse and ensures fair usage.
    - **Load balancing**: Distributes traffic across multiple servers.
    - **Caching**: Stores responses to reduce latency.
    - **Logging and monitoring**: Provides insights into API usage and performance.
    - **Transformation**: Translates requests and responses to match the back-end service.
- **Examples** of API Gateways:
    - **NGINX**: Open-source web server and reverse proxy.
    - **Apache**: Open-source web server with modules for proxying and load balancing.
    - **Kong**: Open-source API gateway built on NGINX.
    - **Apigee**: Google Cloud's API management platform.
    - **AWS API Gateway**: Amazon's managed API gateway service.
    - **Azure API Management**: Microsoft's API gateway and management service.
    - **Kubernetes Ingress**: Manages external access to services in a Kubernetes cluster.
    - **Google Cloud Endpoints**: Google Cloud's API gateway service.

### Apache reverse proxy

Reverse proxy is a server that sits between clients and backend servers. It forwards client requests to the appropriate backend server and returns the response to the client.

Example configuration for Apache reverse proxy:

```apache
<VirtualHost *:80>
    ServerName example.com
    ProxyPass /api http://apidomain.com/api
    ProxyPassReverse /api http://apidomain.com/api
</VirtualHost>
```

In this configuration:
- `ServerName` specifies the domain name of the server.
- `ProxyPass` forwards requests from `/api` to `http://apidomain.com/api`.
- `ProxyPassReverse` rewrites the response headers to match the client's request.
- Requests to `http://example.com/api` will be proxied to the `http://apidomain.com/api` backend server.
- The backend server's response will be returned to the client.

### Nginx reverse proxy

Nginx is alternative to Apache for reverse proxying and load balancing. Main difference is that Nginx is event-driven and asynchronous, while Apache is process-based. Event driven means that Nginx can handle multiple connections simultaneously without creating a new process for each connection. This makes Nginx more efficient in handling high loads and concurrent connections whereas Apache creates a new process for each connection.

Example configuration for Nginx reverse proxy:

```nginx
server {
    listen 80;
    server_name example.com;

    location /api {
        proxy_pass http://apidomain.com/api;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

In this configuration:
- `listen 80` specifies that Nginx listens on port `80`.
- `server_name` specifies the domain name of the server.
- `proxy_pass` forwards the request to the backend server.
- `proxy_set_header` sets the headers to pass additional information to the backend server.
    - `Host` header specifies the host name of the client.
    - `X-Real-IP` header specifies the IP address of the client.
    - `X-Forwarded-For` header specifies the original client IP address.
    - `X-Forwarded-Proto` header specifies the protocol used by the client.
    - These headers are useful for the backend server to identify the client's information.
- Requests to `http://example.com/api` will be proxied to the `http://apidomain.com/api` backend server.
- The backend server's response will be returned to the client.

## Assignment 1: API Gateway with Apache
Create an API gateway using Apache, which will route requests to three different APIs: Two APIs can be any public REST APIs available on the internet or private APIs that you have created and are available on server/cloud. Use OpenWeatherMap as the third API since it requires an API key.

1. Order a new Virtual Machine from https://ecloud.metropolia.fi/.
    - RockyLinux 9 LAMP template.
2. Log in to the VM via SSH.
3. Open the Apache configuration file and Define the API Gateway Configuration.
    - `sudo nano /etc/httpd/conf.d/api-gateway.conf`
    - example:
      ```apache
      <VirtualHost *:80>
          ServerName get-this-from-email
          # SSL Proxy Support
          SSLProxyEngine On
         
          # API no 1
          ProxyPass /api1 http://api1.example.com
          ProxyPassReverse /api1 http://api1.example.com
             
          # API no 2
          ProxyPass /api2 http://api2.example.com
          ProxyPassReverse /api2 http://api2.example.com
      
          # Adding api key to a request
          RewriteEngine On
          RewriteCond %{QUERY_STRING} !(^|&)appid=(&|$)
          RewriteRule "^/weather" "https://api.openweathermap.org/data/2.5/weather?%{QUERY_STRING}&appid=7c42463f9762df30e2f0f703a1115cd8" [P]
          ProxyPassReverse "/weather" "https://api.openweathermap.org/2.5/weather"
      </VirtualHost>
      ```
4. Test Apache configuration.
    - `sudo httpd -t`
    - `sudo apachectl configtest`
5. Restart Apache.
    - `sudo systemctl restart httpd`
6. Test the API Gateway with Postman or a web browser.
    - `http://your-server-ip/api1`
    - `http://your-server-ip/api2`
    - `http://your-server-ip/weather?q=Somero`

## Assignment 2: API Gateway with Nginx

Create the same API Gateway using Nginx instead of Apache. Follow the same steps as in the Apache assignment, but use Nginx configuration files instead.

1. Order a new Virtual Machine from https://ecloud.metropolia.fi/.
    - RockyLinux 9 template (NOT LAMP).
2. Log in to the VM via SSH.
    - [Install Nginx.](https://docs.rockylinux.org/guides/web/nginx-mainline/)
    - Follow the instructions until you get the welcome page.
3. Open the Nginx configuration file and Define the API Gateway Configuration.
    - `sudo nano /etc/nginx/conf.d/api-gateway.conf`
    - example:
      ```nginx
      server {
          listen 80;
          server_name get-this-from-email;
          location /api1 {
              proxy_pass http://api1.example.com;
              # set headers
          }
          
          # API no 2 the same way
      
          location /weather {
              rewrite ^/weather(.*)$ /weather$1?appid=7c42463f9762df30e2f0f703a1115cd8 break;
              proxy_pass https://api.openweathermap.org/data/2.5;
             # set headers
          }
      }
      ```
4. Test Nginx configuration.
    - `sudo nginx -t`
5. Restart Nginx.
    - `sudo systemctl restart nginx`
6. Test the API Gateway with Postman or a web browser.
    - `http://your-server-ip/api1`
    - `http://your-server-ip/api2`
    - `http://your-server-ip/weather?q=Somero`

## Assignment 3: API Gateway with Node.js

Create the same API Gateway using Node.js. Follow the same steps as in the Apache and Nginx assignments, but use Node.js and Express to create the API Gateway.

You can use [this repo](https://github.com/ilkkamtk/ts_mongo_starter) as a starting point. You can delete the MongoDB part and `api` folder.

1. [http-proxy-middleware](https://www.npmjs.com/package/http-proxy-middleware)

    - The `http-proxy-middleware` library is used to create a proxy server that forwards requests from your application to another server. It provides features such as request forwarding, path rewriting, header manipulation, and HTTPS support.

2. Install http-proxy-middleware and import necessary modules.

    ```ts
    import 'dotenv/config';
    import express, { NextFunction, Request, Response } from 'express';
    import cors from 'cors';
    import helmet from 'helmet';
    import morgan from 'morgan';
    import { createProxyMiddleware } from 'http-proxy-middleware';
    import {ClientRequest} from 'http';
    ```

3. Initialize the Express Application

    ```ts
    const app = express();
    ```

4. Set Up Middleware

    ```ts
    app.use(cors());
    app.use(helmet());
    app.use(morgan('combined'));
    app.disable('x-powered-by');
    ```

5. Define Routes and Microservices

    ```ts
    const services = [
      {
        route: '/api1',
        target: 'https://dummy.example.com/api1',
      },
      {
        route: '/api2',
        target: 'https://dummy.example.com/api2',
      },
      {
        route: '/api3',
        target: 'https://dummy.example.com/api3',
         // Add additional proxy options for this service
         on: {
           onProxyReq: (proxyReq: ClientRequest) => {
           // Append apikey query parameter to the target URL
           proxyReq.path += '&appid=your_api_key';
          },
        },
      },
    ];
    ```

6. Set Up Proxy Middleware for Each Microservice

    ```ts
    services.forEach(({ route, target, on }) => {
      const proxyOptions = {
        on,
        target,
        changeOrigin: true,
        pathRewrite: {
          [`^${route}`]: '',
        },
        secure: process.env.NODE_ENV === 'production', // Enable SSL verification in production
      };
    
      console.log(proxyOptions);
      app.use(route, createProxyMiddleware(proxyOptions));
    });
    ```

    - For each microservice, sets up proxy middleware using `createProxyMiddleware`.
    - `target`: The dummy URL to which requests should be proxied.
    - `changeOrigin`: Ensures the `Host` header matches the target URL.
    - `pathRewrite`: Rewrites the URL path by removing the base route (`route`) before forwarding.
    - `secure`: Enables SSL verification based on the environment.

7. Define and Start the Server

    ```ts
    const PORT = process.env.PORT || 3008;
    
    app.listen(PORT, () => {
      console.log(`Gateway is running on port ${PORT}`);
    });
    ```
8. Run the Node.js application and test the API Gateway with Postman or a web browser.
