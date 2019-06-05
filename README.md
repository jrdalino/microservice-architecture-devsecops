# Microservices Architecture with DevSecOps on AWS

This is the landing page for a set of tutorials on how to build a Microservices Architecture w/ DevSecOps on AWS using:

- Python Flask for my Backend deployed on AWS Elastic Container Service (ECS) or AWS Elastic Kubernetes Service (EKS)
- HTML, VueJS, Bootstrap (CSS) for my Frontend deployed on S3 w/ CloudFront as CDN
- Authentication using Amazon Cognito
- NoSQL Database using DynamoDB
- Rest API using Amazon API Gateway
- Infrastucture as Code using CloudFormation or Terraform
- Setup CI/CD using AWS CodeCommit, CodeBuild and CodePipeline for Infrastucture, Backend and Frontend
- High Availability using Kubernetes Liveness and Readiness health Checks
- Autoscaling using Horizonal Pod Autoscaler (HPA) and Cluster Autoscaler (CA)
- Monitoring using Prometheus and Grafana
- Logging with ElasticSearch, Fluentd and Kibana
- Instrumentation and Tracing with X-Ray

## Prerequisites
- https://github.com/jrdalino/myproject-prerequisites

## Step 1: AWS Organizations and Multi-Account Hardening
- https://github.com/jrdalino/aws-organizations-multi-account-hardening

## Step 2: Create Frontend VueJS3 Boostrap4 HTML CSS
- https://github.com/jrdalino/myproject-consumer-web

## Step 3: Deploy Static Website to S3 + CloudFront
- https://github.com/jrdalino/myproject-frontend-deploy-to-s3-cloudfront

## Step 4: Setup CI/CD for Front End
- https://github.com/jrdalino/myproject-consumer-web-codepipeline

## Step 5: Add Authentication using Amazon Cognito
- https://github.com/jrdalino/myproject-consumer-web-amazon-cognito

## Step 6: Create Backend Python Flask RestAPI
- https://github.com/jrdalino/myproject-product-restapi
- (TODO) https://github.com/jrdalino/myproject-consumer-restapi
- (TODO) https://github.com/jrdalino/myproject-order-restapi
- (TODO) https://github.com/jrdalino/myproject-billing-restapi
- (TODO) https://github.com/jrdalino/myproject-payment-restapi
- (TODO) https://github.com/jrdalino/myproject-notification-restapi
- (TODO) https://github.com/jrdalino/myproject-provider-restapi

## Step 7: Add Infrastructure as Code Templates (Custom VPC with Public and Private Subnets) w/ Testing
- Cloudformation: https://github.com/jrdalino/myproject-core-cloudformation
- (TODO) Terraform: https://github.com/jrdalino/myproject-core-terraform

## Step 8: Setup CI/CD for Infrastructure as Code Templates
- Cloudformation: https://github.com/aws-quickstart/quickstart-taskcat-ci
- (TODO) Terraform: https://dzone.com/articles/immutable-infrastructure-cicd-using-hashicorp-terr

## Step 9: Install (a) ECS or (b) Kubernetes Tools & Launch EKS (EKCTL)
- ECS: https://github.com/jrdalino/aws-ecs
- (TODO) EKS: https://github.com/jrdalino/aws-eks-eksctl
- (TODO) K8S: Example > install single-node master K8s cluster https://github.com/jrdalino/single-master-kubernetes-cluster
- (TODO) K8S KOPS: Example > install a multi-node master K8s cluster https://github.com/jrdalino/multi-master-kubernetes-cluster-kops

## Step 10: Deploy Backend MicroService to (a) ECS or (b) EKS
- ECS: https://github.com/jrdalino/backend-deploy-to-ecs-fargate
- (TODO) EKS: https://github.com/jrdalino/backend-deploy-to-kubernetes

## Step 11: Setup CI/CD for Back End Service
- ECS: https://github.com/jrdalino/myproject-product-restapi-codepipeline-ecs
- (TODO) EKS: https://github.com/jrdalino/myproject-product-restapi-codepipeline-kubernetes

## Step 12: Database - NoSQL (DynamoDB)
- https://github.com/jrdalino/myproject-product-amazon-dynamodb
- (TODO) https://github.com/jrdalino/myproject-consumer-amazon-dynamodb
- (TODO) https://github.com/jrdalino/myproject-order-amazon-dynamodb
- (TODO) https://github.com/jrdalino/myproject-billing-amazon-dynamodb
- (TODO) https://github.com/jrdalino/myproject-payment-amazon-dynamodb
- (TODO) https://github.com/jrdalino/myproject-notification-amazon-dynamodb
- (TODO) https://github.com/jrdalino/myproject-provider-amazon-dynamodb

## Step 13: Add API Gateway in front of EKS/ECS Endpoint
- https://github.com/jrdalino/amazon-api-gateway

## ***
## ***
## ***

## (TODO) Step 14: Caching - DynamoDB Accelerator (DAX)

## (TODO) Step 15: Database - RDBMS (RDS Postgres)

## (TODO) Step 16: Caching - ElastiCache (Redis)

## (TODO) Step 17: Add Web Application Firewall in front of API Gateway
- AWS Web Application Firewall
- AWS Shield
- https://github.com/aws-samples/aws-waf-sample/blob/master/waf-owasp-top-10/owasp_10_base.yml

## (TODO) Step 18: Backend Static Testing (Static Code Analyzer)
- Find OSS alternative for SonarQube / Fortify / Checkmarx

## (TODO) Step 19: Backend Dynamic Testing (Containers Scanning)
- Find OSS alternative for  Anchore Engine / Aqua Microscanner / Clair / Dagda / Twistlock

## (TODO) Step 20: Web Application Firewall in front of CloudFront CDN
- AWS Web Application Firewall and AWS Shield
- https://github.com/aws-samples/aws-waf-sample/blob/master/waf-owasp-top-10/owasp_10_base.yml

## (TODO) Step 21: Frontend Static Testing (Packages)
- AuditJS > https://www.npmjs.com/package/auditjs
- RetireJS > https://retirejs.github.io/retire.js > Checks npm packages and compares to CVE's
- OWASP Dependency Check > https://www.owasp.org/index.php/OWASP_Dependency_Check

## (TODO) Step 22: Frontend Dynamic Testing (OWASP)
- OWASP ZAP Web App Pentest Tool > https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project

## (TODO) Step 23: Check for Commited Secrets on Git Repository
- https://github.com/awslabs/git-secrets
- https://github.com/zricethezav/gitleaks

## (TODO) Step 24: Security - AWS Secrets Management
- AWS Secrets Manager: https://github.com/jrdalino/aws-secrets-manager
- AWS Parameter Store / Hashicorp Vault

## (TODO) Step 25: SSL/TLS using AWS Certificate Manager
- https://github.com/jrdalino/aws-certificate-manager

## (TODO) Step 26: Register/Transfer Domain Name using Route 53 for CloudFront CDN

## Step 27: (**Kubernetes**) Install Helm
- https://github.com/jrdalino/install-helm

## Step 28: Deploy monitoring using Prometheus
- https://github.com/jrdalino/install-prometheus-using-helm

## Step 29: Deploy Grafana to create Dashboards
- https://github.com/jrdalino/install-grafana-using-helm

## Step 30: (**Kubernetes**) Implement K8s Liveness Probe Health Checks
- https://github.com/jrdalino/kubernetes-liveness-probes

## Step 31: (**Kubernetes**) Implement K8s Readiness Probe Health Checks
- https://github.com/jrdalino/kubernetes-readiness-probes

## (TODO) Step 32: Implement Load Testing using JMeter

## Step 33: (**Kubernetes**) Implementing K8s Auto Scaling
- https://github.com/jrdalino/kubernetes-autoscaling

## Step 34: Logging with ElastiSearch, Fluentd, and Kibana (EFK)
- https://github.com/jrdalino/elasticsearch-fluentd-kibana

## Step 35: Predictive Autoscaling for Kubernetes using Prometheus + Amazon SageMaker

## (TODO) Step 36: Instrumentation using AWS X-Ray
- AWS X-Ray / Prometheus

## (TODO) Step 37: Alerting using AlertManager
- AlertManager
- PagerDuty
- Slack

## (TODO) Step 38: Service Discovery using Consul
- Hashicorp Consul / Netflix Eureka / AWS CloudMap

## (TODO) Step 39: Service Mesh using Istio
- Istio / AWS Appmesh
