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
- Square root, cube root, power and factorial
- Calculator triggered by end users via a web api
- References: https://blog.miguelgrinberg.com/post/designing-a-restful-api-with-python-and-flask

```
HTTP METHOD | URI                                   | Action
-----------   --------------------------------------  ----------------
ADD         | http://[hostname]/add | Adds two numbers
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
    return render_template('index.html')

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
$ chmod a+x app.py
$ ./calculator.py
$ curl http://localhost:5000
$ curl -i -H "Content-Type: application/json" -X POST -d '{"1":"2"}' http://localhost:5000/todo/api/v1.0/tasks
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
