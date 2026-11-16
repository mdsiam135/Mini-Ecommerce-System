# Mini-Ecommerce-System

Mini E-Commerce Management System
CMS3407 - Web and Network Services Assessment 2
This project uses Express.js for the API Gateway and the three microservices. It uses Node.js `net` for the Socket Client-Server application and JSON files as databases.
1. Project structure

Mini-Ecommerce-System/
|
|-- Socket-Client-Server/
|   |-- client.js
|   |-- server.js
|   |-- package.json
|
|-- Gateway-Microservices/
|   |-- api-gateway/
|   |   |-- server.js
|   |   |-- package.json
|   |   |-- routes/
|   |       |-- gatewayRoutes.js
|   |
|   |-- product-service/
|   |   |-- server.js
|   |   |-- package.json
|   |   |-- data/products.json
|   |
|   |-- customer-service/
|   |   |-- server.js
|   |   |-- package.json
|   |   |-- data/customers.json
|   |
|   |-- order-service/
|       |-- server.js
|       |-- package.json
|       |-- data/orders.json
|
|-- reset-data.js

The README and CURL test file are supplied separately. Put them in the project root if you want them visible beside the code.

2. What each part does
Socket Client: connects to the Socket Server and sends a message.
Socket Server: receives the message and sends a reply.
API Gateway: receives client HTTP requests and forwards them to the correct microservice.
Product Service: manages products.
Customer Service: manages customers.
Order Service: manages orders and calculates each order's total price.
JSON files: store the records for each service.
`reset-data.js`: restores the original sample records after testing.

3. Requirements
Install Node.js (version 18 or later recommended).
Install Visual Studio Code.
Extract the code ZIP.
Open the extracted `Mini-Ecommerce-System` folder in VS Code.
Open Terminal > New Terminal.
Check Node.js:
powershell
node --version

Check npm:
powershell
npm --version

4. Install Express
Express is used by the API Gateway and all three microservices.
Run `npm install` once inside each of these four folders.
Terminal 1: Product Service
Terminal
cd Gateway-Microservices/product-service
npm.cmd init -y
npm.cmd install express

Terminal 2: Customer Service
powershell
cd Gateway-Microservices/customer-service
npm.cmd init -y
npm.cmd install express

Terminal 3: Order Service
powershell
cd Gateway-Microservices/order-service
npm.cmd init -y
npm.cmd install express

Terminal 4: API Gateway
powershell
cd Gateway-Microservices/api-gateway
npm.cmd init -y
npm.cmd install express

You only need to install the packages once, unless you delete the `node_modules` folders.
The Socket Client-Server uses Node.js's built-in `net` module, so it does not need Express or an additional package.

5. Run the Socket Client-Server
Open two VS Code terminals.
Terminal 1: Start the Socket Server
powershell
cd Socket-Client-Server
node server.js

Expected message:

Socket server is running on port 4000

Terminal 2: Start the Socket Client
powershell
cd Socket-Client-Server
node client.js

The client sends a message and displays the server's reply.

6. Run the three microservices and API Gateway
Open four new VS Code terminals. Keep all four terminals running.
Terminal 1: Product Service
powershell
cd Gateway-Microservices/product-service
node server.js

Terminal 2: Customer Service
powershell
cd Gateway-Microservices/customer-service
node server.js

Terminal 3: Order Service
powershell
cd Gateway-Microservices/order-service
node server.js

Terminal 4: API Gateway
powershell
cd Gateway-Microservices/api-gateway
node server.js

The Gateway should display:

API Gateway is running at http://127.0.0.1:3000
All client requests must use the Gateway.

Start the three microservices before starting the Gateway.

7. Check the API Gateway
Open this address in a browser:

http://127.0.0.1:3000/health

The three client-facing routes are:

http://127.0.0.1:3000/api/products
http://127.0.0.1:3000/api/customers
http://127.0.0.1:3000/api/orders

Clients should use port 3000. The backend service ports are 3001, 3002 and 3003.

8. Test the HTTP methods
Open a fifth VS Code terminal using Windows PowerShell. Use `curl.exe` for HTTP requests. For JSON POST or PUT commands, use the working Windows PowerShell form `curl.exe --%` followed by JSON with escaped quotation marks (`\\"`). The `--%` stop-parsing operator is for Windows PowerShell, not Command Prompt.
GET: View all products
powershell
curl.exe -i http://127.0.0.1:3000/api/products

GET: View one product
powershell
curl.exe -i http://127.0.0.1:3000/api/products/1001

POST: Add a product
powershell
curl.exe --% -i -X POST http://localhost:3000/api/products -H "Content-Type: application/json" -d "{\"name\":\"USB-C Cable\",\"category\":\"Accessories\",\"price\":8.99,\"stock\":100}"

PUT: Update a product
powershell
curl.exe --% -i -X PUT http://localhost:3000/api/products/1004 -H "Content-Type: application/json" -d "{\"name\":\"USB-C Cable Pro\",\"category\":\"Accessories\",\"price\":12.99,\"stock\":80}"

DELETE: Delete a product
powershell
curl.exe -i -X DELETE http://127.0.0.1:3000/api/products/1004

The same four HTTP methods are available for customers and orders. The separate testing document contains 69 HTTP cURL tests and 5 Socket Client-Server tests. Follow their required order; the example PUT and DELETE above assume the POST created product ID 1004. Actual test results must be recorded only after execution.

9. How the API Gateway works
The client sends a request to port 3000.
Express receives the request.
The Gateway loads the Express Router from `routes/index.js`, which exports `routes/gatewayRoutes.js`.
The Gateway selects the correct microservice.
The Express Router forwards the request to the backend service.
The Gateway adds an internal token to the request.
The microservice checks the token. A direct request without the token receives HTTP 403.
The microservice reads or updates its JSON file.
The response travels back through the Gateway to the client.
The token is a simple coursework demonstration, not production-grade authentication.

10. Reset the JSON data
POST, PUT and DELETE change the JSON files. To restore the original records:
Stop the running services by pressing Ctrl+C in their terminals.
Open a terminal at the main project folder.
Run:
powershell
node reset-data.js

The original products, customers and orders will be restored. Start the services again before testing.

11. Stop the servers
Click each server terminal and press Ctrl+C.

12. Common problems
`node` is not recognised: Install Node.js, restart VS Code and check `node --version`.
`Cannot find module 'express'`: Run `npm install` inside the relevant service folder.
Port is already in use: Find the terminal running the other server and press Ctrl+C.
Gateway returns 502: Check that the selected microservice is running.
Gateway returns 404: Check the URL spelling.
Direct service access returns 403: This is expected. Use the Gateway on port 3000.

13. Important notes
Install Express separately in the four HTTP application folders.
Start the three microservices before starting the Gateway.
Keep all four server terminals open while testing.
Use port 3000 for normal client requests.
Take screenshots of the actual VS Code terminal output for your portfolio.
The CURL test file is supplied separately.
