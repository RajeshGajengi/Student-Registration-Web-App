### Database Schema Files

The project includes comprehensive database schemas:

1. **`database_schema.sql`** - Complete schema with:
   - Users table with all fields
   - Courses and branches tables
   - User roles and permissions
   - Audit logging
   - Sample data (10 students, 6 courses, 5 branches)

2. **`simple_schema.sql`** - Minimal schema with:
   - Basic users table
   - Sample data (5 students)

3. **`DATABASE_SETUP.md`** - Complete setup guide

### MariaDB Setup (Ubuntu)

1. **Install MariaDB:**
   ```bash
   sudo apt update && sudo apt install mariadb-server -y
   ```

2. **Secure the installation:**
   ```bash
   sudo mysql_secure_installation
   ```

3. **Create database and user:**
   ```bash
   sudo mysql -u root -p
   ```
   ```sql
   CREATE DATABASE student_db;
   GRANT ALL PRIVILEGES ON student_db.* TO 'username'@'localhost' IDENTIFIED BY 'your_password';
   FLUSH PRIVILEGES;
   EXIT;
   ```
4. **Import the schema:**
   ```bash
   mysql -u username -p student_db < database_schema.sql
   ```
