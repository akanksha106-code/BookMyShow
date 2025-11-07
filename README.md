# Book My Show — React Sample App

A compact Book My Show clone built with React for learning and demonstration purposes. The app demonstrates component-based UI, simple routing, and state management for booking and ticketing workflows.

## Features
- Browse movies and view seating layout
- Select seats and book tickets
- View booking history and generated tickets
- Sample CI/CD and containerization configs included

## Tech stack
- Frontend: React (Create React App)
- Container: Docker
- CI/CD: Jenkins (sample files)

## Quick start

Prerequisites:
- Node.js (14+)
- npm or yarn
- Docker (optional)

Install and run locally:
1. Open the frontend folder:
   - [bookmyshow-app/package.json](bookmyshow-app/package.json)
2. Install dependencies:
   ```sh
   cd bookmyshow-app
   npm install
   ```
3. Start the development server:
   ```sh
   npm start
   ```
4. Access the app at `http://localhost:3000`

## Docker

To build and run the app in a container:

1. Build the Docker image:
   ```sh
   docker build -t bookmyshow-app .
   ```
2. Run the container:
   ```sh
   docker run -p 3000:3000 bookmyshow-app
   ```
3. Access the app at `http://localhost:3000`

## CI/CD

Sample Jenkins pipeline and configuration files are included for demonstration purposes. Adjust the configurations as needed for your environment.

## DevOps & Deployment (Detailed)

This project includes example DevOps workflows to build, test, secure, containerize and deploy the frontend with automated pipelines. Below are the main operations, where to find related files in the repo, and the standard commands used.

1) CI/CD pipeline (Jenkins)
- Files: Jenkinsfile, any Jenkinsfile.* or pipeline scripts in repo root.
- Pipeline stages:
  - Checkout source
  - Install dependencies (npm ci)
  - Lint (npm run lint)
  - Unit tests (npm test) and coverage gating
  - Build production bundle (npm run build)
  - Build container image (docker build)
  - Run container image scan (Trivy/Snyk)
  - Push image to registry (Docker Hub / ACR / ECR)
  - Deploy to environment (kubectl / Helm)
  - Notify stakeholders (Slack/email)
- Pipeline best practices used: parallel lint/tests, artifact retention, build-numbered tags, and failure gates for security and test coverage.

2) Build & Test
- Commands used in CI:
  - npm ci
  - npm run lint
  - npm test -- --coverage
  - npm run build
- Test types: unit tests (Jest), optional end-to-end (Cypress) if included.

3) Containerization & Image Management
- Dockerfile: bookmyshow-app/Dockerfile
- Image tagging policy: use CI build number or commit SHA (e.g., myregistry/bookmyshow:${BUILD_NUMBER} or :<git-sha>)
- Example:
  - docker build -t myregistry/bookmyshow:${BUILD_NUMBER} .
  - docker push myregistry/bookmyshow:${BUILD_NUMBER}

4) Image Registry & Artifact Storage
- Supported registries: Docker Hub, Azure Container Registry (ACR), Amazon ECR, GCR.
- Recommended: use private registry for staging/prod and configure pipeline credentials securely.

5) Orchestration & Deployment
- K8s manifests or Helm charts: check k8s/ or deployment.yml, service.yml in repo
- Deployment strategy: rolling updates by default; optional blue/green or canary via Helm or additional tooling
- Manual deploy example:
  - kubectl set image deployment/bookmyshow bookmyshow=myregistry/bookmyshow:${BUILD_NUMBER} -n staging
  - kubectl rollout status deployment/bookmyshow -n staging
- Rollback:
  - kubectl rollout undo deployment/bookmyshow -n staging

6) Infrastructure-as-Code (optional)
- If present: Terraform / ARM / CloudFormation scripts to provision infra (load balancers, registries, k8s cluster)
- Use separate IaC repo or folder and run plan/apply from pipeline with locked state.

7) Secrets & Configuration
- Do NOT commit secrets. Use:
  - Kubernetes Secrets or HashiCorp Vault for production
  - Pipeline credentials / secret stores in Jenkins for build-time access
- Use ConfigMaps or environment variables for non-sensitive config.

8) Security & Scanning
- Image scanning: Trivy/Anchore in pipeline
- Dependency scanning: npm audit, Snyk
- Static analysis & linting enforced in CI

9) Monitoring, Logging & Alerting
- Recommendations:
  - Metrics: Prometheus + Grafana dashboards
  - Logs: EFK/ELK stack or cloud logging (CloudWatch/Log Analytics)
  - Alerts: Slack/email on failed deploys, high error rates or SLO breaches

10) Backups, DB migrations & Rollback
- Database: take snapshots before running migrations; use migration scripts with rollback support
- Keep release artifacts to allow fast re-deploy of previous stable images

11) Notifications & Observability in CI
- Pipeline sends build/deploy status to Slack or email and stores logs/artifacts for auditing.

12) Example manual commands (CI-adaptable)
```bash
cd bookmyshow-app
npm ci
npm run lint
npm test -- --coverage
npm run build

# Build & push image (replace placeholders)
docker build -t myregistry/bookmyshow:${BUILD_NUMBER:-latest} .
docker tag myregistry/bookmyshow:${BUILD_NUMBER:-latest} myregistry/bookmyshow:latest
docker push myregistry/bookmyshow:${BUILD_NUMBER:-latest}

# Deploy to Kubernetes
kubectl apply -f k8s/deployment.yml -n staging
kubectl set image deployment/bookmyshow bookmyshow=myregistry/bookmyshow:${BUILD_NUMBER:-latest} -n staging
kubectl rollout status deployment/bookmyshow -n staging

# Rollback
kubectl rollout undo deployment/bookmyshow -n staging
```

Files referenced:
- Dockerfile: bookmyshow-app/Dockerfile
- Jenkins pipeline(s): Jenkinsfile (root)
- K8s manifests: deployment.yml, service.yml (k8s/ or repo root)
- Tests: bookmyshow-app/src/*.test.*

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.