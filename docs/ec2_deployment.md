### Prerequisites
- Java 17 or higher
- npm and Node.js 16 or higher
- Maven (or use the included Maven wrapper)

## 1. Deployment with AWS without external Database.
 
### Backend Setup 

1. **Navigate to backend directory:**
   ```bash
   cd backend
   ```

2. **Build the application:**
   ```bash
   # Make Maven wrapper executable (first time only)
   chmod +x mvnw
   
   # Build the project
   ./mvnw clean package
   ```

3. **Run the application:**
   ```bash
   # Using Maven
   ./mvnw spring-boot:run
   
   # Or using the JAR file
   java -jar target/student-registration-backend-0.0.1-SNAPSHOT.jar
   ```

The backend will start on `http://<public_ip>:8080`

### Frontend Setup

1. **Navigate to frontend directory:**
   ```bash
   cd frontend
   ```
2. **export backend ip for .env**
    ```bash
    export BACKEND=<public_ip>  # or copy paste public ip in .env for safety
    ```	

3. **Install dependencies:**
   ```bash
   npm install
   ```

4. **Start the development server:**
   ```bash
   npm run build
   ```

The frontend will start on `http://<public_ip>:5173`
