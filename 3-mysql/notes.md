# MySQL Docker Hub Documentation Notes

## MYSQL Docker Image Notes

### Example 1:

```bash
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
```

**Explanation:**

- `-e`: Set environment variables for the container.
- `-d`: Run the container in detached mode. Without `-d`, the container would run in the foreground, and you'd see its logs in your terminal.

---

### Example 2:

```bash
docker run -it --network some-network --rm mysql mysql -hsome-mysql -uexample-user -p
```

**Explanation:**

- `-it`: Allocates a pseudo-TTY connected to the container’s stdin, creating an interactive bash shell in the container.
  - `-i`: Interactive mode keeps STDIN open even if not attached.
  - `-t`: Allocates a TTY.
- `-d`: Detached mode allows the container to run in the background.
- Combining `-itd` runs the container interactively and detaches it, meaning it runs in the background.
- `--rm`: Automatically removes the container when it exits. Useful for temporary containers that don’t need to persist.

**MySQL-specific options:**

- `-h`: Specifies the host to connect to (e.g., `some-mysql`). This could be another container on the `some-network` or an external hostname.
- `-u`: Specifies the username for the MySQL connection (e.g., `example-user`).
- `-p`: Prompts for the password for the MySQL user. This is entered interactively.

---

### Example 3:

```bash
docker run --name my-sql-cont -d -p 3306:3306 -p 33060:30060 -v ./sql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root mysql:8
```

**Explanation:**

- `-p`: Maps ports between the host and container. Syntax: `<host_port>:<container_port>`
- `-v`: Maps volumes between the host and container. Syntax: `<host_path>:<container_path>`

---

## Main Commands

### Run a MySQL Container

```bash
docker run --name mysql_cont -d -p 3306:3306 -p 33060:33060 -v ./sql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root mysql:8
```

### Access the Container

```bash
docker exec -it mysql_cont bash
```

### Access MySQL Shell

```bash
mysql -u root -p
```

*Password is ********`root`********.*

---

## MySQL Commands

### Show Databases

```sql
SHOW DATABASES;
```

### Use a Database

```sql
USE mysql;
```

### Show Tables

```sql
SHOW TABLES;
```

### Describe a Table

```sql
DESCRIBE user;
```

### Create a Database

```sql
CREATE DATABASE test_db;
```

### Use the New Database

```sql
USE test_db;
```

### Create a Table

```sql
CREATE TABLE address (
  ID INT NOT NULL,
  STREET_NAME VARCHAR(255),
  CLIENT INT,
  PRIMARY KEY(ID),
  FOREIGN KEY(CLIENT) REFERENCES client(CID)
);
```

### Insert Data

```sql
INSERT INTO client VALUES (1, 'Sa3d');
INSERT INTO address VALUES (102, 'add1', 1);
```

### exit MySQL Shell    

```sql
exit;
```

### exit bash shell

```bash
exit
```

**Note** : container is still running in detached mode, You can use mysql-workbench to access the container.





