Mini E-Commerce Management System: A Node.js-Based Client-Server Application with API Gateway, Microservices, and JSON Database 
CMS3407 - Web and Network Services Assessment 2

The project uses Express.js for the API Gateway and three microservices, Node.js for TCP socket communication, and JSON files for data storage.

1. PROJECT STRUCTURE

A. Socket Client-Server
   Folder: Socket-Client-Server/
   Files: client.js, server.js, package.json

B. API Gateway
   Folder: Gateway-Microservices/api-gateway/
   Files: server.js, package.json, routes/gatewayRoutes.js

C. Product Service
   Folder: Gateway-Microservices/product-service/
   Files: server.js, package.json, data/products.json

D. Customer Service
   Folder: Gateway-Microservices/customer-service/
   Files: server.js, package.json, data/customers.json

E. Order Service
   Folder: Gateway-Microservices/order-service/
   Files: server.js, package.json, data/orders.json

F. Reset Script
   File: reset-data.js (in the main project folder)

The Gateway forwards requests to the correct service. Each service manages its own records. The socket client and server exchange TCP messages.

2. REQUIREMENTS

- Node.js (version 18 or later recommended)
- Visual Studio Code
- Extract the ZIP and open Mini-Ecommerce-System in VS Code.
- Open a terminal and check:

  node --version
  npm --version

3. INSTALL EXPRESS

Express is needed in the four HTTP application folders. From the main project folder, open a new terminal for each set of commands below.

Product Service:
  cd Gateway-Microservices/product-service
  npm.cmd install express

Customer Service:
  cd Gateway-Microservices/customer-service
  npm.cmd install express

Order Service:
  cd Gateway-Microservices/order-service
  npm.cmd install express

API Gateway:
  cd Gateway-Microservices/api-gateway
  npm.cmd install express

Install the packages once. The TCP socket application uses Node.js's built-in net module and does not need Express.

4. START THE HTTP APPLICATION

Open four separate VS Code terminals, each starting in the main project folder. Run these commands and keep all four terminals open.

Terminal 1 - Product Service (port 3001):
  cd Gateway-Microservices/product-service
  node server.js

Terminal 2 - Customer Service (port 3002):
  cd Gateway-Microservices/customer-service
  node server.js

Terminal 3 - Order Service (port 3003):
  cd Gateway-Microservices/order-service
  node server.js

Terminal 4 - API Gateway (port 3000):
  cd Gateway-Microservices/api-gateway
  node server.js

Start the three microservices before the Gateway.

5. CHECK THE API GATEWAY

Open these addresses in a web browser:

  http://127.0.0.1:3000/health
  http://127.0.0.1:3000/api/products
  http://127.0.0.1:3000/api/customers
  http://127.0.0.1:3000/api/orders

The health route should return "API Gateway is running". Use port 3000 for normal client requests. The Gateway passes requests to the backend services using an internal token. Direct backend access returns HTTP 403.

6. RUN THE TCP SOCKET CLIENT-SERVER

Open two more terminals, both starting in the main project folder.

Socket Terminal 1 - Server:
  cd Socket-Client-Server
  node server.js

Expected message: Socket server is running on port 4000

Socket Terminal 2 - Client:
  cd Socket-Client-Server
  node client.js

The client sends a message and displays the server's reply.

7. TEST THE HTTP METHODS

Keep the HTTP services running. Open a new Windows PowerShell terminal.
Run these example requests in order:

GET all products:
  curl.exe -i http://127.0.0.1:3000/api/products

GET one product:
  curl.exe -i http://127.0.0.1:3000/api/products/1001

POST a product:
  curl.exe --% -i -X POST http://localhost:3000/api/products -H "Content-Type: application/json" -d "{\"name\":\"USB-C Cable\",\"category\":\"Accessories\",\"price\":8.99,\"stock\":100}"

PUT the new product (ID 1004):
  curl.exe --% -i -X PUT http://localhost:3000/api/products/1004 -H "Content-Type: application/json" -d "{\"name\":\"USB-C Cable Pro\",\"category\":\"Accessories\",\"price\":12.99,\"stock\":80}"

DELETE the new product:
  curl.exe -i -X DELETE http://127.0.0.1:3000/api/products/1004

The POST must succeed before running the PUT and DELETE examples.
The separate CURL testing file contains 69 HTTP tests and 5 socket tests.
Follow its stated test order. The --% syntax is for Windows PowerShell.

8. RESET DATA AND STOP THE SERVERS

To restore the original JSON data, stop the HTTP services with Ctrl+C.
From the main project folder, run:

  node reset-data.js

Restart the services if you need to test again. To finish, press Ctrl+C
in each running server terminal.

9. COMMON PROBLEMS

- 'node' not recognised: Install Node.js and restart VS Code.
- Cannot find 'express': Run npm.cmd install express in that service folder.
- Port already in use: Stop the other running server with Ctrl+C.
- HTTP 502: Check whether the required backend service is running.
- HTTP 404: Check the requested URL.
- HTTP 403 on a backend port: Use the Gateway on port 3000 instead.

Keep the CURL test file with the project files and capture actual terminal
outputs when recording test evidence.
