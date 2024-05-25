# README

## Introduction

This guide provides instructions for setting up a local WordPress development environment using Docker. It includes a MariaDB database, phpMyAdmin for database management, and WordPress. The setup uses Docker Compose for easy configuration and management.

## Prerequisites

- Docker Desktop (version 4.26.1 recommended for compatibility)
- Docker Compose

## Setup Instructions

### Step 1: Clone the Repository

First, clone the repository containing the Docker Compose configuration:

```bash
git clone <repository-url>
cd <repository-directory>
```

### Step 2: Create a .env File

Create a `.env` file in the root directory of your project with the following properties. Replace `<something>` with your actual values:

```env
MYSQL_DATABASE=<something>
MYSQL_USER=<something>
MYSQL_PASSWORD=<something>
MYSQL_ROOT_PASSWORD=<something>
```

### Step 3: Run Docker Compose

Run the following command to start the services:

```bash
docker-compose up -d
```

### Step 4: Access the Services

- **WordPress**: Open your browser and go to [http://localhost:8080](http://localhost:8080)
- **phpMyAdmin**: Open your browser and go to [http://localhost:8081](http://localhost:8081)

### Step 5: Stopping the Services

To stop the services, run:

```bash
docker-compose down
```

### For permission issues 

```bash
sudo chmod -R a+w wp-boilerplate/
```

## Additional Resources

For a video walkthrough of this setup, you can refer to the following video: [YouTube Video](https://www.youtube.com/watch?v=gEceSAJI_3s).

## Troubleshooting

If you encounter issues, ensure you are using Docker Desktop version 4.26.1 as newer versions might have compatibility issues. Refer to the provided video for additional troubleshooting tips.

## Conclusion

This guide should help you set up a local WordPress development environment using Docker. If you have any questions or need further assistance, please refer to the resources provided or seek help from the Docker and WordPress communities.