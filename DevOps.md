# DevOps

## Table of contents

 * [GitFlow](#gitflow)
 * [Track Issue](#track-issue)
 * [CI](#ci-continuos-integration)
 * [Tests](#tests)
 * [Monitoring](#monitoring)
 * [CD](#cd-continuos-delivery)
 
## GitFlow
<p align="justify">
Git Flow is a workflow template that simplifies and organizes the versioning of branches in a development project in Git.
Git Flow can be implemented in any software project and is directly aligned with the DevOps practice of continuous delivery. Git Flow assigns very specific rules to different types of branches and defines when they should interact. In addition, it has individual branches to prepare, maintain and record journal entries. Based on this principle, we are making use of branches in the project.

In our workflow, branches are being divided into four types:
- Main
- Dev
- Feature
- Bugfix

**Main** it is the main branch that contains the production source code that has been tested and approved. It is not allowed to make changes (commit) directly in main, it represents the stable production line of our project. Commits on this branch are done via merges from the Dev branch and occasionally the Bugfix branch.

**Dev** is the main development branch of our project, it is created from Main. It should always be organized, with tested code, but not necessarily stable enough for production deployment. Direct commits to the dev branch are avoided, and work is mostly done on feature branches spawned from it.
Every time you make a feature pull request to Dev, the code from the test pipeline is tested on Github Actions, so that the tested and functional code is stored in the main. 

**Feature** it is a temporary branch created from the Dev branch that carries new functionality for the project, it will always end up being implemented to the Dev itself, through a merge and discarded when it fulfills its purpose.
 
If a bug is found after the merge, we apply the **Bugfix** branch, where it can be generated both from dev and main. The cause of the problem is investigated, corrections are applied to the code and later updating the main branches.
 
<img src="https://github.com/DolphinDatabase/MCS/assets/74321890/6e4c8f8f-c1c9-4a12-b661-e4de9ff2a98a"/>
  
## Track Issue
<p align="justify">
For the management tool we decided to use Jira, it is structured with Epic/ Story/ Task.
 
Stories, in our case, are equivalent to issues, where we can integrate with github using their id.
 
To commit, we use **pre-commit** where we define that every commit must start with the standard abbreviation for stories (CLD-) and the number (it varies for each story).
 
### pre-commit
<p align="justify">

Pre-commit setup commands:
```
pre-commit install
pre-commit autoupdate
```

At the time of commit, the contributor passes an id and if this id is valid, the commit occurs normally.

```
git commit -m "CLD-109 transaction time"
black....................................................................Passed
```

If the id is not valid, you receive a warning to correct it and the commit is not completed.
```
git commit -m "teste"
[INFO] Initializing environment for https://github.com/ambv/black.
[INFO] Installing environment for https://github.com/ambv/black.
[INFO] Once installed this environment will be reused.
[INFO] This may take a few minutes...
black....................................................................Failed
```

## CI (Continuos Integration)
<p align="justify">
As the gitflow scheme of the project, the main branch is the production branch, due to this process, there is a github actions flow that performs the automatic deployment to the AWS EC2 production server.
 
The deployment is done in a container, and it is created from an image created from the [Dockerfile](https://github.com/DolphinDatabase/Cloudin-backend/blob/main/Dockerfile), in addition, every time the deploy is done, all images and containers are deleted according to the [deploy flow](https://github.com/DolphinDatabase/Cloudin-backend/blob/main/.github/workflows/deploy.yml).
</p>
 
## Tests
We developed two types of tests for the application, unit and integration, for the execution of these tests, only the unit tests are done at each merge of some branch feature or bugfix in the dev branch according to the [unit test flow](https://github.com/DolphinDatabase/Cloudin-backend/blob/main/.github/workflows/test.yml), since the integration tests are triggered manually by the tester in the main and dev branches according to the [manual test flow](https://github.com/DolphinDatabase/Cloudin-backend/blob/main/.github/workflows/manualTest.yml).

### Unit test
In the unit test we test functions and the expected return, there is also the database test, where we test your business rule.
Unit testing occurs whenever there is a pull request to the dev branch.

### Integration test
The integration test tests the connection to the server, and in our case, tests file transfers, token validation, and authentication.
This test is manual.

## Monitoring
<p align="justify">
The project uses an Amazon Linux EC2 instance to host the api written in Flask. Inside this virtual machine, we use the "zabbix/zabbix-agent:latest" docker image, which already has an embedded PostgreSQL database, to create the "zabbix_server" container and use the monitoring infrastructure that the Zabbix platform offers. This container uses the Alpine Linux distribution and we use the "apk" package manager to install the "zabbix-agent". In the zabbix web interface, we create the host called "Backend" which is referenced inside the "zabbix-agent" configuration file.

## CD (Continuos Delivery)
    
<details id="k3d-p5"><summary>K3D Cluster</summary>

    In order to create a k8s environment for testing before using the AKS cluster, the configuration and documentation of the k3d cluster was carried out. K3d is a command line tool designed to simplify the management of Kubernetes clusters locally. It lets you create, deploy, and manage Kubernetes clusters in your development or test environment. With K3d, you can quickly provision lightweight Kubernetes clusters in Docker containers, making it easy to run multiple Kubernetes instances on a single machine. It's a popular choice for developers who want to test and debug applications in a local Kubernetes environment.

    After installing the dependencies, just run the following command:
    ```bash
    k3d cluster create --config k3d-simple-cluster.yaml
    ```

    The complete setup guide is present in the [project repository.](https://github.com/DolphinDatabase/Cloudin-backend#readme)

    </details>

    <details id="aks-p5"><summary>AKS Cluster</summary>

    Azure Kubernetes Service (AKS) offers the fastest way to get started developing and deploying cloud-native apps in Azure, in datacenters or at the edge with built-in code-to-cloud pipelines and health checkers. Get unified management and governance for on-premises, edge, and multi-cloud Kubernetes clusters. Interoperate with Azure security, identity, cost management and migration services [\[23\]](#references).

    </details>

    <details id="deployment-p5"><summary>Kubernetes Deployment</summary>

    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: cloudin-midall
    labels:
        app: cloudin-midall
    spec:
    replicas: {{ .Values.replicas }}
    selector:
        matchLabels:
        app: cloudin-midall
    template:
        metadata:
        labels:
            app: cloudin-midall
        spec:
        containers:
            - name: backend
            image: {{ include "cloudin-midall.getImage" . }}
            ports:
                - containerPort: {{ .Values.backend.ports.containerPort }}
            env:
            - name: DATABASE_URL
                value: mysql://dbuser:dbuser@cloudin-midall-mysql:3306/cloudin
    {{ include "cloudin-midall.probeAndResources" .Values.backend.ports.containerPort | nindent 10 }}
    ```

    A "Deployment" is used to create and manage application instances in a Kubernetes cluster. It specifies settings for replication, pod selection, pod template, and associated containers. In the case used in the project, the Deployment called "cloudin-midall" is being configured to create a replicaSet with the number of replicas defined by the value defined in values.yaml. The "backend" container is defined with its image and port and environment variable settings.

    A Deployment is suitable for applications that are stateless, that is, do not store persistent data. It manages the deployment of pod replicas in a controlled manner, enabling upgrades, rollback, and automatic scaling. Pods created by a Deployment have no unique identity and do not maintain persistent connections to storage.

    </details>

    <details id="statefulset-p5"><summary>Kubernetes StatefulSet</summary>

    ```yaml
    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
    name: cloudin-midall-mysql
    labels:
        app: cloudin-midall-mysql
    spec:
    serviceName: cloudin-midall-mysql
    replicas: 1
    selector:
        matchLabels:
        app: cloudin-midall-mysql
    template:
        metadata:
        labels:
            app: cloudin-midall-mysql
        spec:
        containers:
            - name: mysql
            image: mysql:latest
            ports:
                - containerPort: 3306
            env:
                - name: MYSQL_ROOT_PASSWORD
                value: "example"
                - name: MYSQL_DATABASE
                value: "cloudin"
                - name: MYSQL_USER
                value: "dbuser"
                - name: MYSQL_PASSWORD
                value: "dbuser"
            resources:
                limits:
                cpu: 100m
                memory: 512Mi
                requests:
                cpu: 80m
                memory: 128Mi
    ```

A "StatefulSet" has been defined in Kubernetes to run a MySQL database instance. The StatefulSet is named "cloudin-midall-mysql" and has a replica. The service associated with StatefulSet is also called "cloudin-midall-mysql". The pod definition includes a container called "mysql" that uses the latest MySQL image. Port 3306 is exposed for communication. Environment variables are set to define the MySQL root user password, database name, database user name and password. Resource constraints are defined to limit the container's CPU and memory usage.

    A "StatefulSet" is used for applications that have state and require unique identity and persistent storage for their pods. It provides guarantees of order and stability when creating, updating, and deleting pods. Each pod in a StatefulSet is assigned a unique identifier and maintains its own persistent state. This is especially useful for databases and other applications that require persistent storage and state consistency across pods.

    </details>

    <details id="service-ip-p5"><summary>Kubernetes Service ClusterIP</summary>

    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
    name: cloudin-midall-mysql
    labels:
        app: cloudin-midall-mysql
    spec:
    type: ClusterIP
    ports:
        - port: 3306
        targetPort: 3306
        protocol: TCP
        name: mysql
    selector:
        app: cloudin-midall-mysql
    ```

  Service ClusterIP is a type of service in Kubernetes that exposes a set of pods for internal communication within the cluster. It provides a static internal IP address to the service, allowing other resources within the cluster to connect to it. The "cloudin-midall-mysql" service is defined as a Service ClusterIP. It maps port 3306 to targetPort 3306 in TCP protocol, which is the standard for communication with the MySQL database. The "app: cloudin-midall-mysql" selector ensures that the Service forwards traffic to the pods that have the same label. This type of service is suitable for internal communication and is not accessible from outside the cluster.

    </details>

    <details id="service-lb-p5"><summary>Kubernetes Service LoadBalancer</summary>

    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
    name: cloudin-midall
    labels:
        app: cloudin-midall
    spec:
    type: LoadBalancer
    ports:
    - port: {{ .Values.backend.ports.containerPort }}
        targetPort: {{ .Values.backend.ports.containerPort }}
        protocol: TCP
        name: http
    selector:
        app: cloudin-midall
    ```

    Service LoadBalancer is a type of service in Kubernetes that exposes a set of pods to external traffic, allowing the application to be accessed from outside the cluster. It automatically provisions an external load balancer, such as a cloud load balancer, that distributes traffic to the Service Pods. The "cloudin-midall" service is defined as a Service LoadBalancer. It maps a given port, defined by the ".Values.backend.ports.containerPort" value, to the corresponding targetPort. The specified protocol is TCP and the service name is "http". The "app: cloudin-midall" selector ensures that the Service forwards traffic to pods with the same tag. This type of service is suitable for exposing an application to external traffic, allowing accessibility from outside the cluster, through the load balancer provided by the Kubernetes runtime, such as a cloud load balancer.

    </details>

    <details id="hpa-p5"><summary>Kubernetes HorizontalPodAutoscaler</summary>
    
    ```yaml
    apiVersion: autoscaling/v2
    kind: HorizontalPodAutoscaler
    metadata:
    name: cloudin-midall-hpa
    spec:
    scaleTargetRef:
        apiVersion: apps/v1
        kind: Deployment
        name: cloudin-midall
    minReplicas: 1
    maxReplicas: 5
    metrics:
        - type: Resource
        resource:
            name: cpu
            target:
            type: Utilization
            averageUtilization: 80
    ```

    The HorizontalPodAutoscaler (HPA) is a Kubernetes feature that allows automatic scaling of the number of replicas of a Deployment or other horizontally scaling object. The HPA named "cloudin-midall-hpa" is configured to automatically scale the number of replicas of the "cloudin-midall" Deployment. The minimum number of replicas is set to 1 and the maximum number to 5. HPA uses metrics to make sizing decisions. The metric used is the average CPU utilization, where the HPA will try to keep the CPU utilization around 80%. Based on this metric, the HPA will increase or decrease the number of Deployment replicas to meet traffic demands. This allows the Kubernetes cluster to automatically adjust processing power as needed, ensuring efficient scaling of application resources.

    </details>

    <details id="ingress-p5"><summary>Kubernetes Ingress</summary>
    
    ```yaml
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
    name: cloudin-midall
    annotations:
        kubernetes.io/ingress.class: azure/application-gateway
    spec:
    rules:
    - http:
        paths:
        - path: /
            pathType: Prefix
            backend:
            service:
                name: cloudin-midall
                port:
                number: 5000
    ```

   Ingress is a feature in Kubernetes that allows exposing HTTP and HTTPS services externally to the cluster. The Ingress named "cloudin-midall" is defined with the annotation "kubernetes.io/ingress.class" set to "azure/application-gateway", indicating that the Ingress will be managed by Azure Application Gateway. The Ingress specification includes rules for routing traffic. In the example, there is a single rule that forwards all HTTP traffic coming into the root ("/") to the service called "cloudin-midall" on port 5000. The "pathType" field is set to "Prefix", which means that Ingress will match URLs that start with the specified path. Ingress allows flexible configuration of external traffic routing to internal services in Kubernetes, providing a layer of access control and load balancing.

    </details>

## Table of technologies
<img src="https://github.com/DolphinDatabase/Cloud-In/assets/58821700/489292f1-79a2-42a9-8a06-0d739c8feebf"/>

