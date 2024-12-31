```markdown
# MySQL Docker Hub Documentation Notes 🐳

## 1️⃣ Running a MySQL Container

**Example Command**:  
```bash
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
```

### Explanation:
- `-e`: Set environment variables for the container.
- `-d`: Run the container in detached mode. Without `-d`, the container runs in the foreground, showing logs in your terminal.

---

## 2️⃣ Connecting to MySQL Using Docker

**Example Command**:  
```bash
docker run -it --network some-network --rm mysql mysql -hsome-mysql -uexample-user -p
```

### Explanation:
- `-it`: 
  - Allocates a pseudo-TTY and connects it to the container’s stdin.
  - Creates an interactive bash shell in the container.  
  - `-it` combines `--interactive` and `--tty`, taking you directly inside the container.

- `-d` (detach mode): 
  - Runs the container in the background and detaches from it.
  - Combined `-itd` options allow the container to run interactively and detach simultaneously.

- `--rm`: Automatically removes the container after it exits. Useful for temporary containers.

- `-h`: Specifies the host to connect to (e.g., `some-mysql`).  
- `-u`: Specifies the username for the MySQL connection (e.g., `example-user`).  
- `-p`: Prompts for the MySQL user password interactively.

---

## 3️⃣ Setting Up MySQL with Custom Configurations

**Example Command**:  
```bash
docker run --name my-sql-cont -d -p 3306:3306 -p 33060:33060 -v ./sql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root mysql:8
```

### Explanation:
- `-p`: Maps ports `<host_port>:<container_port>`.  
- `-v`: Maps volumes `<host_path>:<container_path>`.

**Additional Commands**:
1. Access the container:
   ```bash
   docker exec -it mysql_cont bash
   ```
2. Log in to MySQL:
   ```bash
   mysql -u root -p
   ```
   (Password is `root`)

**Common SQL Commands**:
```sql
SHOW DATABASES;
USE mysql;
SHOW TABLES;
DESCRIBE user;

CREATE DATABASE test_db;
USE test_db;

CREATE TABLE address (
    ID INT NOT NULL,
    STREET_NAME VARCHAR(255),
    CLIENT INT,
    PRIMARY KEY (ID),
    FOREIGN KEY (CLIENT) REFERENCES client(CID)
);

INSERT INTO client VALUES (1, 'Sa3d');
INSERT INTO address VALUES (102, 'add1', 1);
```

---

## 4️⃣ Testing the Connection via MySQL Workbench

- Use MySQL Workbench to connect to your running MySQL container.  
- Test your setup and ensure your database and tables are accessible.
