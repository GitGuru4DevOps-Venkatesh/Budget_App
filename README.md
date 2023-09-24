To build a Dockerfile for deploying a Ruby on Rails application with PostgreSQL running in separate containers, you can follow these steps using the GitHub repository you provided.

Before you begin, make sure you have Docker installed on your system.

Here are the steps:

Let's go through the steps in more detail.

### Step 1: Clone the Repository

```bash
git clone https://github.com/GitGuru4DevOps-Venkatesh/Budget_App.git
```

This command clones the GitHub repository containing your Ruby on Rails application code to your local machine.

### Step 2: Create a Dockerfile for the Rails Application

Inside the cloned repository's root directory, create a Dockerfile named `Dockerfile-rails`. You can use a text editor to create and edit this file. The Dockerfile defines how to build the Docker image for your Rails application.

Here's an example of a Dockerfile for a Rails application:

```Dockerfile
# Use an official Ruby runtime as a parent image
FROM ruby:2.7

# Set the working directory in the container
WORKDIR /app

# Copy the Gemfile and Gemfile.lock into the container
COPY Gemfile Gemfile.lock ./

# Install Rails dependencies
RUN bundle install

# Copy the rest of the application code into the container
COPY . .

# Expose port 3000 to the outside world
EXPOSE 3000

# Start the Rails application
CMD ["rails", "server", "-b", "0.0.0.0"]
```

This Dockerfile specifies how to set up a Ruby environment, install Rails dependencies, and start the Rails application.

### Step 3: Create a Dockerfile for PostgreSQL

Create another Dockerfile named `Dockerfile-postgres` in the same directory. This Dockerfile defines how to build the Docker image for the PostgreSQL database container:

```Dockerfile
# Use an official PostgreSQL image as a parent image
FROM postgres:13

# Set the PostgreSQL database user and password
ENV POSTGRES_USER=myuser
ENV POSTGRES_PASSWORD=mypassword

# Create a database named 'myapp'
ENV POSTGRES_DB=myapp
```

This Dockerfile uses an official PostgreSQL image and sets environment variables for the database user, password, and the name of the database.

### Step 4: Build the Docker Images

In your terminal, navigate to the root directory of the cloned repository where you have the Dockerfiles, and build the Docker images for both the Rails application and PostgreSQL:

```bash
docker build -f Dockerfile-rails -t myrailsapp .
docker build -f Dockerfile-postgres -t mypostgresdb .
```

Replace `myrailsapp` and `mypostgresdb` with your preferred image names.

### Step 5: Create a Docker Network

Create a Docker network to enable communication between the Rails application and the PostgreSQL container:

```bash
docker network create myapp_network
```

This network will allow the containers to connect to each other.

### Step 6: Run the PostgreSQL Container

Start the PostgreSQL container using the Docker network you created:

```bash
docker run --name mypostgresdb --network myapp_network -d mypostgresdb
```

This command runs the PostgreSQL container as a daemon with the specified name and connects it to the `myapp_network` network.

### Step 7: Run the Rails Application Container

Finally, run the Rails application container and link it to the PostgreSQL container through the Docker network:

```bash
docker run --name myrailsapp --network myapp_network -p 3000:3000 -d myrailsapp
```

This command runs the Rails application container as a daemon, maps port 3000 from the container to port 3000 on your local machine, and connects it to the `myapp_network` network.

### Step 8: Access the Rails Application

You can access your Ruby on Rails application by opening a web browser and navigating to `http://localhost:3000`. This will connect to the Rails application running in the Docker container.

That's it! You've successfully created and configured Docker containers for your Ruby on Rails application and PostgreSQL database. They run independently in separate containers and communicate through the Docker network.
