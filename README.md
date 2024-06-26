 # Full-Stack Application with Docker

This repository contains the Docker setup for a full-stack application consisting of a React.js front-end application, a Node.js backend API, and a PostgreSQL database. The Dockerfiles and Docker Compose file provided here allow you to containerize the entire application, making it easily deployable and scalable using Docker containers.

## Directory Structure

		Dockerizing-node/
		│
		├── front_app/
		│ ├── Dockerfile
		│ └── (other frontend files)
		│
		├── app_back/
		│ ├── Dockerfile
		│ └── (other backend files)
		│
		└── docker-compose.yml
app_back/Dockerfile

		FROM node:latest
		WORKDIR /app
		COPY package.json ./
		RUN npm install
		COPY . .
		EXPOSE 3000

		CMD ["npm" , "start"]

front/Dockerfile

		FROM node:latest
		WORKDIR /app
		COPY package.json ./
		RUN npm install
		COPY . .
		EXPOSE 8000
		CMD ["npm", "start"]
	

Docker Compose
docker-compose.yml

	version: '3'
	services:
	  frontend:
	    build:
	      context: ./front_app
	      dockerfile: Dockerfile
	    container_name: front-end
	    networks:
	      - node-network
	    ports:
	      - "8001:8000"
	    networks:
	      - node-network
	  backend:
	    build:
	      context: ./app_back
	      dockerfile: Dockerfile
	    container_name: back-end
	
	    ports:
	      - "8001:3000"
	    depends_on:
	      - database
	    environment:
	      - POSTGRES_HOST=postgres
	      - POSTGRES_PORT=5432
	      - POSTGRES_USER=amina
	      - POSTGRES_PASSWORD=PS1234
	      - POSTGRES_DB=databaseNj
	
	  database:
	    image: postgres:latest
	    container_name: database
	   # ports:
	   #   - "5432:5432"
	    environment:
	      - POSTGRES_USER=amina
	      - POSTGRES_PASSWORD=PS1234
	      - POSTGRES_DB=databaseNj
	    volumes:
	      - pgdata:/var/lib/postgresql/data
	    networks:
	      - node-network
	volumes:
	  pgdata:
	
	networks:
	  node-network:
	    driver: bridge


----------------------------------------------------------------------------------------------------------------------

Remember, always avoid mapping the database port to the host machine to prevent unwanted access to the database. 
  Instead, ensure that your database container is accessible only within the Docker network.

Always mount volumes to persist data. This ensures that even if the container is down, your data remains intact.

Utilize virtual networks to isolate your containers and control communication between them effectively.

Consider using Docker plugins for encrypting your data volumes to enhance security and protect sensitive information.

