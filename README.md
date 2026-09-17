# Sausage Store

![image](https://user-images.githubusercontent.com/9394918/121517767-69db8a80-c9f8-11eb-835a-e98ca07fd995.png)

## Technologies used

* Frontend – TypeScript, Angular.
* Backend  – Java 16, Spring Boot, Spring Data.
* Database – H2.

## Installation guide

### Backend

Install Java 16 and maven and run:

```bash
cd backend
mvn package
cd target
java -jar sausage-store-0.0.1-SNAPSHOT.jar
```

### Frontend

Install NodeJS and npm on your computer and run:

```bash
cd frontend
npm install
npm run build
npm install -g http-server
sudo http-server ./dist/frontend/ -p 80 --proxy http://localhost:8080
```

Then open your browser and go to [http://localhost](http://localhost)

## How to

### Create docker images

#### backend

```bash
docker build --build-arg VERSION=1.0.0 -t akurnenkov/sausage_store_backend:1.0.0 .
```

#### backend-report

```bash
docker build -t akurnenkov/sausage_store_backend_report:1.0.0 .
```

#### frontend

```bash
docker build -t akurnenkov/sausage_store_frontend:1.0.0 .
```

### Find migration scripts

```bash
cloud-services-engineer-sausage-store-project-sem2\backend\src\main\resources\db\migration
```

### deploy app via helm

1. build dependency

    ```bash
    helm dependency build sausage-store-chart
    ```

2. linting

    ```bash
    helm lint ./sausage-store-chart
    ```

3. templating

    ```bash
    helm template ./sausage-store-chart
    ```

4. deploy app

    ```bash
    helm upgrade -i sausage-store ./sausage-store-chart --wait --cleanup-on-fail --timeout 10m
    ```

5. delete app

    ```bash
    helm uninstall sausage-store
    ```
