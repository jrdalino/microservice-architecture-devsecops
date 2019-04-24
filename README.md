# python-calculator-rest-api-docker-kubernetes

## Prerequisites
Python
```
$ Python3 --version
```
Flask
```
$ flask --version
```
Docker
```
$ docker -v
```
Git
```
$ git --version
```
AWS CLI
```
$ aws --version
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
- References: 
- https://github.com/ajportier/rest-calculator
- https://blog.miguelgrinberg.com/post/designing-a-restful-api-using-flask-restful
- TODO: Use Flask-RESTful https://blog.miguelgrinberg.com/post/designing-a-restful-api-using-flask-restful

```
HTTP METHOD | URI                                                         | Action
-----------   -----------------------------------------------------------   --------------------------------------------------
POST        | http://[hostname]/add {"argument1":a, "argument2":b }       | Adds two numbers (a + b)
POST        | http://[hostname]/subtract {"argument1":a, "argument2":b }  | Subracts two numbers (a - b)
POST        | http://[hostname]/multiply {"argument1":a, "argument2":b }  | Multiplies two numbers (a * b)
POST        | http://[hostname]/divide {"argument1":a, "argument2":b }    | Divides two numbers (a / b)
POST        | http://[hostname]/squareroot {"argument1":a }               | Gets the square root of a number (a)
POST        | http://[hostname]/cuberoot {"argument1":a }                 | Gets the cube root of a number (a)
POST        | http://[hostname]/exp {"argument1":a, "argument2":b }       | Gets the the exponent of a raised to b
POST        | http://[hostname]/factorial {"argument1":a }                | Get the factorial of number 5! = 5 * 4 * 3 * 2 * 1 
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

@app.route('/subtract', methods=['POST'])
def subtract_args():
    if not request.json:
        abort(400)
    try:
        arg1 = request.json['argument1']
        arg2 = request.json['argument2']
        answer = arg1 - arg2
        return (jsonify({'answer':answer}), 200)
    except KeyError:
        abort(400)

@app.route('/multiply', methods=['POST'])
def multiply_args():
    if not request.json:
        abort(400)
    try:
        arg1 = request.json['argument1']
        arg2 = request.json['argument2']
        answer = arg1 * arg2
        return (jsonify({'answer':answer}), 200)
    except KeyError:
        abort(400)

@app.route('/divide', methods=['POST'])
def divide_args():
    if not request.json:
        abort(400)
    try:
        arg1 = request.json['argument1']
        arg2 = request.json['argument2']
        answer = arg1 / arg2
        return (jsonify({'answer':answer}), 200)
    except KeyError:
        abort(400)
    except ZeroDivisionError:
        abort(400)
        
# TODO Square Root
# TODO Cube Root
        
@app.route('/exp', methods=['POST'])
def exponent_args():
    if not request.json:
        abort(400)
    try:
        arg1 = request.json['argument1']
        arg2 = request.json['argument2']
        answer = arg1 ** arg2
        return (jsonify({'answer':answer}), 200)
    except KeyError:
        abort(400)

# TODO Factorial

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

### Module 1.3 Run Locally and Test
```
$ chmod a+x calculator.py
$ ./calculator.py
$ curl http://localhost:5000
```

Test Add
```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"argument1":2, "argument2":1 }' http://localhost:5000/add
{
  "answer": 3
}
```
Test Subtract
```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"argument1":4, "argument2":3 }' http://localhost:5000/subtract
{
  "answer": 1
}
```
Test Mulitply
```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"argument1":2, "argument2":3 }' http://localhost:5000/multiply
{
  "answer": 6
}
```
Test Divide
```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"argument1":12, "argument2":4 }' http://localhost:5000/divide
{
  "answer": 3
}
```
Test Square Root
Test Cube Root

Test Power
```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"argument1":2, "argument2":3 }' http://localhost:5000/exp
{
  "answer": 8
}
```

Test Factorial

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

### Module 1.5 Build and Run Locally
```
$ docker build -t python-calculator-rest-api-docker-kubernetes:latest .
$ docker run -d -p 5000:5000 python-calculator-rest-api-docker-kubernetes:latest
```
Test again

## Module 2: Backend Unit Tests
- References:
http://liangshang.github.io/2014/01/17/a-simple-calculator-by-python-and-tdd

## Module 3: Frontend HTML, CSS and JS for Calculator Local
- Calculator triggered by end users through a web page

## Module 4: Frontend Unit Tests
TODO: Front End Unit Tests

## Module 5: Push Backend and Frontend to ECR
### Module 5.1

## Module 6: Create Amazon EKS Cluster VPC using Cloudformation
```
https://amazon-eks.s3-us-west-2.amazonaws.com/cloudformation/2019-02-11/amazon-eks-vpc-sample.yaml
```

## Module 7: Configure Prerequisites for EKS

### Module 7.1: Create the default ~/.kube directory for storing kubectl configuration
```
$ mkdir -p ~/.kube
```

### Module 7.2: Install kubectl
```
sudo curl --silent --location -o /usr/local/bin/kubectl "https://amazon-eks.s3-us-west-2.amazonaws.com/1.11.5/2018-12-06/bin/linux/amd64/kubectl"
```
```
$ sudo chmod +x /usr/local/bin/kubectl
```

### Module 7.3: Install IAM Authenticator
```
$ go get -u -v github.com/kubernetes-sigs/aws-iam-authenticator/cmd/aws-iam-authenticator
```
```
$ sudo mv ~/go/bin/aws-iam-authenticator /usr/local/bin/aws-iam-authenticator
```

### Module 7.4 Install JQ and envsubst
```
$ sudo yum -y install jq gettext
```

### Module 7.5 Verify the binaries are in the path and executable
```
$ for command in kubectl aws-iam-authenticator jq envsubst
  do
    which $command &>/dev/null && echo "$command in path" || echo "$command NOT FOUND"
  done
```

### Module 7.6 Clone the Frontend and Backend Sevice Repos
```
$ cd ~/environment
$ git clone https://github.com/jrdalino/calculator-frontend.git
$ git clone https://github.com/jrdalino/calculator-python.git
```

### Module 7.7 Update IAM Settings for your Workspace
- Reference: https://eksworkshop.com/prerequisites/workspaceiam/

### Module 7.7 Create an SSH Key
- Reference: https://eksworkshop.com/prerequisites/sshkey/

## Module 8: Launch EKS using EKCTL

### Module 8.1
```
$ cd ~/environment
$ git clone https://github.com/jrdalino/calculator-frontend.git
$ git clone https://github.com/jrdalino/calculator-python.git
```
### Module 8.2 Install EKSCTL prerequisites
```
$ curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
$ sudo mv -v /tmp/eksctl /usr/local/bin
$ eksctl version
```

### Module 8.3 Create an EKS Cluster (This will take ~15 minutes) and test cluster
```
$ eksctl create cluster --name=eksworkshop-eksctl --nodes=3 --node-ami=auto --region=${AWS_REGION}
```
```
$ kubectl get nodes
```

## Module 9: Deploy MicroServices to EKS
- containerized with Docker and Kubernetes for the orchestration

### Module 9.1

## Module 10: Setup CI/CD for Front End
- proper CI/CD processes to put in place

## Module 11: Setup CI/CD for Back End
- proper CI/CD processes to put in place

## Module 12: Setup Monitoring using Prometheus and Grafana
- Basic monitoring

## Module 13: Implement Health Checks
- The API being a crucial part of the application it needs to be highly available

## Module 14: Implementing Auto Scaling

## Module 15: Log Amazon EKS API Calls with CloudTrail

## Module 16: Log REST operations performed to S3

## Module 17: S3 and Athena Report
- Daily/weekly/monthly report showing the operations that have been performed during that time period

## Module 18: Add additional feature
- Additional killer feature of your choice
