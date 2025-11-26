# Dockerfile Cheat Sheet (Simple & Practical)

## 1. What is a Dockerfile?
A Dockerfile is a **recipe** that Docker reads to build a **Docker image**.  
It describes step-by-step how to prepare a small, isolated environment.

---

## 2. Structure of a Dockerfile (super simple)

```
FROM base-image           # Start from an existing image
ENV ...                   # Optional settings
RUN ...                   # Install stuff
COPY ...                  # Copy files into the image
WORKDIR ...               # Set the working directory
EXPOSE ...                # Optional port
CMD [...]                 # Default command when container starts
```

---

## 3. Most common Dockerfile commands

### `FROM`
Defines the base image.
```
FROM ubuntu:22.04
FROM rocker/rstudio
FROM python:3.11
```

### `RUN`
Runs a command **while building** the image.
```
RUN apt-get update && apt-get install -y python3
RUN R -e "install.packages('shiny')"
```

### `COPY`
Copies files **from your machine** into the image.
```
COPY . /app
COPY requirements.txt /app/
```

### `WORKDIR`
Moves inside a folder in the image.
```
WORKDIR /app
```

### `ENV`
Sets environment variables.
```
ENV DEBIAN_FRONTEND=noninteractive
ENV USER=rstudio
```

### `EXPOSE`
Documents which port the container will use.
```
EXPOSE 8080
```

### `CMD`
Command executed when the container starts.
```
CMD ["python3", "main.py"]
CMD ["/init"]
```

---

## 4. Minimal Dockerfiles

### â­ Minimal Ubuntu Dockerfile
```
FROM ubuntu:22.04
RUN apt-get update
CMD ["bash"]
```

### â­ Minimal Python Dockerfile
```
FROM python:3.11
WORKDIR /app
COPY . /app
RUN pip install -r requirements.txt
CMD ["python", "main.py"]
```

### â­ Minimal R Dockerfile
```
FROM r-base
WORKDIR /app
COPY . /app
RUN R -e "install.packages('tidyverse', repos='https://cloud.r-project.org')"
CMD ["Rscript", "main.R"]
```

---

## 5. Full Dockerfile for Python (Data Science template)

```
FROM python:3.11

# Setup
WORKDIR /app
COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt

# Copy project
COPY . /app

EXPOSE 8888

CMD ["python", "main.py"]
```

---

## 6. Full Dockerfile for R + Shiny + Tidyverse

```
FROM rocker/shiny:4.3.2

# Install R packages
RUN R -e "install.packages(c('tidyverse','data.table','shinythemes'), repos='https://cloud.r-project.org')"

# Copy your Shiny app
COPY app/ /srv/shiny-server/app/

EXPOSE 3838

CMD ["/usr/bin/shiny-server"] 
```

---

## 7. Full Dockerfile for RStudio Server

```
FROM rocker/rstudio:4.3.2

# Install system deps
RUN apt-get update && apt-get install -y     libcurl4-openssl-dev     libssl-dev     libxml2-dev     && apt-get clean

# Install R packages
RUN R -e "install.packages(c('tidyverse','devtools'), repos='https://cloud.r-project.org')"

# Set default password for user 'rstudio'
ENV PASSWORD=rina123

EXPOSE 8787

CMD ["/init"]
```

---

## 8. CLI Commands (just the essentials)

### Build an image
```
docker build -t myimage .
```

### Run a container
```
docker run -it myimage bash
```

### Run with port mapping
```
docker run -d -p 8787:8787 rocker/rstudio
```

### List images + containers
```
docker images
docker ps -a
```

### Enter a running container
```
docker exec -it <name> bash
```

### Stop / start / remove
```
docker stop <name>
docker start <name>
docker rm <name>
```

### Clean unused
```
docker system prune
```
