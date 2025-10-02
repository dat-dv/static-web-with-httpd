# Readme

## 1.Build basic docker image

### Example: Create basic static web with httpd service

1. Prepare docker file
2. Build docker image from docker file (command.sh)
3. Run container from docker file (command.sh)

#### Run

Run prepared command in command.sh

- bash command.sh
- open browser and check at: http://localhost:8080

#### For any error:

1. Remove container

- stop container: docker stop my-httpd-container
- remove container: docker rm my-httpd-container

2. Remove images

- docker images
- docker rmi my-httpd-image

3. Build image & run container again

- bash command.sh

## 2.Build basic with docker compose

- docker compose up -d --build
- open browser and check at: http://localhost:3010

## 3 Build with jenkins

#### Step 1: Create New Pipeline Job

1. **New Item** → **Pipeline** → Name: `static-web-deployment`

#### Step 2: Configure Pipeline

1. **Build Triggers**: ✅ GitHub hook trigger for GITScm polling
2. **Pipeline Definition**: Pipeline script from SCM
3. **SCM**: Git
   - **Repository URL**: `https://github.com/dat-dv/static-web-with-httpd.git`
   - **Credentials**: `github-creds`
   - **Branch**: `Master`
   - **Script Path**: `Jenkinsfile`

### 🔄 Pipeline Workflow

The Jenkinsfile will automatically:

1. **Trigger**: When code is pushed to Master branch or tags
2. **Checkout**: Pull latest code from GitHub
3. **Deploy**:
   - Copy files to server via `rsync`
   - Build and run Docker container
   - Verify deployment
4. **Cleanup**: Clean workspace

### 📁 Server Directory Structure

```bash
# On deployment server
/home/dat-doan/projects/jenkins/out/static-web-with-httpd/
├── Dockerfile
├── docker-compose.yml
├── public/
│   ├── index.html
│   ├── style.css
│   └── assets/
└── Jenkinsfile
```
