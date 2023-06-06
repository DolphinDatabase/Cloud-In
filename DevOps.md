# DevOps

## Table of contents

 * [GitFlow](#gitflow)
 * [Track Issue](#track-issue)
 * [CI](#ci-continuos-integration)
 * [Tests](#tests)
 * [Monitoring](#monitoring)
 
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

## Table of technologies
<img src="https://github.com/DolphinDatabase/Cloud-In/assets/58821700/489292f1-79a2-42a9-8a06-0d739c8feebf"/>
