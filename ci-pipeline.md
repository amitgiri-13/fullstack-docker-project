# Docker Build & Push GitHub Actions Workflow

This workflow allows you to **manually build and push Docker images** for your project’s frontend or backend to Docker Hub using GitHub Actions.

---

## Features

* Manual trigger via `workflow_dispatch`.
* Build Docker images from the `frontend` or `backend` directories.
* Push images to Docker Hub.
* Supports custom image names and tags.

---

## Workflow Inputs

| Input        | Description                                                           | Required | Default             |
| ------------ | --------------------------------------------------------------------- | -------- | ------------------- |
| `image_name` | Name of the Docker image/repository                                   | Yes      | `pingpong-frontend` |
| `image_tag`  | Docker image tag                                                      | Yes      | `latest`            |
| `directory`  | Working directory containing the Dockerfile (`frontend` or `backend`) | Yes      | `frontend`          |

---

##  Secrets Required

Before using the workflow, add the following **secrets** in your GitHub repository:

* `DOCKERHUB_USERNAME` – Your Docker Hub username.
* `DOCKERHUB_PAT` – Docker Hub access token (not your password).

---

##  Usage

1. **Push the workflow** to `.github/workflows/docker-build.yml`.
2. Go to **GitHub → Actions → Docker Build & Push**.
3. Click **Run workflow**.
4. Fill in the inputs:

   * `image_name`: e.g., `pingpong-frontend` or `pingpong-backend`
   * `image_tag`: e.g., `latest` or `v1.0.0`
   * `directory`: `frontend` or `backend`
5. Click **Run workflow**.

The workflow will:

1. Checkout the repository code.
2. Log in to Docker Hub using the provided credentials.
3. Build the Docker image from the specified directory.
4. Push the image to Docker Hub with the given name and tag.

---

## Example

**Build frontend image:**

* `image_name`: `pingpong-frontend`
* `image_tag`: `latest`
* `directory`: `frontend`

**Build backend image:**

* `image_name`: `pingpong-backend`
* `image_tag`: `v1.0.0`
* `directory`: `backend`

---

## Workflow YAML

```yaml
name: Docker Build & Push

on: 
  workflow_dispatch:
    inputs:
      image_name:
        description: "Docker image/repo name"
        required: true 
        default: "pingpong-frontend"
      image_tag:
        description: "Docker image tag"
        required: true 
        default: "latest"
      directory:
        description: "Working directory: [frontend | backend]"
        required: true
        default: "frontend"

jobs:
  build-and-push:
    runs-on: ubuntu-latest 

    steps:
      - name: Checkout code 
        uses: actions/checkout@v4 

      - name: Log in to Docker Hub 
        uses: docker/login-action@v3 
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_PAT }}
        
      - name: Build and push Docker image 
        uses: docker/build-push-action@v5 
        with:
          context: ${{ github.event.inputs.directory }}
          push: true 
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/${{ github.event.inputs.image_name }}:${{ github.event.inputs.image_tag }}
```

---

## Pre-Built Images

For convenience, pre-built images are already available at:

* Frontend: `amitgiri13/pingpong-frontend:1.0.0`
* Backend: `amitgiri13/pingpong-backend:1.0.0`

You can pull them directly using:

```bash
docker pull amitgiri13/pingpong-frontend:1.0.0
docker pull amitgiri13/pingpong-backend:1.0.0
```

---

