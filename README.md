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

### Modile 1.1 Create and Navigate to Directory
```
$ mkdir calculator-rest-api
$ cd calculator-rest-api
$ virtualenv flask
$ flask/bin/pip install flask
```

### Module 1.2 Add calculator.py
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

### Module 1.3 Create the requirements.txt file
```
$ vi requirements.txt
```
```
flask
```

### Module 1.4 Run Locally and Test
```
$ chmod a+x calculator.py
$ ./calculator.py
$ curl http://localhost:5000
```

### Module 1.5 Create the Dockerfile
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

### Module 1.6 Build and Run Locally
```
$ docker build -t calculator-rest-api:latest .
$ docker run -d -p 5000:5000 calculator-rest-api:latest
```

### Module 1.7 Test Math Operations
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

- Test Mulitply
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

- TODO: Test Square Root
- TODO: Test Cube Root

- Test Power
```
$ curl -i -H "Content-Type: application/json" -X POST -d '{"argument1":2, "argument2":3 }' http://localhost:5000/exp
{
  "answer": 8
}
```

- TODO: Test Factorial

## Module 2: TODO - Backend Unit Tests
- References: http://liangshang.github.io/2014/01/17/a-simple-calculator-by-python-and-tdd

## Module 3: Frontend HTML, CSS, JS and Bootstrap for Calculator Local
- Calculator triggered by end users through a web page

### Modile 3.1 Create and Navigate to Directory
```
$ mkdir calculator-frontend
$ cd calculator-frontend
$ mkdir css
$ mkdir js
$ touch index.html
$ cd css
$ touch base.css
$ cd ..
$ cd js
$ touch querycalc.js
```

### Module 3.2 Create index.html file
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
                    <input class= "func-btn" type="button" id="exp" value="Exponent"/>
                </div>
            </div>
            </form>
        </div>

        <link rel="stylesheet" type="text/css" href="./css/base.css">
        <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
        <script src="./js/querycalc.js"></script>
        <script type="text/javascript">
            // once the DOM is ready start the calculator listener function
            $( document ).ready( calcListener ); 
        </script>

    </body>
</html>
```

### Module 3.2 Create base.css file
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

### Module 3.3 Create querycalc.js file
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
    
    $( "#exp" ).click( function ( e ) {
        var arg1 = $( "#argument1" ).val();
        var arg2 = $( "#argument2" ).val();
        doMath (arg1, arg2, 'exp');
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
                url: resource,
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

### Module 3.4 Add default.conf file
```
$ vi default.conf
```
```
server {
    listen       80;
    server_name  localhost;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

### Module 3.5 Create the Docker File
```
$ vi Dockerfile
```
```
FROM nginx:alpine
COPY default.conf /etc/nginx/conf.d/default.conf
COPY . /usr/share/nginx/html
```

### Module 3.6 Build and Run Locally
```
$ docker build -t calculator-frontend:latest .
$ docker run -d -p 8080:80 calculator-frontend:latest
```

### Module 3.7 Test User Interface
```
$ curl http://localhost:8080
```

## Module 4: TODO - Frontend Unit Tests

## Module 5: Push Backend and Frontend to ECR

### Module 5.1

## Module 6: Create Amazon EKS Cluster VPC using Cloudformation
- TODO: Use Terraform https://dzone.com/articles/amazon-aws-eks-and-rds-postgresql-with-terraform-i
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
- Reference: https://eksworkshop.com/codepipeline/

### Module 10.1: Create IAM role
```
$ ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
$ TRUST="{ \"Version\": \"2012-10-17\", \"Statement\": [ { \"Effect\": \"Allow\", \"Principal\": { \"AWS\": \"arn:aws:iam::${ACCOUNT_ID}:root\" }, \"Action\": \"sts:AssumeRole\" } ] }"
$ echo '{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Action": "eks:Describe*", "Resource": "*" } ] }' > /tmp/iam-role-policy
$ aws iam create-role --role-name EksWorkshopCodeBuildKubectlRole --assume-role-policy-document "$TRUST" --output text --query 'Role.Arn'
$ aws iam put-role-policy --role-name EksWorkshopCodeBuildKubectlRole --policy-name eks-describe --policy-document file:///tmp/iam-role-policy
```

### Module 10.2: Modify AWS-Auth Config Map
```
$ ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
$ ROLE="    - rolearn: arn:aws:iam::$ACCOUNT_ID:role/EksWorkshopCodeBuildKubectlRole\n      username: build\n      groups:\n        - system:masters"
$ kubectl get -n kube-system configmap/aws-auth -o yaml | awk "/mapRoles: \|/{print;print \"$ROLE\";next}1" > /tmp/aws-auth-patch.yml
$ kubectl patch configmap/aws-auth -n kube-system --patch "$(cat /tmp/aws-auth-patch.yml)"
```

### Module 10.3 Setup CodePipeline
- TODO: Replace Github with CodeCommit
```
Execute CloudFormation Template - https://s3.amazonaws.com/eksworkshop.com/templates/master/ci-cd-codepipeline.cfn.yml
```

### Module 10.4 Review
```
$ kubectl describe deployment hello-k8s
$ kubectl describe service hello-k8s
```

### Module 10.5 Trigger New Release


## Module 11: Setup CI/CD for Back End
- Same as Module 10.

## Module 12: Install Helm

### Module 12.1: Install Helm CLI
```
$ cd ~/environment
$ curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh
$ chmod +x get_helm.sh
$ ./get_helm.sh
```

### Module 12.2: Configure Helm access with RBAC
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

### Module 12.3: Apply the config
```
$ kubectl apply -f ~/environment/rbac.yaml
```

### Module 12.4: Install helm and tiller into the cluster which gives it access to manage resources in your cluster.
```
$ helm init --service-account tiller
```

## Module 13: Deploy Prometheus
- Basic monitoring
- References: https://eksworkshop.com/monitoring/

### Module 13.1: Install Prometheus
```
$ kubectl create namespace prometheus
$ helm install stable/prometheus \
    --name prometheus \
    --namespace prometheus \
    --set alertmanager.persistentVolume.storageClass="gp2" \
    --set server.persistentVolume.storageClass="gp2"
```

### Module 13.2: Check if Prometheus components deployed as expected
```
$ kubectl get all -n prometheus
```

### Module 13.3: Access the Prometheus server URL w/ kubectl port-forward and access /targets Web UI
```
$ kubectl port-forward -n prometheus deploy/prometheus-server 8080:9090
```

## Module 14: Deploy Grafana
- References: https://eksworkshop.com/monitoring/deploy-grafana/
### Module 14.1:
```
$
```

### Module 14.2: 
```
$ 
```

## Module 15: Implement Health Checks
- The API being a crucial part of the application it needs to be highly available
- References: https://eksworkshop.com/healthchecks/

## Module 16: Implementing Auto Scaling
- References: https://eksworkshop.com/scaling/

## Module 17: Log Amazon EKS API Calls with CloudTrail
- References: https://docs.aws.amazon.com/eks/latest/userguide/logging-using-cloudtrail.html

## Module 18: Log REST operations performed to S3
- Reference: https://github.com/aws-samples/aws-modern-application-workshop/tree/python/module-5 ?

## Module 19: S3 and Athena Report
- Daily/weekly/monthly report showing the operations that have been performed during that time period
- Refences: https://aws.amazon.com/blogs/big-data/analyzing-data-in-s3-using-amazon-athena/

## Module 20: Add additional feature
- Additional killer feature of your choice
