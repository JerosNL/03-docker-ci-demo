# Docker CI Demo

## What this is

A Python calculator app containerised with Docker and pushed to GitHub Container Registry through an Azure DevOps pipeline. This is my third hands-on DevOps project, building on Project 1 by adding containerisation.

## What the pipeline does

```
GitHub Push -> Build Docker image -> Run tests inside build -> Push to GitHub Container Registry
```

Tests run as part of the Docker build. If a test fails the image cannot be built, which means a broken image can never be pushed to the registry.

Every successful build produces two tags:
- A unique build ID tag matching the Azure DevOps run number
- `latest`, always pointing at the most recent successful build

## What I learned

- A Docker image bundles the application with everything it needs to run, so it behaves the same on any machine
- Dockerfile instruction order matters because of layer caching. Copying `requirements.txt` before the application code means pip install only reruns when dependencies change, not on every code change
- Running tests inside the Dockerfile build step means a broken image can never be pushed
- The self-hosted agent runs as NetworkService on Windows, which does not have access to Docker by default. Adding NetworkService to the local `docker-users` group fixed this
- Docker registry image paths must be entirely lowercase even if the GitHub username used for login has mixed case
- A Personal Access Token pasted anywhere outside its intended storage location should be treated as compromised and revoked immediately

## Tech used

- Docker
- Python, pytest
- GitHub Container Registry (ghcr.io)
- Azure DevOps Pipelines
- Self-hosted Windows agent

## Screenshots

![Pipeline summary](screenshots/pipeline-summary.jpg)
![GitHub package with tags](screenshots/GitHub-package-with-tags.jpg)