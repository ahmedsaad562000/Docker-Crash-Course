MY-sql docker hub documentaiton notes

1- example:  docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag

- `-e` = Set environment variables for the container.
- `-d` = Run the container in detached mode, Without `-d`, the container would run in the foreground, and you'd see its logs in your terminal.

2- example: docker run -it --network some-network --rm mysql mysql -hsome-mysql -uexample-user -p

The -it instructs Docker to allocate a pseudo-TTY connected to the container’s stdin; creating an interactive bash shell in the container.

-it is short for --interactive + --tty. When you docker run with this command it takes you straight inside the container.

-d is short for --detach, which means you just run the container and then detach from it. Essentially, you run container in the background.

So if you run the Docker container with -itd, it runs both the -it options and detaches you from the container. As a result, your container will still be running in the background even without any default app to run.

--rm

Automatically removes the container when it exits. This is useful for temporary containers that don’t need to persist after use.


-h: Specifies the host to connect to (some-mysql). This could be another container on the some-network or an external hostname.
-u: Specifies the username to use for the MySQL connection (example-user).
-p: Prompts for the password for the MySQL user. You’ll need to enter this interactively after running the command.

3- example: docker run --name my-sql-cont -d -p 3306:3306 -p 33060:30060 -v ./sql-data:/var/lib/mys

-p: ports mapping <host_port>:<container_port>
-v: volume mapping <host_path>:<container_path>

coomands
docker run --name mysql_cont -d -p 3306:3306 -p 33060:33060 -v ./sql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root mysql:8
docker exec -it mysql_cont bash
mysql -u root -p (password is root)


show databases;
use mysql;
show tables;

describe user;
create database test_db;
use test_db
create table address(ID int NOT NULL,STREET_NAME varchar(255),CLIENT int,PRIMARY KEY(ID),FOREIGN KEY(CLIENT) references client(CID));
insert into client values(1,'Sa3d');
insert into address values(102,'add1',1);

test your connection from mysql workbench





