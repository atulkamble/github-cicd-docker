**⚙️ GitHub Actions CI/CD with Docker** project to:

* 🔨 **Build** a Docker image
* ☁️ **Push** it to DockerHub
* 🚀 **Deploy** on push to `main`

---

## 📁 Project Structure:

```
docker-ci-cd-github-actions/
├── app/
│   └── index.js
├── Dockerfile
├── .github/
│   └── workflows/
│       └── docker.yml
└── README.md
```

---

### 🐳 Dockerfile (Node.js Sample App)

```Dockerfile
# Use official Node.js image
FROM node:18

# Create app directory
WORKDIR /usr/src/app

# Copy app code
COPY app/ .

# Install dependencies (optional if you have a package.json)
RUN npm install express

# Expose port
EXPOSE 3000

# Run app
CMD ["node", "index.js"]
```

---

### 📦 `app/index.js`

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('🚀 Hello from Docker + GitHub Actions!');
});

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`App running on http://localhost:${PORT}`);
});
```

---

### ⚙️ `.github/workflows/docker.yml`

```yaml
name: Build and Push Docker Image

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: 🛒 Checkout code
      uses: actions/checkout@v3

    - name: 🔐 Log in to DockerHub
      uses: docker/login-action@v3
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}

    - name: 🔨 Build Docker image
      run: docker build -t ${{ secrets.DOCKER_USERNAME }}/ci-cd-demo:latest .

    - name: ☁️ Push Docker image
      run: docker push ${{ secrets.DOCKER_USERNAME }}/ci-cd-demo:latest
```

---

### 🗝️ GitHub Secrets Required

Go to **Repo → Settings → Secrets → Actions** and add:

* `DOCKER_USERNAME`: your DockerHub username
* `DOCKER_PASSWORD`: your DockerHub password or [access token](https://hub.docker.com/settings/security)

---
