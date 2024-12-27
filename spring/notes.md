### Steps to Set Up, Deploy, and Run the Project

1. **Clone the Repository**  
   ```bash
   git pull https://github.com/danielpm1982/springboot2-meeting-mng.git
   ```

2. **Install Maven (if not already installed)**  
   ```bash
   sudo apt install maven
   ```

3. **Build and Package the Project**  
   - Clean the project:  
     ```bash
     mvn clean
     ```  
   - Compile the code:  
     ```bash
     mvn compile
     ```  
   - Validate the project:  
     ```bash
     mvn validate
     ```  
   - Package the application:  
     ```bash
     mvn package
     ```

4. **Dockerize the Application**  
   Create a `Dockerfile` with the following content:  
   ```dockerfile
   FROM openjdk:latest
   WORKDIR /
   EXPOSE 8080
   COPY ./springboot2-meetingmng-0.0.1-SNAPSHOT.jar /meeting-app.jar
   CMD ["java","-jar","meeting-app.jar"]
   ```

5. **Build the Docker Image**  
   ```bash
   docker build -t meeting-mng .
   ```

6. **Tag the Docker Image**  
   ```bash
   docker tag meeting-mng:latest ahmedsaad56/my_hub_repo:meet
   ```

7. **Log in to Docker Hub**  
   ```bash
   docker login
   ```

8. **Push the Docker Image to Docker Hub**  
   ```bash
   docker push ahmedsaad56/my_hub_repo:meet
   ```

9. **Pull the Docker Image from Docker Hub**  
   ```bash
   docker pull ahmedsaad56/my_hub_repo:meet
   ```

10. **Run the Application Using Docker**  
    ```bash
    docker run -it -p 8080:8080 ahmedsaad56/my_hub_repo:meet
    ```