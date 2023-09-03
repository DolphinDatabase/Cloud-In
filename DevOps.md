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

- <h4 id="cd-p5">Continuous Delivery</h4>
    
    <details id="k3d-p5"><summary>K3D Cluster</summary>

    Com o objetivo de criar um ambiente k8s para teste antes de utilizar o cluster AKS, foi realizado a configuração e a documentação do cluster k3d. K3d é uma ferramenta de linha de comando projetada para simplificar o gerenciamento de clusters de Kubernetes localmente. Ele permite criar, implantar e gerenciar clusters Kubernetes em seu ambiente de desenvolvimento ou teste. Com o K3d, você pode provisionar rapidamente clusters Kubernetes leves em contêineres Docker, o que facilita a execução de várias instâncias do Kubernetes em uma única máquina. É uma opção popular para desenvolvedores que desejam testar e depurar aplicativos em um ambiente Kubernetes local [\[22\]](#referências).

    Depois de instalado as dependências, é necessário apenas rodar o seguinte comando:
    ```bash
    k3d cluster create --config k3d-simple-cluster.yaml
    ```

    O guia completo de setup está presente no [repositório do projeto.](https://github.com/DolphinDatabase/Cloudin-backend#readme)

    </details>

    <details id="aks-p5"><summary>AKS Cluster</summary>

    O Serviço de Kubernetes do Azure (AKS) oferece a maneira mais rápida de começar a desenvolver e implantar aplicativos nativos de nuvem no Azure, em datacenters ou na borda com pipelines internos do código para a nuvem e verificadores de integridade. Obtenha gerenciamento e governança unificados para clusters do Kubernetes locais, de borda e multinuvem. Interopere com os serviços de segurança, identidade, gerenciamento de custos e migração do Azure [\[23\]](#referências).

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

    Um "Deployment" é usado para criar e gerenciar instâncias de aplicativos em um cluster Kubernetes. Ele especifica as configurações para replicação, seleção de pods, modelo de pod e containers associados. No caso utilizado no projeto, o Deployment chamado "cloudin-midall" está sendo configurado para criar um replicaSet com o número de réplicas definido pelo valor definido no values.yaml. O container "backend" é definido com sua imagem e configurações de porta e variável de ambiente.

    Um Deployment é adequado para aplicativos que não possuem estado, ou seja, não armazenam dados persistentes. Ele gerencia a implantação de réplicas de pods de forma controlada, permitindo atualizações, rollback e dimensionamento automático. Os pods criados por um Deployment não possuem identidade única e não mantêm conexões persistentes com o armazenamento [\[24\]](#referências).

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

    Foi definido um "StatefulSet" no Kubernetes para executar uma instância do banco de dados MySQL. O StatefulSet é nomeado como "cloudin-midall-mysql" e possui uma réplica. O serviço associado ao StatefulSet também é chamado de "cloudin-midall-mysql". A definição do pod inclui um container chamado "mysql" que usa a imagem mais recente do MySQL. A porta 3306 é exposta para comunicação. Variáveis de ambiente são configuradas para definir a senha do usuário root do MySQL, o nome do banco de dados, o nome de usuário e a senha do usuário do banco de dados. Restrições de recursos são definidas para limitar o uso de CPU e memória do container.

    Um "StatefulSet" é usado para aplicativos que possuem estado e requerem identidade única e armazenamento persistente para seus pods. Ele fornece garantias de ordem e estabilidade durante a criação, atualização e exclusão dos pods. Cada pod em um StatefulSet é atribuído a um identificador exclusivo e mantém seu próprio estado persistente. Isso é especialmente útil para bancos de dados e outras aplicações que exigem armazenamento persistente e consistência de estado entre os pods [\[25\]](#referências).

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

    O Service ClusterIP é um tipo de serviço no Kubernetes que expõe um conjunto de pods para comunicação interna dentro do cluster. Ele fornece um endereço IP interno estático para o serviço, permitindo que outros recursos dentro do cluster se conectem a ele. O serviço "cloudin-midall-mysql" é definido como um Service ClusterIP. Ele mapeia a porta 3306 para o targetPort 3306 em protocolo TCP, que é o padrão para comunicação com o banco de dados MySQL. O seletor "app: cloudin-midall-mysql" garante que o Service encaminhe o tráfego para os pods que possuem a mesma etiqueta. Esse tipo de serviço é adequado para comunicação interna e não é acessível de fora do cluster [\[26\]](#referências).

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

    O Service LoadBalancer é um tipo de serviço no Kubernetes que expõe um conjunto de pods para o tráfego externo, permitindo que o aplicativo seja acessado de fora do cluster. Ele provisiona automaticamente um balanceador de carga externo, como um balanceador de carga na nuvem, que distribui o tráfego para os pods do serviço. O serviço "cloudin-midall" é definido como um Service LoadBalancer. Ele mapeia uma determinada porta, definida pelo valor ".Values.backend.ports.containerPort", para o targetPort correspondente. O protocolo especificado é TCP e o nome do serviço é "http". O seletor "app: cloudin-midall" garante que o Service encaminhe o tráfego para os pods com a mesma etiqueta. Esse tipo de serviço é adequado para expor um aplicativo para o tráfego externo, permitindo a acessibilidade de fora do cluster, através do balanceador de carga fornecido pelo ambiente de execução do Kubernetes, como um balanceador de carga na nuvem [\[27\]](#referências).

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

    O HorizontalPodAutoscaler (HPA) é um recurso do Kubernetes que permite o dimensionamento automático do número de réplicas de um Deployment ou outro objeto de escala horizontal. O HPA chamado "cloudin-midall-hpa" está configurado para dimensionar automaticamente o número de réplicas do Deployment "cloudin-midall". O número mínimo de réplicas é definido como 1 e o número máximo como 5. O HPA utiliza métricas para tomar decisões de dimensionamento. A métrica utilizada é a utilização média de CPU, onde o HPA tentará manter a utilização de CPU em torno de 80%. Com base nessa métrica, o HPA aumentará ou diminuirá o número de réplicas do Deployment para atender às demandas de tráfego. Isso permite que o cluster Kubernetes ajuste automaticamente a capacidade de processamento conforme necessário, garantindo um dimensionamento eficiente dos recursos do aplicativo.

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

    O Ingress é um recurso no Kubernetes que permite a exposição de serviços HTTP e HTTPS externamente ao cluster. O Ingress chamado "cloudin-midall" é definido com a anotação "kubernetes.io/ingress.class" configurada como "azure/application-gateway", indicando que o Ingress será gerenciado pelo Application Gateway do Azure. A especificação do Ingress inclui regras para roteamento do tráfego. No exemplo, há uma única regra que encaminha todo o tráfego HTTP que chega à raiz ("/") para o serviço chamado "cloudin-midall" na porta 5000. O campo "pathType" é definido como "Prefix", o que significa que o Ingress corresponderá a URLs que começam com o caminho especificado. O Ingress permite a configuração flexível do roteamento do tráfego externo para serviços internos no Kubernetes, fornecendo uma camada de controle de acesso e balanceamento de carga [\[29\]](#referências).

    </details>

<h3 id="aprendizados-p5">Aprendizados Efetivos</h3>
Durante o desenvolvido do projeto foi aprendizado a implantar e monitorar clusters kubernetes para produção e para testes, como realizar integração contínua utilizando Github Actions, usabilidade do serviço Amazon S3 e como realizar operações de listener para realizar de sincronização e transferência automáticas sem interferência humana. Além disso, foi realizado testes de integração e de unidade com o GoogleService, S3Service e K8SApi ao qual o autor ainda tinha dúvidas de como funcionavam que durante o semestre foram sanadas e a partir do aprendizado foi desenvolvido uma base mais sólida para futuros trabalhos.

<h3 id="demo-p5">Demonstração das Funcionalidades</h3>

Para acessar a playlist do projeto, clique [aqui](https://www.youtube.com/watch?v=AGRvBq9Xq4U&list=PLUOBqJKbljZsvHbaHWKrQ3z0l9l2Uo_f0):

[<img src="https://user-images.githubusercontent.com/74321890/228991716-687c07f9-3b6a-4cea-b855-677b51b2b20a.svg" width="60%" height="60%">](https://www.youtube.com/watch?v=AGRvBq9Xq4U&list=PLUOBqJKbljZsvHbaHWKrQ3z0l9l2Uo_f0 "Cloud-in vídeo Demonstração")

</details></h4>

## Table of technologies
<img src="https://github.com/DolphinDatabase/Cloud-In/assets/58821700/489292f1-79a2-42a9-8a06-0d739c8feebf"/>

