1. git clone --branch v5.0.3 https://github.com/mendix/docker-mendix-buildpack.git
2. In the VS Code terminal, run the two command line below to generate the mendix-rootfs:app and the mendix-rootfs:builder images:
	docker build -t mendix-rootfs:app -f rootfs-app.dockerfile .
	docker build -t mendix-rootfs:builder -f rootfs-builder.dockerfile .



#//*-----------------------------Builder pack Master branch (working) (folder_name= atrina crud1)-----------------------*//

# Run Command (Inside: atrina crud1 )
	1. docker build --build-arg BUILD_PATH="./project" -t atrinacrud .
	2. docker compose -f ./docker-compose-postgres.yml up
	3. browser: http://localhost:8080

#Project setup

1. download buildpack for project directory:
	https://github.com/mendix/docker-mendix-buildpack/tree/master
2. create a folder new project inside it.
3. Rename the .md file to .zip and extrzct it.
	cmd: Rename-Item -Path "atrina.mda" -NewName "atrina.zip"
	extract the zip file in project dict

4. Copy postgres docer.yml file from test directory, and change the mendix image name as you want.
	a. change admin password.
	b. add pg admin in yml file

	pgadmin:
        	container_name: pgadmin2
        	image: dpage/pgadmin4
        	environment:
            		- PGADMIN_DEFAULT_EMAIL=admin@gmail.com
            		- PGADMIN_DEFAULT_PASSWORD=admin
        	ports:
            		- 5050:80
        	links:
            		- db   
        	depends_on:
            		- db 
5. add below code to : Dockerfile.rootfs.bionic

# Add PostgreSQL repository and install PostgreSQL
RUN apt-get update && \
    apt-get -y upgrade && \
    apt-get install -y curl gnupg fontconfig && \
    apt-get clean \
    echo "deb http://apt.postgresql.org/pub/repos/apt/ bullseye-pgdg main" >> /etc/apt/sources.list.d/pgdg.list && \
    curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | gpg --dearmor -o /usr/share/keyrings/postgresql-archive-bullseye.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/postgresql-archive-bullseye.gpg] http://apt.postgresql.org/pub/repos/apt bullseye-pgdg main" > /etc/apt/sources.list.d/pgdg.list && \
    apt-get update && \
    apt-get install -y postgresql-15 postgresql-client-15

6. Add Below code to: Docker file

# Expose the PostgreSQL port
EXPOSE 5432

# Start PostgreSQL service
CMD ["pgadmin", "14", "main", "start", "-w"]


7. Run YAML file
	cmd : docker build --build-arg BUILD_PATH="./project" -t atrinacrud .
	cmd: docker compose -f ./docker-compose-postgres.yml up
	browser: http://localhost:8080


postgresql://[user[:password]@][netloc][:port][/dbname]
postgres://postgres:1234@postgres:5432/atrina_db
postgres://mendix:mendix@db:5432/mendix

8. To remove imaes: docker system prune -a

#//*-----------------------------------------------------------------------------------------------------------*//

#//*-----------------------------Builder pack latest branch (working)(folder name= atrina_crud_latest_branch)-----------------------*//

# Run Commands (inside: atrina_crud_latest_branch directory) :
1. docker build -t mendix-rootfs:app -f rootfs-app.dockerfile .
2. docker build -t mendix-rootfs:builder -f rootfs-builder.dockerfile .
3. docker build  --build-arg BUILD_PATH="./project" --tag atrina_crud_lb .
4. docker compose -f ./docker-compose-postgres.yml up

# Project Setup page

1. download buildpack for project directory:
	 clone --branch v5.0.4 --config core.autocrlf=false https://github.com/mendix/docker-mendix-buildpack

2. create a folder new project inside it.
3. Rename the .md file to .zip and extrzct it.
	cmd: Rename-Item -Path "atrina.mda" -NewName "atrina.zip"
	extract the zip file in project dict

4. Copy postgres docer.yml file from test directory, and change the mendix image name as you want.
	a. change admin password.
	b. add pg admin in yml file

	pgadmin:
        	container_name: pgadmin2
        	image: dpage/pgadmin4
        	environment:
            		- PGADMIN_DEFAULT_EMAIL=admin@gmail.com
            		- PGADMIN_DEFAULT_PASSWORD=admin
        	ports:
            		- 5050:80
        	links:
            		- db   
        	depends_on:
            		- db 
5. add below code to : Dockerfile.rootfs.bionic

# Add PostgreSQL repository and install PostgreSQL
RUN apt-get update && \
    apt-get -y upgrade && \
    apt-get install -y curl gnupg fontconfig && \
    apt-get clean \
    echo "deb http://apt.postgresql.org/pub/repos/apt/ bullseye-pgdg main" >> /etc/apt/sources.list.d/pgdg.list && \
    curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | gpg --dearmor -o /usr/share/keyrings/postgresql-archive-bullseye.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/postgresql-archive-bullseye.gpg] http://apt.postgresql.org/pub/repos/apt bullseye-pgdg main" > /etc/apt/sources.list.d/pgdg.list && \
    apt-get update && \
    apt-get install -y postgresql-15 postgresql-client-15

6. Add Below code to: Docker file

# Expose the PostgreSQL port
EXPOSE 5432

# Start PostgreSQL service
CMD ["pgadmin", "14", "main", "start", "-w"]

7. Run Command:
	a. docker build -t mendix-rootfs:app -f rootfs-app.dockerfile .
	b. docker build -t mendix-rootfs:builder -f rootfs-builder.dockerfile .

8. Run YAML file
	cmd : docker build  --build-arg BUILD_PATH="./project" --tag atrina_crud_lb .
	cmd: docker compose -f ./docker-compose-postgres.yml up 
	browser: http://localhost:8080


postgresql://[user[:password]@][netloc][:port][/dbname]
postgres://postgres:1234@postgres:5432/atrina_db
postgres://mendix:mendix@db:5432/mendix

8. To remove imaes: docker system prune -a
