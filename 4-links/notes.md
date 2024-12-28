# Setting Up a Spring Boot Application with MySQL Using Docker

## Step 1: Set Up the MySQL Container

1. Run the following command to create and start a MySQL container:

   ```bash
   docker run -d --name mysql_cont -p 3306:3306 -p 33060:33060 -v ./sql-data:/var/lib/sql -e MYSQL_ROOT_PASSWORD=root mysql:8
   ```

2. Access the MySQL container:

   ```bash
   docker exec -it mysql_cont bash
   ```

3. Log in to MySQL as the root user:

   ```bash
   mysql -u root -p
   ```

   - Password: `root`

4. Create a database:

   ```sql
   create database meet_mng;
   ```

5. Create a new user with access permissions:

   ```sql
   create user 'newuser'@'%' identified by 'password';
   grant all on meet_mng.* to 'newuser'@'%';
   ```

   - `%` allows access from all hosts.

6. Verify the setup by logging in with the new user:

   ```bash
   mysql -u newuser -p
   ```

   - Password: `password`

7. Use the `meet_mng` database:

   ```sql
   use meet_mng;
   ```

8. Exit the MySQL shell and container:

   ```bash
   exit
   exit
   ```

## Step 2: Set Up the Spring Boot Application

1. Clone the repository:

   ```bash
   git clone https://github.com/danielpm1982/springboot2-meeting-mng.git ./meet
   ```

2. Navigate to the project directory:

   ```bash
   cd meet
   ```

3. Checkout the branch configured for MySQL:

   ```bash
   git checkout mysql-db
   ```

4. Ensure you are using OpenJDK 1 or adjust the version in `pom.xml` if necessary.

5. Build the application:

   ```bash
   mvn clean
   mvn compile
   mvn validate
   mvn package
   ```

## Step 3: Create the Docker Image for Spring Boot

1. Copy the generated JAR file to the Dockerfile location.

2. Create a Dockerfile with the following content:

   ```dockerfile
   FROM openjdk:latest
   WORKDIR /
   EXPOSE 8080
   COPY ./springboot2-meetingmng-0.0.1-SNAPSHOT.jar /meet_app.jar
   CMD ["java", "-jar", "meet_app.jar"]
   ```

3. Build the Docker image:

   ```bash
   docker build -t meet_img:v1.0.0 .
   ```

## Step 4: Run the Spring Boot Container

1. Start the container and link it to the MySQL container:

   ```bash
   docker run --name meet_cont --link mysql_cont:mysql -p 8080:8080 \
     -e MYSQL_PORT_3306_TCP_ADDR=mysql_cont \
     -e MYSQL_ENV_MYSQL_DATABASE=meet_mng \
     -e MYSQL_ENV_MYSQL_USER=ahmedsaad \
     -e MYSQL_ENV_MYSQL_PASSWORD=ahmedsaad \
     -d meet_img:v1.0.0
   ```

2. Stop and restart the container as needed:

   ```bash
   docker stop meet_cont
   docker start -i meet_cont
   ```

   - `-i` opens the container in interactive mode.

## Step 5: Verify the Setup

1. Test that the application has populated the database with sample data.
2. Use `localhost:8080` to test adding and deleting data through the application.


