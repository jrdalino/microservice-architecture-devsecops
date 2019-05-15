# Python Flask Backend + JS Frontend on AWS EKS (Kubernetes) with CI/CD, Monitoring and Logging
This is a simple tutorial on how to create a Python Flask Backend + Javascript Frontend Application, Setup CI/CD with AWS Code% and AWS Elastic Kubernetes Service, High Availability, Autoscaling, Monitoring using Prometheus and Grafana and Logging with ElasticSearch, Fluentd and Kibana.

- **Step 1: Create Backend using Python Flask Rest API**
- **Step 2: Create Frontend using HTML, CSS, JS**

- TODO: Add Governance - AWS Organizations and AWS Account Hardening
- TODO: Add Infrastructure as Code Templates (Custom VPC with Public and Private Subnets)
- TODO: Add Testing for Infrastructure as Code
- TODO: Setup CI/CD for Infrastructure as Code

- **Step 3: Install Kubernetes Tools and Launch EKS using EKCTL**
- **Step 4: Deploy Backend MicroService to EKS**
- **Step 5: Setup CI/CD for Back End Service**

- TODO: Add Security - API Gateway in front of EKS Endpoint
- TODO: Add Security - Web Application Firewall in front of API Gateway
- TODO: Add Testing - Backend Static Code Analysis
- TODO: Add Testing - Backend Containers Scanning

- **Step 6: Deploy Frontend to S3, CloudFront**
- **Step 7: Setup CI/CD for Front End Service**

- TODO: Add Authentication using Amazon Cognito

- TODO: Add Security - Web Application Firewall in front of CloudFront CDN
- TODO: Add Testing - Frontend Static Testing
- TODO: Add Testing - Frontend Dynamic Testing

- TODO: Add Database - NoSQL (DynamoDB)
- TODO: Add Caching - DynamoDB Accelerator (DAX)
- TODO: Add Database - RDBMS (Postgres)
- TODO: Add Caching - ElastiCache (Redis)

- TODO: Add Testing - Committed Secrets on Git Repository
- TODO: Add Security - AWS Secrets Management

- TODO: Add SSL/TLS using AWS Certificate Manager
- TODO: Add Register/Transfer Domain Name using Route 53

- **Step 8: Install Helm**
- **Step 9: Deploy Prometheus for basic monitoring**
- **Step 10: Deploy Grafana to create Dashboards**
- **Step 11: Implement Liveness Probe Health Checks**
- **Step 12: Implement Readiness Probe Health Checks**
- **Step 13: Implement Auto Scaling**
- **Step 14: Logging with ElastiSearch, Fluentd, and Kibana (EFK)**

- TODO: Add Instrumentation
- TODO: Add Alerting
- TODO: Add Service Discovery
- TODO: Add Service Mesh

## Prerequisites
Working Directory
```
$ cd ~
$ mkdir environment
$ cd ~/environment
```

Homebrew
```
$ brew --version
```

AWS CLI
```
$ aws --version
$ aws configure
```

Git
```
$ git --version
$ git config --global user.name "REPLACE_ME_WITH_YOUR_NAME"
$ git config --global user.email REPLACE_ME_WITH_YOUR_EMAIL@example.com
$ git config --global credential.helper '!aws codecommit credential-helper $@'
$ git config --global credential.UseHttpPath true
```

Python
```
$ python3 --version
Python 3.7.3
```

Virtualenv
```
$ virtualenv --version
16.4.3
$ echo 'venv' > .gitignore
$ python3 -m venv venv
$ source venv/bin/activate
(venv) $
(venv) $ deactivate
```

Flask
```
(venv) $ pip install flask
(venv) $ flask --version
Flask 1.0.2
Python 3.7.3 (default, Mar 27 2019, 09:23:15) 
[Clang 10.0.1 (clang-1001.0.46.3)]
```

Docker
```
$ docker -v
Docker version 18.09.2, build 6247962
```

# ************************************************************

## Step 1: Create Backend Python Flask RestAPI Calculator
- https://github.com/jrdalino/backend-python-flask-restapi-calculator

## Step 2: Create Frontend HTML CSS JS Calculator
- https://github.com/jrdalino/frontend-html-css-js-calculator

## TODO: Add Governance - AWS Organizations and AWS Account Hardening
- Wait for https://aws.amazon.com/controltower/
- Enable AWS Organizations + Landing Zone (https://aws.amazon.com/solutions/aws-landing-zone/)
- Add SCP Policies
- Create new AWS Account per product and per environment (production and staging)
- New Email alias, AWS Account, Contacts
- Secure Root User, MFA
- Delete Default VPC's
- S3 Buckets for Logging
- S3 Buckets for ELB Logging
- S3 Buckets for Cloudfront Logs
- Enable CloudTrail Logs (Auditing)
- Enable VPC Flow Logs
- Enable ELB Logging
- Enable Config for AWS resource config tracking
- Enable SNS topics for alerting and notifications
- Enable Guard Duty for Intelligent threat detection
- Enable Security Hub for Compliance Scanning

## TODO: Add Infrastructure as Code Templates (Custom VPC with Public and Private Subnets) w/ Testing
- Terraform
- Reusable Infrastructure w/ Terraform Modules
- Locking and Isolating State Files on S3 using https://github.com/gruntwork-io/terragrunt
- Testing using Terratest https://github.com/gruntwork-io/terratest

## TODO: Setup CI/CD for Infrastructure as Code Templates
- CI/CD Pipeline using S3, CodeBuild, CodePipeline, DynamoDB for State Locking

## Step 3: Install Kubernetes Tools and Launch EKS using EKCTL
- https://github.com/jrdalino/aws-eks-eksctl
- Example on how to install single-node master Kubernetes cluster  https://github.com/jrdalino/single-master-kubernetes-cluster
- Example on how to install a multi-node master Kubernetes cluster using kops https://github.com/jrdalino/multi-master-kubernetes-cluster-kops

## Step 4: Deploy Backend MicroService to EKS
- https://github.com/jrdalino/backend-deploy-to-kubernetes

## Step 5: Setup CI/CD for Back End Service
- https://github.com/jrdalino/backend-cicd-codecommit-codebuild-codepipeline

## TODO: Add API Gateway in front of EKS Endpoint
- AWS API Gateway
- Authentication: Resource Policies/ IAM / Lambda Authorizers (token based, request parameter based)/ Cognito User Pools
- Authorization
- Enable CORS
- Swagger Documentation

## TODO: Add Web Application Firewall in front of API Gateway
- AWS Web Application Firewall
- AWS Shield
- https://github.com/aws-samples/aws-waf-sample/blob/master/waf-owasp-top-10/owasp_10_base.yml

## TODO: Add Testing - Backend Static Code Analyzer
- Find OSS alternative for SonarQube / Fortify / Checkmarx

## TODO: Add Testing - Backend Containers Scanning
- Find OSS alternative for  Anchore Engine / Aqua Microscanner / Clair / Dagda / Twistlock

## Step 6: Deploy Frontend
- https://github.com/jrdalino/frontend-deploy-to-s3-cloudfront

## Step 7: Setup CI/CD for Front End
- https://github.com/jrdalino/frontend-cicd-codecommit-codepipeline

## TODO: Add Authentication using Amazon Cognito

## TODO: Add Security - Web Application Firewall in front of CloudFront CDN
- AWS Web Application Firewall and AWS Shield
- https://github.com/aws-samples/aws-waf-sample/blob/master/waf-owasp-top-10/owasp_10_base.yml

## TODO: Add Testing - Frontend Static Application
- AuditJS > https://www.npmjs.com/package/auditjs
- RetireJS > https://retirejs.github.io/retire.js > Checks npm packages and compares to CVE's
- OWASP Dependency Check > https://www.owasp.org/index.php/OWASP_Dependency_Check

## TODO: Add Testing - Frontend Dynamic Application
- OWASP ZAP Web App Pentest Tool > https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project > 

## TODO: Add Database - NoSQL (DynamoDB)

## TODO: Add Caching - DynamoDB Accelerator (DAX)

## TODO: Add Database - RDBMS (RDS Postgres)

## TODO: Add Caching - ElastiCache (Redis)

## TODO: Add Testing - Commited secrets on Git Repository
- https://github.com/awslabs/git-secrets
- https://github.com/zricethezav/gitleaks

## TODO: Add Security - AWS Secrets Management
- AWS Secrets Manager: https://github.com/jrdalino/aws-secrets-manager
- AWS Parameter Store / Hashicorp Vault

## TODO: Add SSL/TLS using AWS Certificate Manager

## TODO: Add Register/Transfer Domain Name using Route 53 for CloudFront CDN

# ************************************************************

## Step 8: Install Helm

### Step 8.1: Install Helm CLI
```
$ cd ~/environment
$ curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > get_helm.sh
$ chmod +x get_helm.sh
$ ./get_helm.sh
```

### Step 8.2: Configure Helm access with RBAC
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

### Step 8.3: Apply the config
```
$ kubectl apply -f ~/environment/rbac.yaml
```

### Step 8.4: Install helm and tiller into the cluster which gives it access to manage resources in your cluster.
```
$ helm init --service-account tiller
```

### (Optional) Clean up
```
$ kubectl delete -f ~/environment/rbac.yaml
$ rm ~/environment/rbac.yaml
$ rm ~/environment/get_helm.sh
```

# ************************************************************

## Step 9: Deploy Prometheus for basic monitoring

### Step 9.1: Install Prometheus
```
$ kubectl create namespace prometheus
$ helm install stable/prometheus \
    --name prometheus \
    --namespace prometheus \
    --set alertmanager.persistentVolume.storageClass="gp2" \
    --set server.persistentVolume.storageClass="gp2"
```

### Step 9.2: Check if Prometheus components deployed as expected
```
$ kubectl get all -n prometheus
```

### Step 9.3: Access the Prometheus server URL w/ kubectl port-forward and access /targets Web UI
```
$ kubectl port-forward -n prometheus deploy/prometheus-server 8080:9090
```

### (Optional) Clean up
```
$ helm delete prometheus
$ helm del --purge prometheus
```

# ************************************************************

## Step 10: Deploy Grafana to create Dashboards

### Step 10.1: Install Grafana
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

### Step 10.2: Check if Grafana is deployed
```
$ kubectl get all -n grafana
```

### Step 10.3: Get Grafana ELB URL
```
$ export ELB=$(kubectl get svc -n grafana grafana -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')
$ echo "http://$ELB"
```

### Step 10.4: Login using admin and password
```
$ kubectl get secret --namespace grafana grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

### Step 10.5: Create Grafana Dashboards
- Login into Grafana dashboard using credentials supplied during configuration
- You will notice that ‘Install Grafana’ & ‘create your first data source’ are already completed. We will import community created dashboard for this tutorial
- Click ‘+’ button on left panel and select ‘Import’
- Enter 3131 dashboard id under Grafana.com Dashboard & click ‘Load’.
- Leave the defaults, select ‘Prometheus’ as the endpoint under prometheus data sources drop down, click ‘Import’.
- This will show monitoring dashboard for all cluster nodes
- For creating dashboard to monitor all pods, repeat same process as above and enter 3146 for dashboard id

### (Optional) Clean up
```
$ helm delete grafana
$ helm del --purge grafana
```

# ************************************************************

## Step 11: Implement Liveness Probe Health Checks
- The API being a crucial part of the application it needs to be highly available

### Step 11.1: Configure the Probe
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

### Step 11.2: Create the pod using the manifest
```
$ kubectl apply -f ~/environment/healthchecks/liveness-app.yaml
$ kubectl get pod liveness-app
$ kubectl describe pod liveness-app
```

### Step 11.3: Introduce a Failure to Test
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

## Step 12: Implement Readiness Probe Health Checks
- The API being a crucial part of the application it needs to be highly available

### Step 12.1: Configure the Probe
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

### Step 12.2: Create a deployment to test readiness probe
```
$ kubectl apply -f ~/environment/healthchecks/readiness-deployment.yaml
$ kubectl get pods -l app=readiness-deployment
```

### Step 12.3: Confirm that all the replicas are available to serve traffic when a service is pointed to this deployment
```
$ kubectl describe deployment readiness-deployment | grep Replicas:
```

### Step 12.4: Introduce a Failure
```
$ kubectl exec -it <YOUR-READINESS-POD-NAME> -- rm /tmp/healthy
$ kubectl get pods -l app=readiness-deployment
$ kubectl describe deployment readiness-deployment | grep Replicas:
```

### Step 12.5: Restore pod to Ready Status
```
$ kubectl exec -it <YOUR-READINESS-POD-NAME> -- touch /tmp/healthy
$ kubectl get pods -l app=readiness-deployment
```

### (Optional) Clean Up
```
$ kubectl delete -f ~/environment/healthchecks/readiness-deployment.yaml
```

# ************************************************************

## Step 13: Implementing Auto Scaling

### Step 13.1: Configure Horizontal Pod AutoScaler (HPA) - Deploy the Metrics Server
```
$ helm install stable/metrics-server \
    --name metrics-server \
    --version 2.0.4 \
    --namespace metrics
$ kubectl get apiservice v1beta1.metrics.k8s.io -o yaml
```

### Step 13.2: Scale an Application with Horizontal Pod AutoScaler (HPA)
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

### Step 13.3: Configure Cluster AutoScaler (CA)
- Configure the Cluster Autoscaler (CA)
```
$ mkdir ~/environment/cluster-autoscaler
$ cd ~/environment/cluster-autoscaler
$ vi ~/environment/cluster-autoscaler/cluster_autoscaler.yml
```

- Change <AUTOSCALING GROUP NAME>, AWS_REGION and minimum nodes (2) and maximum nodes (8)
```
apiVersion: v1
kind: ServiceAccount
metadata:
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
  name: cluster-autoscaler
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRole
metadata:
  name: cluster-autoscaler
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
rules:
- apiGroups: [""]
  resources: ["events","endpoints"]
  verbs: ["create", "patch"]
- apiGroups: [""]
  resources: ["pods/eviction"]
  verbs: ["create"]
- apiGroups: [""]
  resources: ["pods/status"]
  verbs: ["update"]
- apiGroups: [""]
  resources: ["endpoints"]
  resourceNames: ["cluster-autoscaler"]
  verbs: ["get","update"]
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["watch","list","get","update"]
- apiGroups: [""]
  resources: ["pods","services","replicationcontrollers","persistentvolumeclaims","persistentvolumes"]
  verbs: ["watch","list","get"]
- apiGroups: ["extensions"]
  resources: ["replicasets","daemonsets"]
  verbs: ["watch","list","get"]
- apiGroups: ["policy"]
  resources: ["poddisruptionbudgets"]
  verbs: ["watch","list"]
- apiGroups: ["apps"]
  resources: ["statefulsets"]
  verbs: ["watch","list","get"]
- apiGroups: ["storage.k8s.io"]
  resources: ["storageclasses"]
  verbs: ["watch","list","get"]

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: Role
metadata:
  name: cluster-autoscaler
  namespace: kube-system
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
rules:
- apiGroups: [""]
  resources: ["configmaps"]
  verbs: ["create"]
- apiGroups: [""]
  resources: ["configmaps"]
  resourceNames: ["cluster-autoscaler-status"]
  verbs: ["delete","get","update"]

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: cluster-autoscaler
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-autoscaler
subjects:
  - kind: ServiceAccount
    name: cluster-autoscaler
    namespace: kube-system

---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: RoleBinding
metadata:
  name: cluster-autoscaler
  namespace: kube-system
  labels:
    k8s-addon: cluster-autoscaler.addons.k8s.io
    k8s-app: cluster-autoscaler
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: cluster-autoscaler
subjects:
  - kind: ServiceAccount
    name: cluster-autoscaler
    namespace: kube-system

---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: cluster-autoscaler
  namespace: kube-system
  labels:
    app: cluster-autoscaler
spec:
  replicas: 1
  selector:
    matchLabels:
      app: cluster-autoscaler
  template:
    metadata:
      labels:
        app: cluster-autoscaler
    spec:
      serviceAccountName: cluster-autoscaler
      containers:
        - image: k8s.gcr.io/cluster-autoscaler:v1.2.2
          name: cluster-autoscaler
          resources:
            limits:
              cpu: 100m
              memory: 300Mi
            requests:
              cpu: 100m
              memory: 300Mi
          command:
            - ./cluster-autoscaler
            - --v=4
            - --stderrthreshold=info
            - --cloud-provider=aws
            - --skip-nodes-with-local-storage=false
            - --nodes=2:8:<AUTOSCALING GROUP NAME>
          env:
            - name: AWS_REGION
              value: us-east-1
          volumeMounts:
            - name: ssl-certs
              mountPath: /etc/ssl/certs/ca-certificates.crt
              readOnly: true
          imagePullPolicy: "Always"
      volumes:
        - name: ssl-certs
          hostPath:
            path: "/etc/ssl/certs/ca-bundle.crt"
```

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

### Step 13.4: Scale a Cluster with Cluster Auto Scaler
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

## Step 14: Logging with ElastiSearch, Fluentd, and Kibana (EFK)
- Daily/weekly/monthly report showing the operations that have been performed during that time period

### Step 14.1: Configure IAM Policy for Worker Nodes
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

### Step 14.2: Provision an Elasticsearch Cluster
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

### Step 14.3: Deploy Fluentd
```
$ mkdir ~/environment/fluentd
$ cd ~/environment/fluentd
$ vi ~/environment/fluentd/fluentd.yml
```

- Replace REGION and CLUSTER_NAME
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: fluentd
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRole
metadata:
  name: fluentd
  namespace: kube-system
rules:
- apiGroups: [""]
  resources:
  - namespaces
  - pods
  verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: fluentd
  namespace: kube-system
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: fluentd
subjects:
- kind: ServiceAccount
  name: fluentd
  namespace: kube-system
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: fluentd-config
  namespace: kube-system
  labels:
    k8s-app: fluentd-cloudwatch
data:
  fluent.conf: |
    @include containers.conf
    @include systemd.conf

    <match fluent.**>
      @type null
    </match>
  containers.conf: |
    <source>
      @type tail
      @id in_tail_container_logs
      @label @containers
      path /var/log/containers/*.log
      pos_file /var/log/fluentd-containers.log.pos
      tag *
      read_from_head true
      <parse>
        @type json
        time_format %Y-%m-%dT%H:%M:%S.%NZ
      </parse>
    </source>

    <label @containers>
      <filter **>
        @type kubernetes_metadata
        @id filter_kube_metadata
      </filter>

      <filter **>
        @type record_transformer
        @id filter_containers_stream_transformer
        <record>
          stream_name ${tag_parts[3]}
        </record>
      </filter>

      <match **>
        @type cloudwatch_logs
        @id out_cloudwatch_logs_containers
        region "#{ENV.fetch('REGION')}"
        log_group_name "/eks/#{ENV.fetch('CLUSTER_NAME')}/containers"
        log_stream_name_key stream_name
        remove_log_stream_name_key true
        auto_create_stream true
        <buffer>
          flush_interval 5
          chunk_limit_size 2m
          queued_chunks_limit_size 32
          retry_forever true
        </buffer>
      </match>
    </label>
  systemd.conf: |
    <source>
      @type systemd
      @id in_systemd_kubelet
      @label @systemd
      filters [{ "_SYSTEMD_UNIT": "kubelet.service" }]
      <entry>
        field_map {"MESSAGE": "message", "_HOSTNAME": "hostname", "_SYSTEMD_UNIT": "systemd_unit"}
        field_map_strict true
      </entry>
      path /run/log/journal
      pos_file /var/log/fluentd-journald-kubelet.pos
      read_from_head true
      tag kubelet.service
    </source>

    <source>
      @type systemd
      @id in_systemd_kubeproxy
      @label @systemd
      filters [{ "_SYSTEMD_UNIT": "kubeproxy.service" }]
      <entry>
        field_map {"MESSAGE": "message", "_HOSTNAME": "hostname", "_SYSTEMD_UNIT": "systemd_unit"}
        field_map_strict true
      </entry>
      path /run/log/journal
      pos_file /var/log/fluentd-journald-kubeproxy.pos
      read_from_head true
      tag kubeproxy.service
    </source>

    <source>
      @type systemd
      @id in_systemd_docker
      @label @systemd
      filters [{ "_SYSTEMD_UNIT": "docker.service" }]
      <entry>
        field_map {"MESSAGE": "message", "_HOSTNAME": "hostname", "_SYSTEMD_UNIT": "systemd_unit"}
        field_map_strict true
      </entry>
      path /run/log/journal
      pos_file /var/log/fluentd-journald-docker.pos
      read_from_head true
      tag docker.service
    </source>

    <label @systemd>
      <filter **>
        @type record_transformer
        @id filter_systemd_stream_transformer
        <record>
          stream_name ${tag}-${record["hostname"]}
        </record>
      </filter>

      <match **>
        @type cloudwatch_logs
        @id out_cloudwatch_logs_systemd
        region "#{ENV.fetch('REGION')}"
        log_group_name "/eks/#{ENV.fetch('CLUSTER_NAME')}/systemd"
        log_stream_name_key stream_name
        auto_create_stream true
        remove_log_stream_name_key true
        <buffer>
          flush_interval 5
          chunk_limit_size 2m
          queued_chunks_limit_size 32
          retry_forever true
        </buffer>
      </match>
    </label>
---
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: fluentd-cloudwatch
  namespace: kube-system
  labels:
    k8s-app: fluentd-cloudwatch
spec:
  template:
    metadata:
      labels:
        k8s-app: fluentd-cloudwatch
    spec:
      serviceAccountName: fluentd
      terminationGracePeriodSeconds: 30
      # Because the image's entrypoint requires to write on /fluentd/etc but we mount configmap there which is read-only,
      # this initContainers workaround or other is needed.
      # See https://github.com/fluent/fluentd-kubernetes-daemonset/issues/90
      initContainers:
      - name: copy-fluentd-config
        image: busybox
        command: ['sh', '-c', 'cp /config-volume/..data/* /fluentd/etc']
        volumeMounts:
        - name: config-volume
          mountPath: /config-volume
        - name: fluentdconf
          mountPath: /fluentd/etc
      containers:
      - name: fluentd-cloudwatch
        image: fluent/fluentd-kubernetes-daemonset:v1.1-debian-cloudwatch
        env:
          - name: REGION
            value: us-east-1
          - name: CLUSTER_NAME
            value: calculator-eksctl
        resources:
          limits:
            memory: 200Mi
          requests:
            cpu: 100m
            memory: 200Mi
        volumeMounts:
        - name: config-volume
          mountPath: /config-volume
        - name: fluentdconf
          mountPath: /fluentd/etc
        - name: varlog
          mountPath: /var/log
        - name: varlibdockercontainers
          mountPath: /var/lib/docker/containers
          readOnly: true
        - name: runlogjournal
          mountPath: /run/log/journal
          readOnly: true
      volumes:
      - name: config-volume
        configMap:
          name: fluentd-config
      - name: fluentdconf
        emptyDir: {}
      - name: varlog
        hostPath:
          path: /var/log
      - name: varlibdockercontainers
        hostPath:
          path: /var/lib/docker/containers
      - name: runlogjournal
        hostPath:
          path: /run/log/journal
```

- Apply
```
$ kubectl apply -f ~/environment/fluentd/fluentd.yml
$ kubectl get pods -w --namespace=kube-system
```

### Step 14.4: Configure CloudWatch Logs
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

### Step 14.5 Configure Kibana
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
$ aws logs delete-log-group --log-group-name /eks/calculator-eksctl/containers
$ aws logs delete-log-group --log-group-name /aws/lambda/LogsToElasticsearch_kubernetes-logs
$ rm -rf ~/environment/iam_policy/
```

# ************************************************************

### TODO: Add Instrumentation
- AWS X-Ray / Prometheus

### TODO: Add Alerting
- AlertManager
- PagerDuty
- Slack

### TODO: Add Service Discovery
- Hashicorp Consul / Netflix Eureka / AWS CloudMap

### TODO: Add Service Mesh
- Istio / AWS Appmesh
