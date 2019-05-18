# Python Flask Backend + JS Frontend on AWS EKS (Kubernetes) with CI/CD, Monitoring and Logging
This is a simple tutorial on how to create a Python Flask Backend + Javascript Frontend Application, Setup CI/CD with AWS Code% and AWS Elastic Kubernetes Service, High Availability, Autoscaling, Monitoring using Prometheus and Grafana and Logging with ElasticSearch, Fluentd and Kibana.

## Prerequisites
- Working Directory
```
$ cd ~
$ mkdir environment
$ cd ~/environment
```

- Homebrew
```
$ brew --version
```

- AWS CLI
```
$ aws --version
$ aws configure
```

- Git
```
$ git --version
$ git config --global user.name "REPLACE_ME_WITH_YOUR_NAME"
$ git config --global user.email REPLACE_ME_WITH_YOUR_EMAIL@example.com
$ git config --global credential.helper '!aws codecommit credential-helper $@'
$ git config --global credential.UseHttpPath true
```

- Python
```
$ python3 --version
Python 3.7.3
```

- Virtualenv
```
$ virtualenv --version
16.4.3
$ echo 'venv' > .gitignore
$ python3 -m venv venv
$ source venv/bin/activate
(venv) $
(venv) $ deactivate
```

- Flask
```
(venv) $ pip install flask
(venv) $ flask --version
Flask 1.0.2
Python 3.7.3 (default, Mar 27 2019, 09:23:15) 
[Clang 10.0.1 (clang-1001.0.46.3)]
```

- Docker
```
$ docker -v
Docker version 18.09.2, build 6247962
```

## Step 1: Create Backend Python Flask RestAPI
- https://github.com/jrdalino/myproject-product-restapi
- https://github.com/jrdalino/myproject-consumer-restapi
- https://github.com/jrdalino/myproject-provider-restapi
- https://github.com/jrdalino/myproject-order-restapi
- https://github.com/jrdalino/myproject-billing-restapi
- https://github.com/jrdalino/myproject-payment-restapi
- https://github.com/jrdalino/myproject-notification-restapi

## Step 2: Create Frontend HTML CSS JS 
- https://github.com/jrdalino/myproject-consumer-web
- https://github.com/jrdalino/myproject-provider-web

## Step 3: AWS Organizations and Multi-Account Hardening
- https://github.com/jrdalino/aws-organizations-multi-account-hardening

## (TODO) Step 4: Add Infrastructure as Code Templates (Custom VPC with Public and Private Subnets) w/ Testing
- Terraform: https://github.com/jrdalino/infra-as-code-terraform
- Cloudformation: https://github.com/jrdalino/infra-as-code-aws-cloudformation

## (TODO) Step 5: Setup CI/CD for Infrastructure as Code Templates
- Terraform: https://dzone.com/articles/immutable-infrastructure-cicd-using-hashicorp-terr
- Cloudformation: https://github.com/aws-quickstart/quickstart-taskcat-ci

## Step 6: Install Kubernetes Tools and Launch EKS using EKCTL
- EKS: https://github.com/jrdalino/aws-eks-eksctl
- K8S: Example > install single-node master K8s cluster https://github.com/jrdalino/single-master-kubernetes-cluster
- K8S KOPS: Example > install a multi-node master K8s cluster https://github.com/jrdalino/multi-master-kubernetes-cluster-kops
- ECS: https://github.com/jrdalino/aws-ecs

## Step 7: Deploy Backend MicroService to EKS
- EKS: https://github.com/jrdalino/backend-deploy-to-kubernetes
- ECS: https://github.com/jrdalino/backend-deploy-to-ecs-fargate

## Step 8: Setup CI/CD for Back End Service
- EKS: https://github.com/jrdalino/backend-cicd-codecommit-codebuild-codepipeline-kubernetes
- ECS: https://github.com/jrdalino/backend-cicd-codecommit-codebuild-codepipeline-ecs

## (TODO) Step 9: Add Authentication using Amazon Cognito
- https://github.com/jrdalino/amazon-cognito

## (TODO) Step 10: Add API Gateway in front of EKS Endpoint
- https://github.com/jrdalino/amazon-api-gateway

## (TODO) Step 11: Add Web Application Firewall in front of API Gateway
- AWS Web Application Firewall
- AWS Shield
- https://github.com/aws-samples/aws-waf-sample/blob/master/waf-owasp-top-10/owasp_10_base.yml

## (TODO) Step 12: Backend Static Testing (Static Code Analyzer)
- Find OSS alternative for SonarQube / Fortify / Checkmarx

## (TODO) Step 13: Backend Dynamic Testing (Containers Scanning)
- Find OSS alternative for  Anchore Engine / Aqua Microscanner / Clair / Dagda / Twistlock

## Step 14: Deploy Frontend
- https://github.com/jrdalino/frontend-deploy-to-s3-cloudfront

## Step 15: Setup CI/CD for Front End
- https://github.com/jrdalino/frontend-cicd-codecommit-codepipeline

## (TODO) Step 16: Web Application Firewall in front of CloudFront CDN
- AWS Web Application Firewall and AWS Shield
- https://github.com/aws-samples/aws-waf-sample/blob/master/waf-owasp-top-10/owasp_10_base.yml

## (TODO) Step 17: Frontend Static Testing (Packages)
- AuditJS > https://www.npmjs.com/package/auditjs
- RetireJS > https://retirejs.github.io/retire.js > Checks npm packages and compares to CVE's
- OWASP Dependency Check > https://www.owasp.org/index.php/OWASP_Dependency_Check

## (TODO) Step 18: Frontend Dynamic Testing (OWASP)
- OWASP ZAP Web App Pentest Tool > https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project

## (TODO) Step 19: Database - NoSQL (DynamoDB)
- https://github.com/jrdalino/amazon-dynamodb

## (TODO) Step 20: Caching - DynamoDB Accelerator (DAX)

## (TODO) Step 21: Database - RDBMS (RDS Postgres)

## (TODO) Step 22: Caching - ElastiCache (Redis)

## (TODO) Step 23: Check for Commited Secrets on Git Repository
- https://github.com/awslabs/git-secrets
- https://github.com/zricethezav/gitleaks

## (TODO) Step 24: Security - AWS Secrets Management
- AWS Secrets Manager: https://github.com/jrdalino/aws-secrets-manager
- AWS Parameter Store / Hashicorp Vault

## (TODO) Step 25: SSL/TLS using AWS Certificate Manager
- https://github.com/jrdalino/aws-certificate-manager

## (TODO) Step 26: Register/Transfer Domain Name using Route 53 for CloudFront CDN

## Step 27: Install Helm
- https://github.com/jrdalino/install-helm

## Step 28: Deploy Prometheus for basic monitoring
- https://github.com/jrdalino/install-prometheus-using-helm

## Step 29: Deploy Grafana to create Dashboards
- https://github.com/jrdalino/install-grafana-using-helm

## Step 30: Implement K8s Liveness Probe Health Checks
- https://github.com/jrdalino/kubernetes-liveness-probes

## Step 31: Implement K8s Readiness Probe Health Checks
- https://github.com/jrdalino/kubernetes-readiness-probes

## Step 32: Implementing K8s Auto Scaling
- https://github.com/jrdalino/kubernetes-autoscaling

## Step 33: Logging with ElastiSearch, Fluentd, and Kibana (EFK)
- https://github.com/jrdalino/elasticsearch-fluentd-kibana

## (TODO) Step 34: Instrumentation using AWS X-Ray
- AWS X-Ray / Prometheus

## (TODO) Step 35: Alerting using AlertManager
- AlertManager
- PagerDuty
- Slack

## (TODO) Step 36: Service Discovery using Consul
- Hashicorp Consul / Netflix Eureka / AWS CloudMap

## (TODO) Step 37: Service Mesh using Istio
- Istio / AWS Appmesh
