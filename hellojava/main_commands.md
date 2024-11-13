# Java Hello World Docker Guide

## 1. Create `HelloWorld.java`
Asimple Java program that will print "Hello, World!".
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

## 1.2 Create `manifest.mf`
The manifest file contains information about the Java program.
```manifest
Manifest-Version: 1.0
Main-Class: HelloWorld
```



## 2. Create `Dockerfile`
The Dockerfile will compile and run the Java program.
```dockerfile
FROM openjdk
WORKDIR /
COPY HelloWorld.java HelloWorld.java
COPY manifest.mf manifest.mf
RUN javac HelloWorld.java
RUN jar -cfmv HelloWorld.jar manifest.mf HelloWorld.class
CMD ["java", "-jar", "HelloWorld.jar"] 
```


## 3. Build the Docker Image
Run the following command to build the Docker image. The `-t` flag assigns a tag to the image.

```bash
docker build -t my_image .
```

## 4. Run the Docker Container
Run the container from the image you created.

```bash
docker run --name my_container my_image
```

## 5. Push to Docker Hub
Make sure you're logged into Docker Hub and tag the image for your repository.

### Login to Docker Hub
```bash
docker login
```

### Tag the Image
Replace `username` and `my_hub_repo` with your Docker Hub username and repository name.

```bash
docker tag my_image username/my_hub_repo:hello_java
```

### Push the Image
Push the tagged image to Docker Hub.

```bash
docker push username/my_hub_repo:hello_java
```

## 6. Pull and Run the Image
You can pull the image from Docker Hub on any system and run it.

### Pull the Image
```bash
docker pull username/my_hub_repo:hello_java
```

### Run the Container
Run the container without any port mapping.

```bash
docker run username/my_hub_repo:hello_java
```

