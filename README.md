# python-calculator-rest-api-docker-kubernetes

## Prerequisites
Docker
```
$ docker -v
```
Python
```
$ Python3 --version
```
Flask
```
$ flask --version
```
Create and Navigate to Directory
```
$ mkdir python-calculator-rest-api-docker-kubernetes
$ cd python-calculator-rest-api-docker-kubernetes
$ virtualenv flask
$ flask/bin/pip install flask
```

## Module 1: Backend Python Flask Rest API for Calculator Local
- Basic calculations (add, subtract, multiply, divide)
- Advanced calculations (square root, cube root, power, factorial)
- Calculator triggered by developers via a web api

```
HTTP METHOD | URI                                                         | Action
-----------   -----------------------------------------------------------   ----------------
POST        | http://[hostname]/add {"argument1":1, "argument2":2 }       | Adds two numbers
POST        | http://[hostname]/subtract {"argument1":1, "argument2":2 }  | Subracts two numbers
POST        | http://[hostname]/multiply {"argument1":1, "argument2":2 }  | Multiplies two numbers
POST        | http://[hostname]/divide {"argument1":1, "argument2":2 }    | Divides two numbers
POST        | http://[hostname]/squareroot {"argument1":1 }               | Gets the square root of a number
POST        | http://[hostname]/cuberoot {"argument1":1 }                 | Gets the cube root of a number
POST        | http://[hostname]/exp {"argument1":1, "argument2":2 }       | Gets the the exponent of number1 raised to number2
POST        | http://[hostname]/factorial {"argument1":1 }                | Get the factorial of a number
```

### Module 1.1 Add calculator.py
```
$ vi calculator.py
```
```
#!/usr/bin/env python
from flask import (Flask, jsonify, request, abort, render_template)
import math

app = Flask(__name__)

@app.route('/')
def index_page():
    return "This is a RESTful Calculator App built with Python Flask!"
#    return render_template('index.html')

@app.route('/add', methods=['POST'])
def add_args():
    if not request.json:
        abort(400)
    try:
        arg1 = request.json['argument1']
        arg2 = request.json['argument2']
        answer = arg1 + arg2
        return (jsonify({'answer':answer}), 200)
    except KeyError:
        abort(400)
        
if __name__ == '__main__':
    app.run(debug=True)
```

### Module 1.2 Create the requirements.txt file
```
$ vi requirements.txt
```
```
flask
```

### Module 1.3 Run Locally
```
$ chmod a+x calculator.py
$ ./calculator.py
$ curl http://localhost:5000
$ curl -i -H "Content-Type: application/json" -X POST -d '{"argument1":1, "argument2":2 }' http://localhost:5000/add
```
```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"argument1":1, "argument2":2 }' http://localhost:5000/add
HTTP/1.0 200 OK
Content-Type: application/json
Content-Length: 18
Server: Werkzeug/0.15.2 Python/2.7.10
Date: Wed, 24 Apr 2019 01:08:53 GMT

{
  "answer": 3
}
```

### Module 1.4 Create the Dockerfile
```
$ vi Dockerfile
```
```
# Dockerfile - this is a comment. Delete me if you want.
FROM python:2.7
COPY . /calculator
WORKDIR /calculator
RUN pip install -r requirements.txt
ENTRYPOINT ["python"]
CMD ["calculator.py"]
```

### Module 1.4 Build and Run Locally
```
$ docker build -t python-calculator-rest-api-docker-kubernetes:latest .
$ docker run -d -p 5000:5000 python-calculator-rest-api-docker-kubernetes:latest
$ curl http://127.0.0.1:5000/add/1
{
    "2"
}
```


## Module 2: Backend Unit Tests

## Module 3: Frontend HTML, CSS and JS for Calculator Local
- Calculator triggered by end users through a web page

## Module 4: Front End Unit Tests

## Module 5: Push to ECR

## Module 6: Cloudformation Stack

## Module 7: Launch EKS using EKCTL

## Module 8: Deploy MicroServices to EKS
- containerized with Docker and Kubernetes for the orchestration

## Module 9: Setup CI/CD for Front End
- proper CI/CD processes to put in place

## Module 10: Setup CI/CD for Back End
- proper CI/CD processes to put in place

## Module 11: Setup Monitoring using Prometheus and Grafana
- Basic monitoring

## Module 12: Implement Health Checks
- The API being a crucial part of the application it needs to be highly available

## Module 13: Implementing Auto Scaling

## Module 14: Log REST operations performed to S3

## Module 15: S3 and Athena Report
- Daily/weekly/monthly report showing the operations that have been performed during that time period

## Module 16: Add additional feature
- Additional killer feature of your choice
