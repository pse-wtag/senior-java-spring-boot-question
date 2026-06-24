# CICD Senior Technical Interview Framework

---

## 📋 Table of Contents
- [1.0. CICD](#10-cicd)
  - [Q1.1. What is continuous integration (ci)?](#q11-what-is-continuous-integration)
  - [Q1.2. What is continuous delivery (cd)?](#q12-what-is-continuous-delivery)
  - [Q1.3. What is continuous deployment?](#q13-what-is-continuous-deployment)
  - [Q1.4. What are CI/CD pipelines?](#q14-what-are-ci-cd-pipelines)
- [2.0. Kubernetes](#20-Kubernetes)
  - [Q2.1. Types of Kubernetes Deployment](#q21-types-of-kubernetes-deployment)
  - [Q2.2. Canary Deployment](#q22-canary-deployment)
  - [Q2.3. Rolling Deployment](#q23-rolling-deployment)
  - [Q2.4. Blue-Green Deployment](#q24-blue-green-deployment)
- [3.0. Github Action](#30-github-action)
  - [Q3.1. What is Github Runner?](#q31-what-is-github-runner?)
  - [Q3.2. How to check for api key leaks?](#q32-how-to-check-for-api-key-leaks)
  - [Q3.3. What is dependabot?](#q33-what-is-dependabot)
- [🎯 Candidate Final Tally Matrix](#-candidate-final-tally-matrix)
---


## 1.0 CICD

### Q1.1. What is continuous integration (ci)?
#### Target Answer
- Ensures that code changes are validated early and often through automated builds and tests, reducing errors and speeding up development.
    - **Frequent Integration:** Developers merge code changes into a shared main branch early and often.
    - **Automated Validation:** Every commit automatically triggers a build and a suite of tests to validate the new code.
    - **Early Issue Detection:** Bugs and security vulnerabilities are caught immediately, making them easier to fix while the code is still fresh in the developer's mind.
    - **Reduced Code Conflicts:** Integrating small changes constantly minimizes the risk of massive merge conflicts when multiple developers are working on the same app.
    - **Quality Control:** The automated process typically starts with static code analysis to verify quality before compiling and running further tests.

### Q1.2. What is continuous delivery (cd)?
#### Target Answer
- Process of automatically preparing tested code so it is always ready for deployment to any environment.
    - **Always "Deploy-Ready":** Automatically packages tested code so it can be pushed to production at any given moment.
    - **The CI Hand-off:** Takes over where Continuous Integration ends, bundling the application with all required dependencies and environment configs.
    - **Automated Staging:** Handles the infrastructure provisioning and automatically deploys the build to testing/staging environments.
    - **The "Manual Button" Gate:** The code is 100% prepared to go live, but pushing it to Production still requires a human trigger (this is the exact detail interviewers test to see if you confuse it with Continuous Deployment).

### Q1.3. What is continuous deployment?
#### Target Answer
- Enables teams to deliver new features faster, reduce errors, and respond to user needs in real time.
    - **Zero Human Intervention:** Code is pushed live to production automatically the exact moment it passes all tests.
    - **Rule-Based Releases:** Predefined pipeline criteria act as the ultimate gatekeeper—if the tests turn green, the code ships.
    - **Real-Time Delivery:** Shrinks the gap between a developer writing a line of code and an end-user actually touching it.
    - **Strictly Requires CI:** Impossible to execute safely without a rigorous Continuous Integration setup already catching bugs upstream.


### Q1.4. What are CI/CD pipelines?
#### Target Answer
- Automated process utilized by software development teams to streamline the creation, testing and deployment of applications.

## 2.0. Kurbernetes


### Q2.1. Types of Kubernetes Deployment
#### Target Answer
1. Canary
2. Rolling
3. Blue-Green

### Q2.2. Canary Deployment
#### Target Answer
- Technique for rolling out new features or changes to a small subset of users or servers before releasing the update to the entire system. 
- **Traffic Splitting:** Creates a new replica set for the update and routes a small percentage of live traffic to it, keeping the majority of traffic on the original, stable replica set.
- **Live Testing & Risk Mitigation:** Safely tests the new feature in a real-world production environment while minimizing the "blast radius" if something breaks.
- **Instant Rollbacks:** If the "canary" fails or issues are detected, traffic is immediately routed back to the original version with minimal user impact.
```yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp-container
        image: myapp:v1
        ports:
        - containerPort: 8080
      readinessProbe:
        httpGet:
          path: /
          port: 8080
        initialDelaySeconds: 5
        periodSeconds: 5
      livenessProbe:
        httpGet:
          path: /
          port: 8080
        initialDelaySeconds: 10
        periodSeconds: 10
```

### Q2.3. Rolling Deployment
#### Target Answer
- Strategy for updating and deploying new versions of software in a controlled and gradual manner rather than all at once.
- **Zero Downtime:** Ensures uninterrupted service to users by keeping the old version running while the new version spins up.
- **Replica Set Scaling:** Mechanically works by gradually scaling up a new replica set (the update) while simultaneously scaling down the old one.
- **Lower Risk & Easy Rollbacks:** The controlled pace minimizes the risk of widespread failures and allows for a quick rollback if the new version contains errors.
- **Final Clean-up:** Once the new version is 100% active and stable, the old replica set is completely deleted to finish the deployment.
```yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp-container
        image: myapp:v1
        ports:
        - containerPort: 8080
```

### Q2.4. Blue-Green Deployment
#### Target Answer
- Technique for releasing new versions of an application to minimise downtime and risk.
- **Twin Environments:** Maintains two identical production environments (referred to as Blue and Green) running side-by-side.
- **Active vs. Idle:** One environment (Blue) actively handles all live user traffic, while the idle one (Green) is updated with the new release.
- **Risk-Free Testing:** The new release is fully tested in the idle environment—which is an exact replica of production—without impacting actual users.
- **Instant Cutover:** Once validated, a router or load balancer instantly switches all traffic from Blue to Green, resulting in zero downtime.
- **Immediate Rollback:** If the new version fails after the switch, traffic can be instantly routed back to the original environment.
```yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
        version: blue
    spec:
      containers:
        - name: my-app
          image: myregistry/my-app:blue
          ports:
            - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

## Github

### Q3.1. What is Github Runner?
#### Target Answer
- It is a server VM that GitHub provisions to execute the steps of your workflow jobs. 
- When a workflow is triggered, GitHub spins up a fresh VM, runs all the steps defined in the job on it, and then tears it down. 
- You specify which runner to use with the 
```yaml 
runs-on:
```

### Q3.2. How to check for api key leaks?
#### 


## 🎯 Candidate Final Tally Matrix
| Question | Answer | Status ✅ ❌ | Mark |
| :--- | :--- | :---: | :---: 
| **Q1.1** | - Knew about CICD the term <br> - Did not asked more details no time | ✅ | 5/10 |
| **Q1.2** | |  |  |
| **Q1.3** | |  |  |
| **Q1.4** | |  |  |
| **Q2.1** | - Did not do kubernetes |  |  |
| **Q2.2** | - Did not do kubernetes |  |  |
| **Q2.3** | - Did not do kubernetes |  |  |
| **Q2.4** | - Did not do kubernetes |  |  |
| **Q3.1** | - Did not know what a github runner is | ❌ | 0/10 |
| **Q3.2** | - Knew how to handle api keys leaks in github <br> - using store secrets via GitHub management tools | ✅ | 10/10 |
| **Q3.3** | - Did not have time to asked about renovate |  |  |