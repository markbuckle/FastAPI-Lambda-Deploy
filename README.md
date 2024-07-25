# FastAPI & AWS Lambda Tutorial

In this project you'll learn how to deploy your FastAPI on AWS Lambda so you can host it serverlessly and not have to stress about server maintenance. This is a simple example FastAPI application that pretends to be a bookstore.

Server based hosting with AWS EC2 is hard to scale out with traffic increases, difficult to do rolling updates for an app, hard to do security patching, and it's quiet expensive. 

AWS Lambda is a serverless compute for hosting APIs. Lambda takes care of all of the scaling, security, hosting and load balancing problems. Serverless hosting is scalable, cheap and requires low maintenance.

## FastAPI
An Application Programming Interface (API) is an interface between your service and the rest of the world (users). Requesting a ride from Uber, booking an Airbnb, or tweeting on X are all examples of APIs in action.

## Deploying to AWS EC2

Log into your AWS account and create an EC2 instance (`t2.micro`), using the latest stable
Ubuntu Linux AMI.

[SSH into the instance](https://aws.amazon.com/blogs/compute/new-using-amazon-ec2-instance-connect-for-ssh-access-to-your-ec2-instances/) and run these commands to update the software repository and install
our dependencies.

```bash
sudo apt-get update
sudo apt install -y python3-pip nginx
```

Clone the FastAPI server app (or create your `main.py` in Python).

```bash
git clone https://github.com/pixegami/fastapi-tutorial.git
```

Add the FastAPI configuration to NGINX's folder. Create a file called `fastapi_nginx` (like the one in this repository).

```bash
sudo vim /etc/nginx/sites-enabled/fastapi_nginx
```

And put this config into the file (replace the IP address with your EC2 instance's public IP):

```
server {
    listen 80;   
    server_name <YOUR_EC2_IP>;    
    location / {        
        proxy_pass http://127.0.0.1:8000;    
    }
}
```


Start NGINX.

```bash
sudo service nginx restart
```

Start FastAPI.

```bash
cd fastapi-tutorial
python3 -m uvicorn main:app
```

Update EC2 security-group settings for your instance to allow HTTP traffic to port 80.

Now when you visit your public IP of the instance, you should be able to access your API.

# Deploying FastAPI to AWS Lambda

We'll need to modify the API so that it has a Lambda handler. Use Mangum:

```python
from mangum import Mangum

app = FastAPI()
handler = Mangum(app)
```

We'll also need to install the dependencies into a local directory so we can zip it up.

```bash
pip install -t lib -r requirements.txt
```

We now need to zip it up.

```bash
(cd lib; zip ../lambda_function.zip -r .)
```

Now add our FastAPI file and the JSON file.

```bash
zip lambda_function.zip -u main.py
zip lambda_function.zip -u books.json
```

Tutorial videos:
1) [FastAPI Python Tutorial - Learn How to Build a REST API](https://www.youtube.com/watch?v=34cqrIp5ANg)
2) [Deploy FastAPI on AWS Lambda ⚡ Serverless hosting!](https://www.youtube.com/watch?v=RGIM4JfsSk0)
