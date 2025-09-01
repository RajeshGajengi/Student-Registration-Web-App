### Prerequisites
- Docker installed
- You should have DockerHub account.

### Backend Setup 

1. **Navigate to backend directory:**
   ```bash
   cd backend
   ```
2. **Create Dockerfile for Backend:**
   ```bash
   nano dockerfile
   ```
   ```bash
    FROM maven:3.9.6-eclipse-temurin-17
    COPY . /opt/
    WORKDIR /opt
    RUN  ./mvnw clean package
    WORKDIR target/
    EXPOSE 8080
    ENTRYPOINT ["java","-jar"]
    CMD ["student-registration-backend-0.0.1-SNAPSHOT.jar"]
   ```
3. **Build the image for backend:**
   ```bash
   docker build -t backend:latest .
   ```
5. **Run the container with backend image:**
   ```bash
   docker run -d -p 8080:8080 backend:latest
   ```
6. Now you have successfully created container for backend. To cross verify backend is running or not, just copy public ip and paste in browser with port numbber 8080.(<public_ip:8080>) 

### Frontend Setup    

1. **Navigate to Frontend directory:**
  ```bash
  cd frontend
  ```
2. **Create Dockerfile for Frontend:** 
   ```bash
   nano dockerfile
   ```
   ```bash
    FROM node:24-alpine
    COPY . /opt/
    WORKDIR /opt
    ENV BACKEND=<Add-backend-public-ip>
    RUN npm install && npm run build
    RUN apk update && apk add apache2
    RUN rm -rf /var/www/localhost/htdocs/*
    RUN cp -rf dist/* /var/www/localhost/htdocs
    EXPOSE 80
    ENTRYPOINT ["httpd","-D","FOREGROUND"]
   ```
   Make sure to add backend IP in dockerfile.
   
4. **Build the image for frontend:**
   ```bash
   docker build -t frontend:latest .
   ```
5. **Run the container with frontend image:**
   ```bash
   docker run -d -p 80:80 frontend:latest
   ```
6. Now you have successfully deployed your application using Docker. Copy and paste your PublicIP on browser. It will open home page of your application.
Note: if your page shows half of the page and loading for rest, it means your backend is not running or not linked with your frontend.  


   
