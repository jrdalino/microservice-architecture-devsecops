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
$ git config --global user.name "REPLACE_ME_WITH_YOUR_NAME"
$ git config --global user.email REPLACE_ME_WITH_YOUR_EMAIL@example.com
$ git config --global credential.helper '!aws codecommit credential-helper $@'
$ git config --global credential.UseHttpPath true
```

AWS CLI
```
$ aws --version
$ aws configure
```
Homebrew
```
$ brew --version
```
Working Directory
```
$ cd ~
$ mkdir environment
$ cd ~/environment
```

Backend Project Layout will look like this:
```
~/environment/calculator-backend
├── aws-cli/
│   ├── artifacts-bucket-policy.json
│   └── code-build-project.json
├── cfn/
│   └── eks-calculator-codebuild-codepipeline-iam-role.yml
├── app.py
├── buildspec.yml
├── calculator.py
├── kubernetes/
│   ├── deployment.yml
│   └── service.yml
├── requirements.txt
├── test_calculator.py
├── venv/
├── Dockerfile
├── README.md
└── .gitignore
```

Frontend Project Layout will look like this:
```
~/environment/calculator-frontend
├── aws-cli/
│   ├── artifacts-bucket-policy.json
│   ├── code-pipeline.json
│   ├── website-bucket-policy.json
│   └── website-cloudfront-distribution.json
├── base.css
├── cfn/
│   └── eks-calculator-codebuild-codepipeline-iam-role.yaml
├── index.html
├── querycalc.js
└── README.md
```

### **************************************************************
### **************************************************************
### **************************************************************

## Module 1: Configure Calculator Backend Git Repository

### Step 1.1: Create a CodeCommit Repository
```
$ aws codecommit create-repository --repository-name calculator-backend
```

### Step 1.2: Clone the repository
```
$ cd ~/environment
$ git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/calculator-backend
```

### Step 1.3: Setup .gitignore
```
$ cd ~/environment/calculator-backend
$ vi .gitignore
```
```
venv
*.pyc
```

### Step 1.4: Test access to repo by adding README.md file and push to remote repository
```
$ cd ~/environment/calculator-backend
$ echo "calculator-backend" >> README.md
$ git add .
$ git commit -m "Adding README.md"
$ git push origin master
```

### (Optional) Clean up
```
$ aws codecommit delete-repository --repository-name calculator-backend
```

### **************************************************************
### **************************************************************
### **************************************************************

## Module 2: Backend Python Flask Rest API for Calculator Local
- Basic calculations (add, subtract, multiply, divide)
- Advanced calculations (square root, cube root, power, factorial)
- Calculator triggered by developers via a web api

```
HTTP METHOD | URI                                                         | Action
-----------   -----------------------------------------------------------   --------------------------------------------------
POST        | http://[hostname]/add {"argument1":a, "argument2":b }       | Adds two numbers (a + b)
POST        | http://[hostname]/subtract {"argument1":a, "argument2":b }  | Subracts two numbers (a - b)
POST        | http://[hostname]/multiply {"argument1":a, "argument2":b }  | Multiplies two numbers (a * b)
POST        | http://[hostname]/divide {"argument1":a, "argument2":b }    | Divides two numbers (a / b)
POST        | http://[hostname]/sqrt {"argument1":a }                     | Gets the square root of a number (a)
POST        | http://[hostname]/cbrt {"argument1":a }                     | Gets the cube root of a number (a)
POST        | http://[hostname]/exp {"argument1":a, "argument2":b }       | Gets the the exponent of a raised to b
POST        | http://[hostname]/factorial {"argument1":a }                | Get the factorial of number 5! = 5 * 4 * 3 * 2 * 1 
```

### Step 2.1: Navigate to working directory
```
$ cd ~/environment/calculator-backend
$ virtualenv venv
$ venv/bin/pip install flask
```

### Step 2.2 Create Calculator Class Calculator.py
```
$ cd ~/environment/calculator-backend
$ vi calculator.py
```
```
#!/usr/bin/env python
import math

class Calculator:
    def __init__(self):
        pass

    def add(self, arg1, arg2):
        return float(arg1) + float(arg2)

    def subtract(self, arg1, arg2):
        return float(arg1) - float(arg2)

    def multiply(self, arg1, arg2):
        return float(arg1) * float(arg2)

    def divide(self, arg1, arg2):
        return float(arg1) / (arg2)

    def sqrt(self, arg1):
        return math.sqrt(float(arg1))

    def cbrt(self, arg1):
        return round(float(arg1)**(1.0/3))

    def exp(self, arg1, arg2):
        return float(arg1) ** float(arg2)

    def factorial(self, arg1):
       if arg1 == 1:
          return arg1
       else:
          return float(arg1)*self.factorial(float(arg1)-1)
```

### Step 2.3: Add app.py
```
$ cd ~/environment/calculator-backend
$ vi app.py
```
```
#!/usr/bin/env python
from flask import (Flask, jsonify, request, abort, render_template)
from flask_cors import CORS
from calculator import Calculator

app = Flask(__name__)
CORS(app)

@app.route('/')
def index_page():
    return "This is a RESTful Calculator App built with Python Flask!"

@app.route('/add', methods=['POST'])
def add_args():
    if not request.json:
        abort(400)
    try:
        arg1 = request.json['argument1']
        arg2 = request.json['argument2']
        calculator = Calculator()
        answer = calculator.add(arg1, arg2)
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
        calculator = Calculator()
        answer = calculator.subtract(arg1, arg2)
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
        calculator = Calculator()
        answer = calculator.multiply(arg1, arg2)
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
        calculator = Calculator()
        answer = calculator.divide(arg1, arg2)
        return (jsonify({'answer':answer}), 200)
    except KeyError:
        abort(400)
    except ZeroDivisionError:
        abort(400)

@app.route('/sqrt', methods=['POST'])
def sqrt_args():
    if not request.json:
        abort(400)
    try:
        arg1 = request.json['argument1']
        calculator = Calculator()
        answer = calculator.sqrt(arg1)
        return (jsonify({'answer':answer}), 200)
    except KeyError:
        abort(400)

@app.route('/cbrt', methods=['POST'])
def cbrt_args():
    if not request.json:
        abort(400)
    try:
        arg1 = request.json['argument1']
        calculator = Calculator()        
        answer = calculator.cbrt(arg1)
        return (jsonify({'answer':answer}), 200)
    except KeyError:
        abort(400)

@app.route('/exp', methods=['POST'])
def exponent_args():
    if not request.json:
        abort(400)
    try:
        arg1 = request.json['argument1']
        arg2 = request.json['argument2']
        calculator = Calculator()        
        answer = calculator.exp(arg1, arg2)
        return (jsonify({'answer':answer}), 200)
    except KeyError:
        abort(400)

@app.route('/factorial', methods=['POST'])
def factorial_args():
    if not request.json:
        abort(400)
    try:
        arg1 = request.json['argument1']
        calculator = Calculator()          
        answer = calculator.factorial(arg1)
        return (jsonify({'answer':answer}), 200)
    except KeyError:
        abort(400)

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```

### Step 2.4: Create the requirements.txt file
```
$ cd ~/environment/calculator-backend
$ vi requirements.txt
```
```
flask
flask_restful
flask_cors
```

### Step 2.5: Run Locally and Test
```
$ cd ~/environment/calculator-backend
$ chmod a+x app.py
$ ./app.py
$ curl http://localhost:5000
```

### Step 2.6: Backend Unit Tests
```
$ cd ~/environment/calculator-backend
$ vi test_calculator.py
```
```
#!/usr/bin/env python
import unittest
from calculator import Calculator

class CalculatorTest(unittest.TestCase):
    calculator = Calculator()
    
    def test_add(self):
        self.assertEqual(4, self.calculator.add(2,2))

    def test_subtract(self):
        self.assertEqual(2, self.calculator.subtract(3,1))
        self.assertEqual(-2, self.calculator.subtract(1,3))

    def test_multiply(self):
        self.assertEqual(12, self.calculator.multiply(3,4))
        self.assertEqual(13.5, self.calculator.multiply(3,4.5))

    def test_divide(self):
        self.assertEqual(3, self.calculator.divide(9,3))
        with self.assertRaises(ZeroDivisionError):
            self.calculator.divide(3,0) 
    
    def test_sqrt(self):
        self.assertEqual(4, self.calculator.sqrt(16))

    def test_cbrt(self):
        self.assertEqual(4, self.calculator.cbrt(64))

    def test_exp(self):
        self.assertEqual(32, self.calculator.exp(2,5))

    def test_factorial(self):
        self.assertEqual(120, self.calculator.factorial(5))

if __name__ == "__main__":
    unittest.main()
```

### Step 2.7: Run Unit Tests
```
$ chmod a+x test_calculator.py
$ ./test_calculator.py -v
```

### Step 2.8 Save changes to remote git repository
```
$ git add .
$ git commit -m "Initial"
$ git push origin master
```

### Step 2.9: Create the Dockerfile
```
$ cd ~/environment/calculator-backend
$ vi Dockerfile
```
```
# Set base image to python
FROM python:2.7

# Copy source file and python req's
COPY . /app
WORKDIR /app

# Install requirements
RUN pip install -r requirements.txt

# Set image's main command and run the command within the container
ENTRYPOINT ["python"]
CMD ["app.py"]
```

### Step 2.10: Build, Tag and Run the Docker Image locally
Replace:
- AccountId: 707538076348
- Region: us-east-1

```
$ docker build . -t 707538076348.dkr.ecr.us-east-1.amazonaws.com/jrdalino/calculator-backend:latest
$ docker run -d -p 5000:5000 707538076348.dkr.ecr.us-east-1.amazonaws.com/jrdalino/calculator-backend:latest
```

### Step 2.11: Test Math Operations
- Test Add
```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"argument1":2, "argument2":1 }' http://localhost:5000/add
{
  "answer": 3
}
```

- Test Subtract
```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"argument1":4, "argument2":3 }' http://localhost:5000/subtract
{
  "answer": 1
}
```

- Test Multiply
```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"argument1":2, "argument2":3 }' http://localhost:5000/multiply
{
  "answer": 6
}
```

- Test Divide
```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"argument1":12, "argument2":4 }' http://localhost:5000/divide
{
  "answer": 3
}
```

- Test Square Root
```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"argument1":9 }' http://localhost:5000/sqrt
{
  "answer": 3
}
```

- Test Cube Root
```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"argument1":64 }' http://localhost:5000/cbrt
{
  "answer": 4
}
```

- Test Power
```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"argument1":2, "argument2":3 }' http://localhost:5000/exp
{
  "answer": 8
}
```

- Test Factorial
```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"argument1":5 }' http://localhost:5000/factorial
{
  "answer": 120
}
```

### Step 2.12: Create the ECR Repository
```
$ aws ecr create-repository --repository-name jrdalino/calculator-backend
```

### Step 2.13: Run login command to retrieve credentials for our Docker client and then automatically execute it (include the full command including the $ below).
```
$ $(aws ecr get-login --no-include-email)
```

### Step 2.14: Push our Docker Image
```
$ docker push 707538076348.dkr.ecr.us-east-1.amazonaws.com/jrdalino/calculator-backend:latest
```

### Step 2.15: Validate Image has been pushed
```
$ aws ecr describe-images --repository-name jrdalino/calculator-backend
```

### (Optional) Clean up
```
$ aws ecr delete-repository --repository-name jrdalino/calculator-backend --force
```

### **************************************************************
### **************************************************************
### **************************************************************

## Module 3: Configure Calculator Frontend Git Repository

### Step 3.1: Create a CodeCommit Repository
```
$ aws codecommit create-repository --repository-name calculator-frontend
```

### Step 3.2: Clone the repository
```
$ cd ~/environment
$ git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/calculator-frontend
```

### Step 3.3: Test access to repo by adding README.md file and push to remote repository
```
$ cd ~/environment/calculator-frontend
$ echo "calculator-frontend" >> README.md
$ git add .
$ git commit -m "Adding README.md"
$ git push origin master
```

### (Optional) Clean up
```
$ aws codecommit delete-repository --repository-name calculator-frontend
```
### **************************************************************
### **************************************************************
### **************************************************************

## Module 4: Frontend HTML, CSS, JS and Bootstrap for Calculator Local
- Calculator triggered by end users through a web page

### Step 4.1: Navigate to working directory
```
$ cd ~/environment/calculator-frontend
```

### Step 4.2: Create index.html file
```
$ vi index.html
```
```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <title>Simple RESTful Calculator - Web Client</title>
    </head>
    <body>
        <div class="outer-container">
            <h1>Simple RESTful Calculator</h1>
            <form id="calcform" method="POST">
            <div class="arguments">
                Argument 1: <input type="text" id="argument1" value = ""/><br/>
                Argument 2: <input type="text" id="argument2" value = ""/><br/>
            </div>
            <div class="buttons">
                <div class="button-row">
                    <input class= "func-btn" type="button" id="add" value="Add"/>
                    <input class= "func-btn" type="button" id="subtract" value="Subtract"/>
                </div>
                <div class="button-row">
                    <input class= "func-btn" type="button" id="multiply" value="Multiply"/>
                    <input class= "func-btn" type="button" id="divide" value="Divide"/>
                </div>
                <div class="button-row">
                    <input class= "func-btn" type="button" id="sqrt" value="SquareRoot"/>
                    <input class= "func-btn" type="button" id="cbrt" value="CubeRoot"/>
                </div>		
                <div class="button-row">
                    <input class= "func-btn" type="button" id="exp" value="Exponent"/>
                    <input class= "func-btn" type="button" id="factorial" value="Factorial"/>		    
                </div>
            </div>
            </form>
        </div>

        <link rel="stylesheet" type="text/css" href="./base.css">
        <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
        <script src="./querycalc.js"></script>
        <script type="text/javascript">
            // once the DOM is ready start the calculator listener function
            $( document ).ready( calcListener ); 
        </script>

    </body>
</html>
```

### Step 4.3: Create base.css file
```
$ vi base.css
```
```
body {
    margin: 0;
    padding: 0;
    background-color: white;
}

.outer-container {
    width: 500px;
    height: 500px;
    margin-left: auto;
    margin-right: auto;
    overflow: auto;
    text-align: center;
    background-color: lightblue;
}

.arguments {
    margin: auto;
    padding: 5px;
}

.buttons {
    padding: 5px;
}

.button-row {
    height: 50px;
}

.func-btn {
	-webkit-appearance: none;
    width: 50%;
    height: 50px;
    float: left;
    padding: 5px;
}
```

### Step 4.4: Create querycalc.js file
```
$ vi querycalc.js
```
```
function calcListener ( jQuery ) {
    console.log( "READY!" );

    $( "#add" ).click( function ( e ) {
        var arg1 = $( "#argument1" ).val();
        var arg2 = $( "#argument2" ).val();
        doMath (arg1, arg2, 'add');
        e.preventDefault();
    });

    $( "#subtract" ).click( function ( e ) {
        var arg1 = $( "#argument1" ).val();
        var arg2 = $( "#argument2" ).val();
        doMath (arg1, arg2, 'subtract');
        e.preventDefault();
    });

    $( "#multiply" ).click( function ( e ) {
        var arg1 = $( "#argument1" ).val();
        var arg2 = $( "#argument2" ).val();
        doMath (arg1, arg2, 'multiply');
        e.preventDefault();
    });

    $( "#divide" ).click( function ( e ) {
        var arg1 = $( "#argument1" ).val();
        var arg2 = $( "#argument2" ).val();
        doMath (arg1, arg2, 'divide');
        e.preventDefault();
    });
    
    $( "#sqrt" ).click( function ( e ) {
        var arg1 = $( "#argument1" ).val();
        doMath (arg1, 1, 'sqrt');
        e.preventDefault();
    });
    
    $( "#cbrt" ).click( function ( e ) {
        var arg1 = $( "#argument1" ).val();
        doMath (arg1, 1, 'cbrt');
        e.preventDefault();
    });       
    
    $( "#exp" ).click( function ( e ) {
        var arg1 = $( "#argument1" ).val();
        var arg2 = $( "#argument2" ).val();
        doMath (arg1, arg2, 'exp');
        e.preventDefault();
    });
    
    $( "#factorial" ).click( function ( e ) {
        var arg1 = $( "#argument1" ).val();
        doMath (arg1, 1, 'factorial');
        e.preventDefault();
    })    
    
    function doMath( arg1, arg2, resource ) {
        var textStatus, jqXHR, errorThrown = '';

        console.log( "Calling " + resource + " on " + arg1 + " and " + arg2 );

        
        try {
            arg1 = Number( arg1 );
            arg2 = Number( arg2 );
	    
            //TODO handle non-numeric inputs
            if ( isNaN(arg1) || isNaN(arg2) ) throw "NaN";

            // Flask requires a JSON string and the following content-type
            $.ajaxSetup({
                contentType: "application/json"
            });

            // Makes an ajax call to url "resource" supplying arg1 and arg2
            $.ajax({
                type: "POST",
                url: "http://localhost:5000/" + resource,
                data: JSON.stringify({ argument1: arg1, argument2: arg2 }),
                dataType: "json",
                success: function ( data ) {
                    var answer = String(data[ 'answer' ]);
                    console.log( "We got an answer! " + answer );

                    // Put the answer in argument1 and blank out argument2
                    $( "#argument1" ).val( answer );
                    $( "#argument2" ).val( '' );
                },
                error: function ( textStatus, jqXHR, errorThrown ) {
                    console.log("Something has gone wrong:" + errorThrown);
                    $( "#argument1" ).val( '' );
                    $( "#argument2" ).val( '' );
                }
            });
        }

        catch( err ) {
            console.log("Error: " + err);
            $( "#argument1" ).val( '' );
            $( "#argument2" ).val( '' );
        }
    }
}
```

### Step 4.5: Test Locally

### Step 4.6: Save changes to remote git repository
```
$ git add .
$ git commit -m "Initial"
$ git push origin master
```

### **************************************************************
### **************************************************************
### **************************************************************

## Module 5: Install Kubernetes Tools

### Step 5.1: Create the default ~/.kube directory for storing kubectl configuration
```
$ mkdir -p ~/.kube
```

### Step 5.2: Install kubectl on MAC
```
$ sudo curl -o kubectl "https://amazon-eks.s3-us-west-2.amazonaws.com/1.11.5/2018-12-06/bin/darwin/amd64/kubectl"
$ sudo chmod +x ./kubectl
$ mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH
$ echo 'export PATH=$HOME/bin:$PATH' >> ~/.bash_profile
$ kubectl version --short --client
```

### Step 5.3: Install IAM Authenticator
```
$ brew install aws-iam-authenticator
$ aws-iam-authenticator help
```

### Step 5.4: Install JQ and envsubst
```
$ brew install jq
$ brew install gettext
$ brew link --force gettext
```

### Step 5.5: Verify the binaries are in the path and executable
```
$ for command in kubectl aws-iam-authenticator jq envsubst
  do
    which $command &>/dev/null && echo "$command in path" || echo "$command NOT FOUND"
  done
```

### Step 5.6: Generate an SSH Key for the Worker Nodes and upload the public key to your EC2 region
```
$ ssh-keygen
$ aws ec2 import-key-pair --key-name "eksworkernodes" --public-key-material file://~/.ssh/id_rsa.pub
```

### **************************************************************
### **************************************************************
### **************************************************************

## Module 6: Launch EKS using EKCTL

### Step 6.1: Download the eksctl binaries
```
$ brew install weaveworks/tap/eksctl
$ eksctl version
```

### Step 6.2: Create an EKS Cluster (This will take ~15 minutes) and test cluster
```
$ eksctl create cluster \
--name=calculator-eksctl \
--nodes=2 \
--node-ami=auto \
--node-type=t2.medium \ 
--region=${AWS_REGION}
$ kubectl get nodes
```

```
[ℹ]  using region us-east-1
[ℹ]  setting availability zones to [us-east-1d us-east-1a]
[ℹ]  subnets for us-east-1d - public:192.168.0.0/19 private:192.168.64.0/19
[ℹ]  subnets for us-east-1a - public:192.168.32.0/19 private:192.168.96.0/19
[ℹ]  nodegroup "ng-2e8e1187" will use "ami-0abcb9f9190e867ab" [AmazonLinux2/1.12]
[ℹ]  creating EKS cluster "calculator-eksctl" in "us-east-1" region
[ℹ]  will create 2 separate CloudFormation stacks for cluster itself and the initial nodegroup
[ℹ]  if you encounter any issues, check CloudFormation console or try 'eksctl utils describe-stacks --region=us-east-1 --name=calculator-eksctl'
[ℹ]  2 sequential tasks: { create cluster control plane "calculator-eksctl", create nodegroup "ng-2e8e1187" }
[ℹ]  building cluster stack "eksctl-calculator-eksctl-cluster"
[ℹ]  deploying stack "eksctl-calculator-eksctl-cluster"
[ℹ]  buildings nodegroup stack "eksctl-calculator-eksctl-nodegroup-ng-2e8e1187"
[ℹ]  --nodes-min=2 was set automatically for nodegroup ng-2e8e1187
[ℹ]  --nodes-max=2 was set automatically for nodegroup ng-2e8e1187
[ℹ]  deploying stack "eksctl-calculator-eksctl-nodegroup-ng-2e8e1187"
[✔]  all EKS cluster resource for "calculator-eksctl" had been created
[✔]  saved kubeconfig as "/Users/jrdalino/.kube/config"
[ℹ]  adding role "arn:aws:iam::707538076348:role/eksctl-calculator-eksctl-nodegrou-NodeInstanceRole-1IGO2PUALGGAC" to auth ConfigMap
[ℹ]  nodegroup "ng-2e8e1187" has 1 node(s)
[ℹ]  node "ip-192-168-24-30.ec2.internal" is not ready
[ℹ]  waiting for at least 2 node(s) to become ready in "ng-2e8e1187"
[ℹ]  nodegroup "ng-2e8e1187" has 2 node(s)
[ℹ]  node "ip-192-168-24-30.ec2.internal" is ready
[ℹ]  node "ip-192-168-35-7.ec2.internal" is ready
[ℹ]  kubectl command should work with "/Users/jrdalino/.kube/config", try 'kubectl get nodes'
[✔]  EKS cluster "calculator-eksctl" in "us-east-1" region is ready
```

### Step 6.3: Export Worker Role name
```
$ INSTANCE_PROFILE_NAME=$(aws iam list-instance-profiles | jq -r '.InstanceProfiles[].InstanceProfileName' | grep nodegroup)
$ ROLE_NAME=$(aws iam get-instance-profile --instance-profile-name $INSTANCE_PROFILE_NAME | jq -r '.InstanceProfile.Roles[] | .RoleName')
$ echo "export ROLE_NAME=${ROLE_NAME}" >> ~/.bash_profile
```

### (Optional) Clean up
```
$ eksctl delete cluster --name=calculator-eksctl
```

### **************************************************************
### **************************************************************
### **************************************************************

## Module 7: Deploy Backend MicroService to EKS
- containerized with Docker and Kubernetes for the orchestration

### Step 7.1: Create our deployment.yaml file
```
$ cd ~/environment/calculator-backend
$ mkdir kubernetes
$ vi ~/environment/calculator-backend/kubernetes/deployment.yaml
```

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: calculator-backend
  labels:
    app: calculator-backend
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      app: calculator-backend
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: calculator-backend
    spec:
      containers:
      - image: 707538076348.dkr.ecr.us-east-1.amazonaws.com/jrdalino/calculator-backend:latest
        imagePullPolicy: Always
        name: calculator-backend
        ports:
        - containerPort: 5000
          protocol: TCP
```

### Step 7.2: Create our service.yaml file
```
$ vi ~/environment/calculator-backend/kubernetes/service.yaml
```

```
apiVersion: v1
kind: Service
metadata:
  name: calculator-backend
spec:
  selector:
    app: calculator-backend
  type: LoadBalancer
  ports:
   -  protocol: TCP
      port: 80
      targetPort: 5000
```

### Step 7.3: Deploy our Backend REST API and watch progress
```
$ cd ~/environment/calculator-backend/kubernetes
$ kubectl apply -f deployment.yaml
$ kubectl apply -f service.yaml
$ kubectl get deployment calculator-backend
```

### Step 7.4: Scale the Backend Service
```
$ kubectl get deployments
$ kubectl scale deployment calculator-backend --replicas=1
$ kubectl get deployments
```

### Step 7.5: Ensure ELB service Role exists
```
$ aws iam get-role --role-name "AWSServiceRoleForElasticLoadBalancing" || aws iam create-service-linked-role --aws-service-name "elasticloadbalancing.amazonaws.com"
```

### Step 7.6: Find the Service Address
```
$ kubectl get service calculator-backend -o wide
```

### (Optional) Clean up
```
$ cd ~/environment/calculator-backend/kubernetes
$ kubectl delete -f service.yaml
$ kubectl delete -f deployment.yaml
```

### **************************************************************
### **************************************************************
### **************************************************************

## Module 8: Deploy Frontend

### Step 8.1: Replace http://localhost:5000 url with ELB Endpoint Ex. http://ac6502b4b6b2811e9b3e702e272af59b-1788422217.us-east-1.elb.amazonaws.com/
```
$ vi ~/environment/calculator-frontend/querycalc.js
```

### Step 8.2: Create an S3 Bucket for Storing Content
```
$ aws s3 mb s3://jrdalino-calculator-frontend
```

### Step 8.3: Create a CloudFront Access Identity
```
$ aws cloudfront create-cloud-front-origin-access-identity \
--cloud-front-origin-access-identity-config CallerReference=Calculator,Comment=Calculator
```

### Step 8.4: Create the S3 Bucket Policy Input File
```
$ mkdir aws-cli
$ vi ~/environment/calculator-frontend/aws-cli/website-bucket-policy.json
```
```
{
    "Version": "2008-10-17",
    "Id": "PolicyForCloudFrontPrivateContent",
    "Statement": [
        {
            "Sid": "1",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity EXZ8BOEUVCLQY"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::jrdalino-calculator-frontend/*"
        }
    ]
}
```

### Step 8.5: Add a public bucket policy to allow CloudFront
```
$ aws s3api put-bucket-policy \
--bucket jrdalino-calculator-frontend \
--policy file://~/environment/calculator-frontend/aws-cli/website-bucket-policy.json
```

### Step 8.6: Publish the Website Content to S3
```
$ cd ~/environment/calculator-frontend
$ aws s3 cp index.html s3://jrdalino-calculator-frontend/index.html
$ aws s3 cp base.css s3://jrdalino-calculator-frontend/base.css
$ aws s3 cp querycalc.js s3://jrdalino-calculator-frontend/querycalc.js
```

### Step 8.7: Create the CloudFront Distribution input file
```
$ vi ~/environment/calculator-frontend/aws-cli/website-cloudfront-distribution.json
```

```
{
  "CallerReference": "Calculator",
  "Aliases": {
    "Quantity": 0
  },
  "DefaultRootObject": "index.html",
  "Origins": {
    "Quantity": 1,
    "Items": [
      {
        "Id": "Calculator",
        "DomainName": "jrdalino-calculator-frontend.s3.amazonaws.com",
        "S3OriginConfig": {
          "OriginAccessIdentity": "origin-access-identity/cloudfront/EXZ8BOEUVCLQY"
        }
      }
    ]
  },
  "DefaultCacheBehavior": {
    "TargetOriginId": "Calculator",
    "ForwardedValues": {
      "QueryString": true,
      "Cookies": {
        "Forward": "none"
      }
    },
    "TrustedSigners": {
      "Enabled": false,
      "Quantity": 0
    },
    "ViewerProtocolPolicy": "allow-all",
    "MinTTL": 0,
    "MaxTTL": 0,
    "DefaultTTL": 0
  },
  "CacheBehaviors": {
    "Quantity": 0
  },
  "Comment": "",
  "Logging": {
    "Enabled": false,
    "IncludeCookies": true,
    "Bucket": "",
    "Prefix": ""
  },
  "PriceClass": "PriceClass_All",
  "Enabled": true
}
```

### Step 8.8: Create CloudFront Distribution
```
$ aws cloudfront create-distribution \
--distribution-config file://~/environment/calculator-frontend/aws-cli/website-cloudfront-distribution.json
```

### Step 8.9: Check Status of CloudFront Distribution
```
$ aws cloudfront list-distributions
```

### Step 8.10 Enable CORS on S3 and CloudFront
- Reference: https://aws.amazon.com/premiumsupport/knowledge-center/no-access-control-allow-origin-error/

### Step 8.11: Test functionality of Calculator Frontend + Backend
```
$ curl d5ny4mdta1kxt.cloudfront.net
```

### (Optional) Clean up
```
$ aws cloudfront delete-distribution --id E2KI3RD2QF7PXM
$ aws cloudfront delete-cloud-front-origin-access-identity --id EXZ8BOEUVCLQY
$ aws s3api delete-bucket --bucket jrdalino-calculator-frontend --region us-east-1
```

### **************************************************************
### **************************************************************
### **************************************************************

## Module 9: Setup CI/CD for Back End Service
- proper CI/CD processes to put in place

### Step 9.1: Create an S3 Bucket for Pipeline Artifacts
```
$ aws s3 mb s3://jrdalino-calculator-backend-artifacts
```

### Step 9.2: Create Codebuild and Codepipeline Role (eks-calculator-codebuild-codepipeline-iam-role)
```
$ cd ~/environment/calculator-backend
$ mkdir cfn
$ vi ~/environment/calculator-backend/cfn/eks-calculator-codebuild-codepipeline-iam-role.yaml
```
```
---
AWSTemplateFormatVersion: '2010-09-09'
Resources:

  # An IAM role that allows the AWS CodeBuild service to perform the actions
  # required to complete a build of our source code retrieved from CodeCommit,
  # and push the created image to ECR.

  CalculatorServiceCodeBuildServiceRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: CalculatorServiceCodeBuildServiceRole
      AssumeRolePolicyDocument:
        Version: "2012-10-17"
        Statement:
          Effect: Allow
          Principal:
            Service: codebuild.amazonaws.com
          Action: sts:AssumeRole
      Policies:
      - PolicyName: "CalculatorService-CodeBuildServicePolicy"
        PolicyDocument:
          Version: "2012-10-17"
          Statement:
          - Effect: "Allow"
            Action:
            - "codecommit:ListBranches"
            - "codecommit:ListRepositories"
            - "codecommit:BatchGetRepositories"
            - "codecommit:Get*"
            - "codecommit:GitPull"
            Resource:
            - Fn::Sub: arn:aws:codecommit:${AWS::Region}:${AWS::AccountId}:CalculatorServiceRepository
          - Effect: "Allow"
            Action:
            - "logs:CreateLogGroup"
            - "logs:CreateLogStream"
            - "logs:PutLogEvents"
            Resource: "*"
          - Effect: "Allow"
            Action:
            - "s3:PutObject"
            - "s3:GetObject"
            - "s3:GetObjectVersion"
            - "s3:ListBucket"
            Resource: "*"
          - Effect: "Allow"
            Action:
            - "ecr:GetAuthorizationToken"
            - "ecr:InitiateLayerUpload"
            - "ecr:UploadLayerPart"
            - "ecr:CompleteLayerUpload"
            - "ecr:BatchCheckLayerAvailability"
            - "ecr:PutImage"
            Resource: "*"

  # An IAM role that allows the AWS CodePipeline service to perform it's
  # necessary actions. We have intentionally left permissions on this role
  # that will not be used by the CodePipeline service during this workshop.
  # This will allow you to more simply use CodePipeline in the future should
  # you want to use the service for Pipelines that interact with different
  # AWS services than the ones used in this workshop.

  CalculatorServiceCodePipelineServiceRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: CalculatorServiceCodePipelineServiceRole
      AssumeRolePolicyDocument:
        Statement:
        - Effect: Allow
          Principal:
            Service:
            - codepipeline.amazonaws.com
          Action:
          - sts:AssumeRole
      Path: "/"
      Policies:
      - PolicyName: CalculatorService-codepipeline-service-policy
        PolicyDocument:
          Statement:
          - Action:
            - codecommit:GetBranch
            - codecommit:GetCommit
            - codecommit:UploadArchive
            - codecommit:GetUploadArchiveStatus
            - codecommit:CancelUploadArchive
            Resource: "*"
            Effect: Allow
          - Action:
            - s3:GetObject
            - s3:GetObjectVersion
            - s3:GetBucketVersioning
            Resource: "*"
            Effect: Allow
          - Action:
            - s3:PutObject
            Resource:
            - arn:aws:s3:::*
            Effect: Allow
          - Action:
            - elasticloadbalancing:*
            - autoscaling:*
            - cloudwatch:*
            - ecs:*
            - eks:*
            - codebuild:*
            - iam:PassRole
            Resource: "*"
            Effect: Allow
          Version: "2012-10-17"
```

```
$ aws cloudformation create-stack \
--stack-name eks-calculator-codebuild-codepipeline-iam-role \
--capabilities CAPABILITY_NAMED_IAM \
--template-body file://~/environment/calculator-backend/cfn/eks-calculator-codebuild-codepipeline-iam-role.yaml
```

### Step 9.3: Modify S3 Bucket Policy
Replace:
- REPLACE_ME_CODEBUILD_ROLE_ARN
- REPLACE_ME_CODEPIPELINE_ROLE_ARN
- ArtifactsBucketName

```
$ cd ~/environment/calculator-backend
$ mkdir aws-cli
$ vi ~/environment/calculator-backend/aws-cli/artifacts-bucket-policy.json
```

```
{
    "Statement": [
      {
        "Sid": "WhitelistedGet",
        "Effect": "Allow",
        "Principal": {
          "AWS": [
            "arn:aws:iam::707538076348:role/CalculatorServiceCodeBuildServiceRole",
            "arn:aws:iam::707538076348:role/CalculatorServiceCodePipelineServiceRole"
          ]
        },
        "Action": [
          "s3:GetObject",
          "s3:GetObjectVersion",
          "s3:GetBucketVersioning"
        ],
        "Resource": [
          "arn:aws:s3:::jrdalino-calculator-backend-artifacts/*",
          "arn:aws:s3:::jrdalino-calculator-backend-artifacts"
        ]
      },
      {
        "Sid": "WhitelistedPut",
        "Effect": "Allow",
        "Principal": {
          "AWS": [
            "arn:aws:iam::707538076348:role/CalculatorServiceCodeBuildServiceRole",
            "arn:aws:iam::707538076348:role/CalculatorServiceCodePipelineServiceRole"
          ]
        },
        "Action": "s3:PutObject",
        "Resource": [
          "arn:aws:s3:::jrdalino-calculator-backend-artifacts/*",
          "arn:aws:s3:::jrdalino-calculator-backend-artifacts"
        ]
      }
    ]
}
```

### Step 9.4: Grant S3 Bucket access to your CI/CD Pipeline
Replace:
- ArtifactsBucketName

```
$ aws s3api put-bucket-policy \
--bucket jrdalino-calculator-backend-artifacts \
--policy file://~/environment/calculator-backend/aws-cli/artifacts-bucket-policy.json
```

### Step 9.5: View/Modify Buildspec file
```
$ cd ~/environment/calculator-backend
$ vi ~/environment/calculator-backend/buildspec.yml
```

```
# A buildspec.yml file informs AWS CodeBuild of all the actions that should be
# taken during a build execution for our application. We are able to divide the
# build execution in separate pre-defined phases for logical organization, and
# list the commands that will be executed on the provisioned build server
# performing a build execution job.
version: 0.2

phases:
  pre_build:
    commands:
      - echo Logging in to Amazon ECR...
      # Retrieves docker credentials so that the subsequent docker push command is
      # authorized. Authentication is performed automatically by the AWS CLI
      # using the AWS credentials associated with the IAM role assigned to the
      # instances in your AWS CodeBuild project.
      - $(aws ecr get-login --no-include-email --region $AWS_DEFAULT_REGION)
  build:
    commands:
      - echo Build started on `date`
      - echo Building the Docker image...
      - docker build -t jrdalino/calculator-backend:latest .
      # Tag the built docker image using the appropriate Amazon ECR endpoint and relevant
      # repository for our service container. This ensures that when the docker push
      # command is executed later, it will be pushed to the appropriate repository.
      - docker tag jrdalino/calculator-backend:latest $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/jrdalino/calculator-backend:latest
  post_build:
    commands:
      - echo Build completed on `date`
      - echo Pushing the Docker image..
      # Push the image to ECR.
      - docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/jrdalino/calculator-backend:latest
      - echo Completed pushing Docker image. Deploying Docker image to AWS Fargate on `date`
      # Create a artifacts file that contains the name and location of the image
      # pushed to ECR. This will be used by AWS CodePipeline to automate
      # deployment of this specific container to Amazon ECS.
      - printf '[{"name":"jrdalino/calculator-backend","imageUri":"%s"}]' $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/jrdalino/calculator-backend:latest > imagedefinitions.json
artifacts:
  # Indicate that the created imagedefinitions.json file created on the previous
  # line is to be referenceable as an artifact of the build execution job.
  files: imagedefinitions.json
```

### Step 9.6: View/Modify CodeBuild Project Input File
```
$ vi ~/environment/calculator-backend/aws-cli/code-build-project.json
```

```
{
  "name": "CalculatorBackendServiceCodeBuildProject",
  "artifacts": {
    "type": "no_artifacts"
  },
  "environment": {
    "computeType": "BUILD_GENERAL1_SMALL",
    "image": "aws/codebuild/python:3.5.2",
    "privilegedMode": true,
    "environmentVariables": [
      {
        "name": "AWS_ACCOUNT_ID",
        "value": "707538076348"
      },
      {
        "name": "AWS_DEFAULT_REGION",
        "value": "us-east-1"
      }
    ],
    "type": "LINUX_CONTAINER"
  },
  "serviceRole": "arn:aws:iam::707538076348:role/CalculatorServiceCodeBuildServiceRole",
  "source": {
    "type": "CODECOMMIT",
    "location": "https://git-codecommit.us-east-1.amazonaws.com/v1/repos/calculator-backend"
  }
}
```

### Step 9.7: Create the CodeBuild Project
```
$ aws codebuild create-project \
--cli-input-json file://~/environment/calculator-backend/aws-cli/code-build-project.json
```

### Step 9.8 Setup Lambda for deployment
```
$ cd ~/environment/
$ git clone https://github.com/BranLiang/lambda-eks
$ cd lambda-eks
$ sed -i -e "s#\$EKS_CA#$(aws eks describe-cluster --name calculator-eksctl --query cluster.certificateAuthority.data --output text)#g" ./config
$ sed -i -e "s#\$EKS_CLUSTER_HOST#$(aws eks describe-cluster --name calculator-eksctl --query cluster.endpoint --output text)#g" ./config
$ sed -i -e "s#\$EKS_CLUSTER_NAME#calculator-eksctl#g" ./config
$ sed -i -e "s#\$EKS_CLUSTER_USER_NAME#lambda#g" ./config
$ kubectl get secrets
```

### Step 9.9: Then run the following command replacing secret name to update your token
sed -i -e "s#\$TOKEN#$(kubectl get secret $SECRET_NAME -o json | jq -r '.data["token"]' | base64 -d)#g" ./config

### Step 9.10: Build,Package and deploy the Lambda Kube Client Function
```
$ npm install
$ zip -r ../lambda-package_v1.zip .
$ cd ..
$ export LAMBDA_SERVICE_ROLE=$(aws cloudformation describe-stacks --stack-name $AWS_MASTER_STACK | jq -r '.Stacks[0].Outputs[]|select(.OutputKey=="LambdaExecutionRoleArn")|.OutputValue')
$ aws lambda create-function --function-name LambdaKubeClient --runtime nodejs8.10 --role $LAMBDA_SERVICE_ROLE --handler index.handler  --zip-file fileb://lambda-package_v1.zip --timeout 10 --memory-size 128
```

### Step 9.11: Providing admin access for default service account
```
$ kubectl create clusterrolebinding default-admin --clusterrole cluster-admin --serviceaccount=default:default
```

### Step 9.12: Modify CodePipeline Input File
```
$ vi ~/environment/calculator-backend/aws-cli/code-pipeline.json
```

```
{
  "pipeline": {
      "name": "CalculatorBackendServiceCICDPipeline",
      "roleArn": "arn:aws:iam::707538076348:role/CalculatorServiceCodePipelineServiceRole",
      "stages": [
        {
          "name": "Source",
          "actions": [
            {
              "inputArtifacts": [
    
              ],
              "name": "Source",
              "actionTypeId": {
                "category": "Source",
                "owner": "AWS",
                "version": "1",
                "provider": "CodeCommit"
              },
              "outputArtifacts": [
                {
                  "name": "CalculatorBackendService-SourceArtifact"
                }
              ],
              "configuration": {
                "BranchName": "master",
                "RepositoryName": "calculator-backend"
              },
              "runOrder": 1
            }
          ]
        },
        {
          "name": "Build",
          "actions": [
            {
              "name": "Build",
              "actionTypeId": {
                "category": "Build",
                "owner": "AWS",
                "version": "1",
                "provider": "CodeBuild"
              },
              "outputArtifacts": [
                {
                  "name": "CalculatorBackendService-BuildArtifact"
                }
              ],
              "inputArtifacts": [
                {
                  "name": "CalculatorBackendService-SourceArtifact"
                }
              ],
              "configuration": {
                "ProjectName": "CalculatorBackendServiceCodeBuildProject"
              },
              "runOrder": 1
            }
          ]
        }
      ],
      "artifactStore": {
        "type": "S3",
        "location": "jrdalino-calculator-backend-artifacts"
      }
  }
}
```

### Step 9.13: Create a pipeline in CodePipeline
```
$ aws codepipeline create-pipeline \
--cli-input-json file://~/environment/calculator-backend/aws-cli/code-pipeline.json
```

### Step 9.14: Modify ECR Policy
Replace: CodeBuild Role ARN

```
$ vi ~/environment/calculator-backend/aws-cli/ecr-policy.json
```

```
{
  "Statement": [
    {
      "Sid": "AllowPushPull",
      "Effect": "Allow",
      "Principal": {
        "AWS": [
         "arn:aws:iam::707538076348:role/CalculatorServiceCodeBuildServiceRole"
        ]
      },
      "Action": [
        "ecr:GetDownloadUrlForLayer",
        "ecr:BatchGetImage",
        "ecr:BatchCheckLayerAvailability",
        "ecr:PutImage",
        "ecr:InitiateLayerUpload",
        "ecr:UploadLayerPart",
        "ecr:CompleteLayerUpload"
      ]
    }
  ]
}
```

### Step 9.15: Enable automated Access to the ECR Image Repository
```
$ aws ecr set-repository-policy \
--repository-name jrdalino/calculator-backend \
--policy-text file://~/environment/calculator-backend/aws-cli/ecr-policy.json
```

### Step 9.16: Make a small code change, push and validate changes

# ************************************************************
# ************************************************************
# ************************************************************

## Module 10: Setup CI/CD for Front End Service

### Step 10.1: Create an S3 Bucket for Pipeline Artifacts
```
$ aws s3 mb s3://jrdalino-calculator-frontend-artifacts
```

### Step 10.2: Check Codepipeline Roles exists

### Step 10.3: Create S3 Bucket Policy File
```
$ cd ~/environment/calculator-frontend
$ mkdir aws-cli
$ vi ~/environment/calculator-frontend/aws-cli/artifacts-bucket-policy.json
```

```
{
    "Statement": [
      {
        "Sid": "WhitelistedGet",
        "Effect": "Allow",
        "Principal": {
          "AWS": [
            "arn:aws:iam::707538076348:role/CalculatorServiceCodePipelineServiceRole"
          ]
        },
        "Action": [
          "s3:GetObject",
          "s3:GetObjectVersion",
          "s3:GetBucketVersioning"
        ],
        "Resource": [
          "arn:aws:s3:::jrdalino-calculator-frontend-artifacts/*",
          "arn:aws:s3:::jrdalino-calculator-frontend-artifacts"
        ]
      },
      {
        "Sid": "WhitelistedPut",
        "Effect": "Allow",
        "Principal": {
          "AWS": [
            "arn:aws:iam::707538076348:role/CalculatorServiceCodePipelineServiceRole"
          ]
        },
        "Action": "s3:PutObject",
        "Resource": [
          "arn:aws:s3:::jrdalino-calculator-frontend-artifacts/*",
          "arn:aws:s3:::jrdalino-calculator-frontend-artifacts"
        ]
      }
    ]
}
```

### Step 10.4: Grant S3 Bucket access to your CI/CD Pipeline
```
$ aws s3api put-bucket-policy \
--bucket jrdalino-calculator-frontend-artifacts \
--policy file://~/environment/calculator-frontend/aws-cli/artifacts-bucket-policy.json
```

### Step 10.5: Create CodePipeline Input File
```
$ vi ~/environment/calculator-frontend/aws-cli/code-pipeline.json
```

```
{
  "pipeline": {
      "name": "CalculatorFrontendServiceCICDPipeline",
      "roleArn": "arn:aws:iam::707538076348:role/CalculatorServiceCodePipelineServiceRole",
      "stages": [
        {
          "name": "Source",
          "actions": [
            {
              "inputArtifacts": [
    
              ],
              "name": "Source",
              "actionTypeId": {
                "category": "Source",
                "owner": "AWS",
                "version": "1",
                "provider": "CodeCommit"
              },
              "outputArtifacts": [
                {
                  "name": "CalculatorFrontendService-SourceArtifact"
                }
              ],
              "configuration": {
                "BranchName": "master",
                "RepositoryName": "calculator-frontend"
              },
              "runOrder": 1
            }
          ]
        },
        {
          "name": "Deploy",
          "actions": [
            {
              "name": "Deploy",
              "actionTypeId": {
                "category": "Deploy",
                "owner": "AWS",
                "version": "1",
                "provider": "S3"
              },
              "inputArtifacts": [
                {
                  "name": "CalculatorFrontendService-SourceArtifact"
                }
              ],
              "configuration": {
                  "Extract": "true", 
                  "BucketName": "jrdalino-calculator-frontend"
              }
            }
          ]
        }
      ],
      "artifactStore": {
        "type": "S3",
        "location": "jrdalino-calculator-frontend-artifacts"
      }
  }
}
```

### Step 10.3: Create a pipeline in CodePipeline
```
$ aws codepipeline create-pipeline \
--cli-input-json file://~/environment/calculator-frontend/aws-cli/code-pipeline.json
```

### Step 10.4: Make a small code change, Push and Validate changes

# ************************************************************
# ************************************************************
# ************************************************************

## Module 11: Install Helm

### Step 11.1: Install Helm CLI
```
$ cd ~/environment
$ curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh
$ chmod +x get_helm.sh
$ ./get_helm.sh
```

### Step 11.2: Configure Helm access with RBAC
```
cat <<EoF > ~/environment/rbac.yaml
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
EoF
```

### Step 11.3: Apply the config
```
$ kubectl apply -f ~/environment/rbac.yaml
```

### Step 11.4: Install helm and tiller into the cluster which gives it access to manage resources in your cluster.
```
$ helm init --service-account tiller
```

# ************************************************************
# ************************************************************
# ************************************************************

## Module 12: Deploy Prometheus for basic monitoring

### Step 12.1: Install Prometheus
```
$ kubectl create namespace prometheus
$ helm install stable/prometheus \
    --name prometheus \
    --namespace prometheus \
    --set alertmanager.persistentVolume.storageClass="gp2" \
    --set server.persistentVolume.storageClass="gp2"
```

### Step 12.2: Check if Prometheus components deployed as expected
```
$ kubectl get all -n prometheus
```

### Step 12.3: Access the Prometheus server URL w/ kubectl port-forward and access /targets Web UI
```
$ kubectl port-forward -n prometheus deploy/prometheus-server 8080:9090
```

### (Optional) Clean up
```
$ helm delete prometheus
$ helm del --purge prometheus
```

# ************************************************************
# ************************************************************
# ************************************************************

## Module 13: Deploy Grafana to create Dashboards

### Step 13.1: Install Grafana
```
$ kubectl create namespace grafana
$ helm install stable/grafana \
    --name grafana \
    --namespace grafana \
    --set persistence.storageClassName="gp2" \
    --set adminPassword="EKS!sAWSome" \
    --set datasources."datasources\.yaml".apiVersion=1 \
    --set datasources."datasources\.yaml".datasources[0].name=Prometheus \
    --set datasources."datasources\.yaml".datasources[0].type=prometheus \
    --set datasources."datasources\.yaml".datasources[0].url=http://prometheus-server.prometheus.svc.cluster.local \
    --set datasources."datasources\.yaml".datasources[0].access=proxy \
    --set datasources."datasources\.yaml".datasources[0].isDefault=true \
    --set service.type=LoadBalancer
```

### Step 13.2: Check if Grafana is deployed
```
$ kubectl get all -n grafana
```

### Step 13.3: Get Grafana ELB URL
```
$ export ELB=$(kubectl get svc -n grafana grafana -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')
$ echo "http://$ELB"
```

### Step 13.4: Login using admin and password
```
$ kubectl get secret --namespace grafana grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

### Step 13.5: Create Grafana Dashboards

### (Optional) Clean up
```
$ helm delete grafana
$ helm del --purge grafana
```

# ************************************************************
# ************************************************************
# ************************************************************

## Module 14: Implement Liveness Probe Health Checks
- The API being a crucial part of the application it needs to be highly available

### Step 14.1: Configure the Probe
```
$ mkdir -p ~/environment/healthchecks
$ cat <<EoF > ~/environment/healthchecks/liveness-app.yaml
apiVersion: v1
kind: Pod
metadata:
  name: liveness-app
spec:
  containers:
  - name: liveness
    image: jrdalino/calculator-backend
    livenessProbe:
      httpGet:
        path: /health
        port: 5000
      initialDelaySeconds: 5
      periodSeconds: 5
EoF
```

### Step 14.2: Create the pod using the manifest
```
$ kubectl apply -f ~/environment/healthchecks/liveness-app.yaml
$ kubectl get pod liveness-app
$ kubectl describe pod liveness-app
```

### Step 14.3: Introduce a Failure to Test
```
$ kubectl exec -it liveness-app -- /bin/kill -s SIGUSR1 1
$ kubectl get pod liveness-app
$ kubectl logs liveness-app
$ kubectl logs liveness-app --previous
```

### (Optional) Clean Up
```
$ kubectl delete -f ~/environment/healthchecks/liveness-app.yaml
```

# ************************************************************
# ************************************************************
# ************************************************************

## Module 15: Implement Readiness Probe Health Checks
- The API being a crucial part of the application it needs to be highly available

### Step 15.1: Configure the Probe
```
$ cat <<EoF > ~/environment/healthchecks/readiness-deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: readiness-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: readiness-deployment
  template:
    metadata:
      labels:
        app: readiness-deployment
    spec:
      containers:
      - name: readiness-deployment
        image: alpine
        command: ["sh", "-c", "touch /tmp/healthy && sleep 86400"]
        readinessProbe:
          exec:
            command:
            - cat
            - /tmp/healthy
          initialDelaySeconds: 5
          periodSeconds: 3
EoF
```

### Step 15.2: Create a deployment to test readiness probe
```
$ kubectl apply -f ~/environment/healthchecks/readiness-deployment.yaml
$ kubectl get pods -l app=readiness-deployment
```

### Step 15.3: Confirm that all the replicas are available to serve traffic when a service is pointed to this deployment
```
$ kubectl describe deployment readiness-deployment | grep Replicas:
```

### Step 15.4: Introduce a Failure
```
$ kubectl exec -it <YOUR-READINESS-POD-NAME> -- rm /tmp/healthy
$ kubectl get pods -l app=readiness-deployment
$ kubectl describe deployment readiness-deployment | grep Replicas:
```

### Step 15.5: Restore pod to Ready Status
```
$ kubectl exec -it <YOUR-READINESS-POD-NAME> -- touch /tmp/healthy
$ kubectl get pods -l app=readiness-deployment
```

### (Optional) Clean Up
```
$ kubectl delete -f ~/environment/healthchecks/readiness-deployment.yaml
```

# ************************************************************
# ************************************************************
# ************************************************************

## Module 16: Implementing Auto Scaling

### Step 16.1: Configure Horizontal Pod AutoScaler (HPA) - Deploy the Metrics Server
```
$ helm install stable/metrics-server \
    --name metrics-server \
    --version 2.0.4 \
    --namespace metrics
$ kubectl get apiservice v1beta1.metrics.k8s.io -o yaml
```

### Step 16.2: Scale an Application with Horizontal Pod AutoScaler (HPA)
- Deploy a Sample App
```
kubectl run php-apache --image=k8s.gcr.io/hpa-example --requests=cpu=200m --expose --port=80
```

- Create an HPA Resource
```
$ kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10
$ kubectl get hpa
```

- Generate load to trigger scaling
```
$ kubectl run -i --tty load-generator --image=busybox /bin/sh
$ while true; do wget -q -O - http://php-apache; done
$ kubectl get hpa -w
```

### Step 16.3: Configure Cluster AutoScaler (CA)
- Configure the Cluster Autoscaler (CA)
```
$ mkdir ~/environment/cluster-autoscaler
$ cd ~/environment/cluster-autoscaler
$ wget https://eksworkshop.com/scaling/deploy_ca.files/cluster_autoscaler.yml
```

- Configure the ASG Min: 2 and Max: 8

- Configure the Cluster Autoscaler
- Open https://eksworkshop.com/scaling/deploy_ca.files/cluster_autoscaler.yml and change <AUTOSCALING GROUP NAME>, AWS_REGION and minimum nodes (2) and maximum nodes (8) and ASG Name

- Create an IAM Policy
```
$ test -n "$ROLE_NAME" && echo ROLE_NAME is "$ROLE_NAME" || echo ROLE_NAME is not set
```

```
$ mkdir ~/environment/asg_policy
cat <<EoF > ~/environment/asg_policy/k8s-asg-policy.json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "autoscaling:DescribeAutoScalingGroups",
        "autoscaling:DescribeAutoScalingInstances",
        "autoscaling:SetDesiredCapacity",
        "autoscaling:TerminateInstanceInAutoScalingGroup"
      ],
      "Resource": "*"
    }
  ]
}
EoF
$ aws iam put-role-policy --role-name $ROLE_NAME --policy-name ASG-Policy-For-Worker --policy-document file://~/environment/asg_policy/k8s-asg-policy.json
$ aws iam get-role-policy --role-name $ROLE_NAME --policy-name ASG-Policy-For-Worker
```

- Deploy the Cluster Autoscaler
```
$ kubectl apply -f ~/environment/cluster-autoscaler/cluster_autoscaler.yml
$ kubectl logs -f deployment/cluster-autoscaler -n kube-system
```

### Step 16.4: Scale a Cluster with Cluster Auto Scaler
- Deploy a Sample App
```
cat <<EoF> ~/environment/cluster-autoscaler/nginx.yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: nginx-to-scaleout
spec:
  replicas: 1
  template:
    metadata:
      labels:
        service: nginx
        app: nginx
    spec:
      containers:
      - image: nginx
        name: nginx-to-scaleout
        resources:
          limits:
            cpu: 500m
            memory: 512Mi
          requests:
            cpu: 500m
            memory: 512Mi
EoF
kubectl apply -f ~/environment/cluster-autoscaler/nginx.yaml
kubectl get deployment/nginx-to-scaleout
```

- Scale our ReplicaSet to 10
```
$ kubectl scale --replicas=10 deployment/nginx-to-scaleout
$ kubectl get pods -o wide --watch
$ kubectl logs -f deployment/cluster-autoscaler -n kube-system
```

### (Optional) Clean Up
```
$ kubectl delete -f ~/environment/cluster-autoscaler/cluster_autoscaler.yml
$ kubectl delete -f ~/environment/cluster-autoscaler/nginx.yaml
$ kubectl delete hpa,svc php-apache
$ kubectl delete deployment php-apache load-generator
$ rm -rf ~/environment/cluster-autoscaler
```

# ************************************************************
# ************************************************************
# ************************************************************

## Module 17: Logging with Elasticache, Fluentd, and Kibana (EFK)
- Daily/weekly/monthly report showing the operations that have been performed during that time period

### Step 17.1: Configure IAM Policy for Worker Nodes
```
$ test -n "$ROLE_NAME" && echo ROLE_NAME is "$ROLE_NAME" || echo ROLE_NAME is not set
```

```
$ mkdir ~/environment/iam_policy
cat <<EoF > ~/environment/iam_policy/k8s-logs-policy.json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "logs:DescribeLogGroups",
                "logs:DescribeLogStreams",
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
EoF

$ aws iam put-role-policy --role-name $ROLE_NAME --policy-name Logs-Policy-For-Worker --policy-document file://~/environment/iam_policy/k8s-logs-policy.json
```

```
$ aws iam get-role-policy --role-name $ROLE_NAME --policy-name Logs-Policy-For-Worker
```

### Step 17.2: Provision an Elasticsearch Cluster
```
$ aws es create-elasticsearch-domain \
  --domain-name kubernetes-logs \
  --elasticsearch-version 6.3 \
  --elasticsearch-cluster-config \
  InstanceType=t2.small.elasticsearch,InstanceCount=1 \
  --ebs-options EBSEnabled=true,VolumeType=standard,VolumeSize=20 \
  --access-policies '{"Version":"2012-10-17","Statement":[{"Effect":"Allow","Principal":{"AWS":["*"]},"Action":["es:*"],"Resource":"*"}]}'
```

```
$ aws es describe-elasticsearch-domain --domain-name kubernetes-logs --query 'DomainStatus.Processing'
```

### Step 17.3: Deploy Fluentd
- Replace REGION and CLUSTER_NAME

```
$ mkdir ~/environment/fluentd
$ cd ~/environment/fluentd
$ wget https://eksworkshop.com/logging/deploy.files/fluentd.yml
$ kubectl apply -f ~/environment/fluentd/fluentd.yml
$ kubectl get pods -w --namespace=kube-system
```

### Step 17.4: Configure CloudWatch Logs
```
$ cat <<EoF > ~/environment/iam_policy/lambda.json
{
   "Version": "2012-10-17",
   "Statement": [
   {
     "Effect": "Allow",
     "Principal": {
        "Service": "lambda.amazonaws.com"
     },
   "Action": "sts:AssumeRole"
   }
 ]
}
EoF

$ aws iam create-role --role-name lambda_basic_execution --assume-role-policy-document file://~/environment/iam_policy/lambda.json

$ aws iam attach-role-policy --role-name lambda_basic_execution --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
```

- CloudWatch Logs Console
- Select the log group /eks/calculator-eksctl/containers. 
- Click on Actions and select Stream to Amazon ElasticSearch Service.
- Select the ElasticSearch Cluster kubernetes-logs and IAM role lambda_basic_execution
- Click Next
- Select Common Log Format and click Next
- Review the configuration. Click Next and then Start Streaming

### Step 17.5 Configure Kibana
- In Amazon Elasticsearch console, select the kubernetes-logs under My domains
- Open the Kibana dashboard from the link. After a few minutes, records will begin to be indexed by ElasticSearch. 
- You’ll need to configure an index patterns in Kibana.
- Set Index Pattern as cwl-* and click Next
- Select @timestamp from the dropdown list and select Create index pattern
- Click on Discover and explore your logs

### (Optional) Clean Up
```
$ cd ~/environment
$ kubectl delete -f ~/environment/fluentd/fluentd.yml
$ rm -rf ~/environment/fluentd/
$ aws es delete-elasticsearch-domain --domain-name kubernetes-logs
$ aws logs delete-log-group --log-group-name /eks/eksworkshop-eksctl/containers
$ rm -rf ~/environment/iam_policy/
```

# ************************************************************
# ************************************************************
# ************************************************************

## Module 18: Add additional feature
- Additional killer feature of your choice
