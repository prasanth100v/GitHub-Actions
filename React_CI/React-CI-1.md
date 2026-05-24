# List of React-CI
1. odeBuild + Sonarqube + Trivy + Docker build + Docker run
   
```yaml
name: CICD pipeline 2 (ReactJS)

on:
  push:
    branches: ["main"]
  workflow_dispatch:

jobs:
  build:
    runs-on: self-hosted

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 18
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Build React app
        run: npm run build

      - name: Upload build artifact
        uses: actions/upload-artifact@v4
        with:
          name: react-build
          path: dist/

  sonarqube:
    runs-on: self-hosted
    needs: build

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: SonarQube Scan
        uses: sonarsource/sonarqube-scan-action@v2
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
          SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}
        with:
          args: |
            -Dsonar.projectKey=vite-react-app
            -Dsonar.projectName=Vite_React_App
            -Dsonar.sources=src
            -Dsonar.exclusions=**/node_modules/**,**/dist/**
            -Dsonar.sourceEncoding=UTF-8

      - name: SonarQube Quality Gate
        uses: sonarsource/sonarqube-quality-gate-action@v1.1.0
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
          SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}
        timeout-minutes: 5

  security-check:
    runs-on: self-hosted
    needs: sonarqube

    steps:
      - uses: actions/checkout@v4

      - name: Trivy filesystem scan
        run: trivy fs --exit-code 1 --severity HIGH,CRITICAL .

  docker-build:
    runs-on: self-hosted
    needs: security-check

    steps:
      - name: Checkout source code
        uses: actions/checkout@v4

      # ✅ THIS STEP SOLVES YOUR ERROR
      - name: Download build artifact
        uses: actions/download-artifact@v4
        with:
          name: react-build
          path: dist

      - name: Docker login
        run: |
          echo "${{ secrets.DOCKER_PASSWORD }}" | docker login \
            -u "${{ secrets.DOCKER_USERNAME }}" \
            --password-stdin

      - name: Build Docker image
        run: |
          IMAGE_NAME=prasanth100v/prasanth-poultry
          IMAGE_TAG=$(git rev-parse --short HEAD)

          docker build -t $IMAGE_NAME:$IMAGE_TAG .
          docker tag $IMAGE_NAME:$IMAGE_TAG $IMAGE_NAME:latest

      - name: Trivy image scan (HIGH, CRITICAL)
        run: |
          IMAGE_NAME=prasanth100v/prasanth-poultry
          IMAGE_TAG=$(git rev-parse --short HEAD)

          trivy image --exit-code 1 --severity HIGH,CRITICAL \
          $IMAGE_NAME:$IMAGE_TAG

      - name: Push Docker image
        run: |
          IMAGE_NAME=prasanth100v/prasanth-poultry
          IMAGE_TAG=$(git rev-parse --short HEAD)

          docker push $IMAGE_NAME:$IMAGE_TAG
          docker push $IMAGE_NAME:latest

      - name: Remove existing container (if any)
        run: docker rm -f my_react_container || true

      - name: Run Docker container
        run: |
          IMAGE_NAME=prasanth100v/prasanth-poultry
          IMAGE_TAG=$(git rev-parse --short HEAD)

          docker run -d \
            --name my_react_container \
            -p 3000:80 \
            $IMAGE_NAME:$IMAGE_TAG
            ```
