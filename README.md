# FastAPI & AWS Lambda Tutorial

In this project, we'll be creating an application using FastAPI. APIs are the backbone of internet services, and I think that the FastAPI framework (released in 2018) is one of the best ways for Python users to easily make custom APIs with high performance. After that you'll learn how to deploy your FastAPI on AWS Lambda so you can host it serverlessly and not have to stress about server maintenance. This is a simple example FastAPI application that pretends to be a bookstore. Server based hosting with AWS EC2 is hard to scale out with traffic increases, difficult to do rolling updates for an app, hard to do security patching, and it's quiet expensive. AWS Lambda is a serverless compute for hosting APIs. Lambda takes care of all of the scaling, security, hosting and load balancing problems. Serverless hosting is scalable, cheap and requires low maintenance.

## FastAPI
An Application Programming Interface (API) is an interface between your service and the rest of the world (users). Requesting a ride from Uber, booking an Airbnb, or tweeting on X are all examples of APIs in action.

### Install FastAPI

Install [FastAPI](https://fastapi.tiangolo.com/#installation) by running:
```sh
pip install fastapi
```
Use the [Create It example](https://fastapi.tiangolo.com/#create-it) to start a main.py file

### Run FastAPI
Run the server with:
```sh
fastapi dev main.py
```
### Create a Database
Create a fake database with a global variable (i.e. BOOK_DATABASE = [])

### Data models with JSON
import BaseModel from pydantic library which is already installed as it is a part of fastapi

### Building APIs
Play around with different API interfaces and check http://127.0.0.1:8000/docs#/ to see a summary of each API method

Examples:
#### GET methods:
1. "/" - Create a home index method that runs a simple health check
2. "/list-books" - returns a list of all the books existing in our database
3. "/book-by-index/{index}" - find a book by index #. Out of range will return an error message
4. "/get-random-book" - return a random book in database
5. "/get-book" - search a book by id

#### POST methods:
6. "/add-book" - input into FastAPI will add a book to your database 

### Using Mangum to adapt the app
Magnum is an adapter for running ASGI applications in AWS Lambda to handle function URL, API Gateway, and more. 

Install Magnum:
```sh
pip install magnum
```
Wrap your entire app with a Mangum handler:
```python
from mangum import Mangum

app = FastAPI()
handler = Mangum(app)
```

### Deploying FastAPI to AWS Lambda
We'll also need to install the dependencies into a local directory so we can zip it up.

```pwsh
pip install -t lib -r requirements.txt
```

Change directory to the library folder:
```pwsh
cd lib
```
We now need to zip it up:
```pwsh
Compress-Archive -Path . -DestinationPath ../lambda_function.zip
```

Now add our FastAPI file and the JSON file.

```pwsh
Compress-Archive -Path main.py -Update -DestinationPath lambda_function.zip
Compress-Archive -Path books.json -Update -DestinationPath lambda_function.zip
```
Tutorial videos:
1) [FastAPI Python Tutorial - Learn How to Build a REST API](https://www.youtube.com/watch?v=34cqrIp5ANg)
2) [Deploy FastAPI on AWS Lambda ⚡ Serverless hosting!](https://www.youtube.com/watch?v=RGIM4JfsSk0)
