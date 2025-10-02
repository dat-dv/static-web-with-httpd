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
