# Automated CI/CD Deployment with Jenkins, Docker & Docker Swarm

An end-to-end CI/CD project that automates the build, containerization, image publishing, and deployment of web applications using **Jenkins, Docker, Docker Hub, and Docker Swarm on AWS EC2**.

## Architecture

```text
                    ┌──────────────┐
                    │   Developer  │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │    GitHub    │
                    │ Source Code  │
                    └──────┬───────┘
                           │
                           ▼
                 ┌────────────────────┐
                 │      Jenkins       │
                 │    CI/CD Pipeline  │
                 │                    │
                 │ Checkout → Build   │
                 │ Push → Deploy      │
                 └─────────┬──────────┘
                           │
                           ▼
                    ┌──────────────┐
                    │  Docker Hub  │
                    │    Registry  │
                    └──────┬───────┘
                           │
                           ▼
              ┌───────────────────────────┐
              │       AWS EC2             │
              │    Docker Swarm Cluster   │
              │                           │
              │  ┌─────────┐ ┌─────────┐ │
              │  │ Manager │ │ Worker 1│ │
              │  │+ Jenkins│ │         │ │
              │  └────┬────┘ └─────────┘ │
              │       │       ┌─────────┐ │
              │       └──────►│ Worker 2│ │
              │               └─────────┘ │
              │                           │
              │  Internet | Mobile       │
              │  Banking  | Banking      │
              │  Insurance| Loan         │
              └───────────────────────────┘
```

## Project Workflow

**1. Source Management**
Application source code and pipeline configuration are maintained in GitHub.

**2. Continuous Integration**
Jenkins automatically checks out the source code and builds the Docker image for the selected application.

**3. Image Publishing**
The generated image is tagged using the Jenkins build number and pushed to Docker Hub.

Example:

```text
snehamagare/internet-banking:v4
```

**4. Container Deployment**
Jenkins deploys the application stack to the Docker Swarm cluster running on AWS EC2.

**5. Service Verification**
The pipeline verifies the deployed Swarm services after deployment.

## Applications

The project demonstrates the deployment of four containerized web applications:

| Application      | Docker Image                   | Port |
| ---------------- | ------------------------------ | ---: |
| Internet Banking | `snehamagare/internet-banking` |   81 |
| Mobile Banking   | `snehamagare/mobile-banking`   |   82 |
| Insurance        | `snehamagare/insurance`        |   83 |
| Loan             | `snehamagare/loan`             |   84 |

Each application uses an **Nginx-based Docker container**.

## Technology Stack

| Area             | Technologies         |
| ---------------- | -------------------- |
| Cloud            | AWS EC2              |
| CI/CD            | Jenkins              |
| Containerization | Docker               |
| Orchestration    | Docker Swarm         |
| Registry         | Docker Hub           |
| Version Control  | Git, GitHub          |
| Web Server       | Nginx                |
| Configuration    | Docker Compose, YAML |
| OS               | Linux                |

## Jenkins Pipeline

The Declarative Jenkins Pipeline contains the following stages:

```text
Checkout
   ↓
Build Docker Image
   ↓
Login to Docker Hub
   ↓
Push Image
   ↓
Deploy to Docker Swarm
   ↓
Verify Deployment
```

Docker Hub credentials are stored securely in **Jenkins Credentials** and are not hard-coded in the pipeline.

## Docker Swarm Infrastructure

The application is deployed on a three-node Swarm cluster:

* **Manager Node:** Controls the Swarm and runs Jenkins.
* **Worker Node 1:** Runs scheduled application containers.
* **Worker Node 2:** Runs scheduled application containers.

Docker Swarm manages service scheduling, replicas, container placement, and service availability across the cluster.

## Project Structure

```text
automated-cicd-jenkins-docker-swarm/
│
├── applications/
│   ├── internet-banking/
│   ├── mobile-banking/
│   ├── insurance/
│   └── loan/
│
├── architecture/
├── docker-compose.yml
├── Jenkinsfile
├── README.md
└── .gitignore
```

## Key DevOps Concepts Demonstrated

* CI/CD pipeline automation
* Docker image creation and versioning
* Docker Hub image publishing
* Docker Swarm cluster management
* Multi-node container deployment
* Jenkins credential management
* AWS EC2 infrastructure
* Linux administration
* Git-based source control

## Author

**Sneha Magare**
B.Tech Computer Science and Engineering

GitHub: `https://github.com/magaresneha`
